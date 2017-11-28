/**
* 可排序/删除的图片列表
* Created by dujianhao on 2017/11/23.
*/
<template>
    <div class='image-list-view'>
        <div class="preview">
            <i v-if="showLeftArrow" class="iconfont icon-arrow-left" @click="goPre"></i>
            <div class="img-list">
                <div v-for="(item, index) in thumbImgList" class='img-item'>
                    <img :src="item.url" @click="showFullImg(item)">
                    <p class="text">{{item.name}}</p>
                    <div class="control">
                        <button :class='["btn", {"btn-disabled": index + startIndex < 1}]' @click="moveForward(item, index)">
                            <i class="iconfont icon-arrow-left"></i>前移
                        </button>
                        <button :class='["btn", {"btn-disabled": index + startIndex >= imgList.length - 1}]' @click="moveBackward(item, index)">
                            后移<i class="iconfont icon-arrow-right"></i></button>
                        <button class="btn" @click="removeItem(item, index)">删除</button>
                    </div>
                </div>
            </div>
            <i v-if="showRightArrow" class="iconfont icon-arrow-right" @click="goNext"></i>
        </div>
        <div class="full-img-container" v-if="showFullImgFlag" @click="showFullImgFlag=false">
            <img :src="selectedImgUrl">
        </div>
    </div>
</template>

<script>
    
    // 缩略图个数
    let THUMB_IMG_LENGTH = 9;
    export default {
        name: 'image-list',
        data() {
            return {
                startIndex: 0, // 图片展示列表startIndex
                /**
                 * 图片地址列表
                 * [{
                 *  url: '',
                 *  name: ''
                 * }]
                 */
                imgList: [],
                thumbImgList: [], // 图片展示列表
                showFullImgFlag: false,
                selectedImgUrl: undefined,
            };
        },
        computed: {
            imgListLength() {
                return this.imgList ? this.imgList.length : 0;
            },
            showLeftArrow() {
                return this.startIndex > 0;
            },
            showRightArrow() {
                return this.imgListLength > this.startIndex + THUMB_IMG_LENGTH;
            }
        },
        mounted() {
            this.thumbImgList = this.getThumbImgList();
            // 根据控件宽度重算缩略图个数
            THUMB_IMG_LENGTH = Math.floor((this.$el.getBoundingClientRect().width - 40) / 164);
            this.$el.style.width = `${164 * THUMB_IMG_LENGTH + 40}px`;
        },
        methods: {
            /**
             * 设置图片源与预览大图index
             */
            setImageList(imgList, startIndex = 0) {
                this.imgList = imgList;
                this.setStartIndex(startIndex);
            },
            getImageList() {
                return this.imgList;
            },
            /**
             * 设置缩略图片位置index
             * @param index
             */
            setStartIndex(index) {
                this.startIndex = index;
                this.thumbImgList = this.getThumbImgList();
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
                if (this.startIndex > 0) { // 选中index不是第一个图片
                    // 选中index后还有足够THUMB_IMG_LENGTH的图片展示
                    if (this.imgListLength >= (this.startIndex + THUMB_IMG_LENGTH)) {
                        return this.imgList.slice(this.startIndex, this.startIndex + THUMB_IMG_LENGTH);
                    } else {
                        this.startIndex = this.imgListLength - THUMB_IMG_LENGTH; // startIndex重新赋值，防止错误
                        return this.imgList.slice(this.imgListLength - THUMB_IMG_LENGTH);
                    }
                } else { // 选中index为第一张图片
                    this.startIndex = 0; // startIndex重新赋值，防止错误
                    return this.imgList.slice(0, THUMB_IMG_LENGTH);
                }
            },
            goPre() {
                this.setStartIndex(Math.max(--this.startIndex, 0));
            },
            goNext() {
                this.setStartIndex(Math.min(++this.startIndex, this.imgListLength - 1));
            },
            showFullImg({ url }) {
                this.selectedImgUrl = url;
                this.showFullImgFlag = true;
            },
            moveForward(item, index) {
                const imgIndex = index + this.startIndex;
                if (imgIndex === 0) {
                    return;
                }
                // 交换位置, 直接赋值无法触发Vue数据变化,使用splice赋值
                this.imgList.splice(imgIndex - 1, 1, this.imgList.splice(imgIndex, 1, this.imgList[imgIndex - 1])[0]);
                // 移动item为第一个，thumbList整体向前移
                this.setStartIndex(index === 0 ? this.startIndex - 1 : this.startIndex);
                this.$emit('move-forward', item, this.imgList);
            },
            moveBackward(item, index) {
                const imgIndex = index + this.startIndex;
                if (imgIndex >= this.imgList.length - 1) {
                    return;
                }
                // 交换位置, 直接赋值无法触发Vue数据变化,使用splice赋值
                this.imgList.splice(imgIndex + 1, 1, this.imgList.splice(imgIndex, 1, this.imgList[imgIndex + 1])[0]);
                // 移动item为最后个，thumbList整体向后移
                this.setStartIndex(index === THUMB_IMG_LENGTH - 1 ? this.startIndex + 1 : this.startIndex);
                this.$emit('move-backward', item, this.imgList);
            },
            removeItem(item, index) {
                const imgIndex = index + this.startIndex;
                this.imgList.splice(imgIndex, 1); // 删除元素
                this.setImageList(this.imgList, this.startIndex);
                this.$emit('remove-img', item, this.imgList);
            },
        },
    };
</script>
