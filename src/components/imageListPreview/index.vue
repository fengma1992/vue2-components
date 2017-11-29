/**
* Created by dujianhao on 2017/9/22.
*/
<template>
    <div class='image-preview-view' v-if='showPreview'>
        <div class="mask" @click="toggleShow(false)" v-if="!showFullImg"></div>
        <div class="preview" v-if="!showFullImg">
            <div class="img-container">
                <i v-if="showLeftArrow" class="icon icon-arrow-left" @click="goPre"></i>
                <img class="preview-img" :style="toggleDisplay" :src='selectedImgUrl' @click="showFullImg=true"/>
                <i v-if="showRightArrow" class="icon icon-arrow-right" @click="goNext"></i>
            </div>
            <div v-if="imgListLength > 1" class="img-list">
                <div v-for="(item, index) in thumbImgList"
                     :class='["img-item", {"selected-item": index + offset === currentIndex}]'
                     @click="setCurrentIndex(index + offset)">
                    <img :src="item.url">
                </div>
            </div>
        </div>
        <div class="full-img-container" v-if="showFullImg" @click="showFullImg=false">
            <img :src="selectedImgUrl">
            <i class='icon icon-close' @click="showFullImg=false"></i>
        </div>
    </div>
</template>

<script>
    
    // 缩略图个数
    let THUMB_IMG_LENGTH = 9;
    export default {
        name: 'image-list-preview',
        data() {
            return {
                // 当前显示的大图index
                currentIndex: 0,
                offset: 0,
                /**
                 * 图片地址列表
                 * {
                 *  url: '',
                 *  index: Number
                 * }
                 */
                imgList: [],
                thumbImgList: [],
                showPreview: false,
                showFullImg: false,
                toggleDisplay: '',
            };
        },
        computed: {
            imgListLength() {
                return this.imgList ? this.imgList.length : 0;
            },
            selectedImgUrl() {
                return this.imgListLength > 0 && this.imgList[this.currentIndex] ? this.imgList[this.currentIndex].url : '';
            },
            showLeftArrow() {
                return (this.imgListLength > 0) ? (this.currentIndex > 0) : false;
            },
            showRightArrow() {
                return (this.imgListLength > 0) ? (this.currentIndex < this.imgListLength - 1) : false;
            }
        },
        watch: {
            /**
             * 图片未加载完成而进行图片切换时，存在链接更改而图片显示为老图片bug，切换display状态解决
             */
            selectedImgUrl() {
                this.toggleDisplay = 'display: none';
                this.$nextTick(() => { this.toggleDisplay = ''; });
            }
        },
        methods: {
            /**
             * 设置图片源与预览大图index
             */
            setImageList(imgList, currentIndex = 0) {
                this.imgList = imgList.slice(0);
                if (currentIndex < 0 || currentIndex > this.imgList.length - 1) {
                    currentIndex = 0;
                }
                this.setCurrentIndex(currentIndex);
                this.toggleShow(true);
            },
            /**
             * 设置预览大图index
             * @param index
             */
            setCurrentIndex(index) {
                this.currentIndex = index;
                this.thumbImgList = this.getThumbImgList();
            },
            /**
             * 禁用/启用 页面滚动
             * @param flag
             */
            toggleBodyScroll(flag = true) {
                document.body.style.overflow = flag ? 'auto' : 'hidden';
            },
            /**
             * 设置显隐
             * @param flag
             */
            toggleShow(flag) {
                this.showPreview = flag === undefined ? !this.showPreview : flag;
                
                this.thumbImgList = this.showPreview ? this.getThumbImgList() : [];
                this.toggleBodyScroll(!this.showPreview);
            },
            /**
             * 获得缩略图列表
             * 保证currentIndex不是在thumbImgList边界时切换到second或-second位置
             * @returns {*}
             */
            getThumbImgList() {
                if (!this.imgList) {
                    return [];
                }
                // 少于THUMB_IMG_LENGTH张图直接返回
                if (this.imgListLength <= THUMB_IMG_LENGTH) {
                    return this.imgList;
                }
                if (!this.thumbImgList || !this.thumbImgList.length) { // 缩略图列表不存在，初始化缩略图列表
                    if (this.currentIndex > 0) { // 选中index不是第一个图片
                        if ((this.imgListLength - THUMB_IMG_LENGTH) >= this.currentIndex) { // 选中index后还有足够THUMB_IMG_LENGTH的图片展示
                            this.offset = this.currentIndex - 1;
                            return this.imgList.slice(this.currentIndex - 1, this.currentIndex + THUMB_IMG_LENGTH - 1); // 默认选中为thumbList第二个
                        } else {
                            this.offset = this.imgListLength - THUMB_IMG_LENGTH;
                            return this.imgList.slice(this.imgListLength - THUMB_IMG_LENGTH);
                        }
                    } else { // 选中index为第一张图片
                        this.offset = 0;
                        this.currentIndex = 0; // 重新赋值，防止错误
                        return this.imgList.slice(0, THUMB_IMG_LENGTH);
                    }
                } else { // 存在缩略图列表，查询位置
                    // 达到缩略图最后一张且imgList后面还有图, thumbList向后移一张
                    if (this.currentIndex - this.offset >= THUMB_IMG_LENGTH - 1 && this.currentIndex < this.imgListLength - 1) {
                        this.offset++;
                        return this.imgList.slice(this.currentIndex - THUMB_IMG_LENGTH + 2, this.currentIndex + 2);
                    }
                    
                    // 达到缩略图第一张且imgList前面还有图, thumbList向前移一张
                    if (this.currentIndex - this.offset <= 0 && this.offset > 0) {
                        this.offset--;
                        return this.imgList.slice(this.currentIndex - 1, this.currentIndex + THUMB_IMG_LENGTH - 1);
                    }
                    
                    return this.thumbImgList; // 其他情况，原样返回
                }
            },
            goPre() {
                if (this.currentIndex <= this.imgList[0].index) {
                    return;
                }
                this.setCurrentIndex(this.currentIndex - 1);
            },
            goNext() {
                if (this.currentIndex >= this.imgList[this.imgListLength - 1].index) {
                    return;
                }
                this.setCurrentIndex(this.currentIndex + 1);
            },
        },
    };
</script>
