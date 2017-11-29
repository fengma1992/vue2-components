<template>
    <div id="app">
        <img src="./assets/logo.png">
        <div class="component-container">
            <div class="introduction-area">
                <p class="type">Component Name:<span class="name">imageList</span></p>
                <p class="intro">Rearrange or delete the list as you like</p>
            </div>
            <image-list ref="imglist"></image-list>
            <button class="reset-btn" @click="setImgList">Reset</button>
        </div>
        <div class="component-container">
            <div class="introduction-area">
                <p class="type">Component Name:<span class="name">imageListPreview</span></p>
                <p class="intro">Click any image to toggle the component imageListPreview</p>
            </div>
            <div class="img-list">
                <div class="img-container" v-for="(item, index) in imageList">
                    <img class="thumb-img" :src="item.url" @click="toggleImgPreview(imageList, index)">
                </div>
            </div>
            <image-list-preview ref="imglistpreview"></image-list-preview>
        </div>
    </div>
</template>

<script>
    
    import imageList from './components/imageList/index.vue'
    import imageListPreview from './components/imageListPreview/index.vue'
    
    export default {
        name: 'app',
        components: {
            imageList,
            imageListPreview
        },
        data() {
            return {
                imageList: [
                    {
                        'name': '1.png',
                        'url': 'http://pic.pimg.tw/jardinsecret/normal_49f7e84cd9be2.jpg'
                    },
                    {
                        'name': '2.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/1213.jpg'
                    },
                    {
                        'name': '3.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/712.jpg'
                    },
                    {
                        'name': '4.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/520.jpg'
                    },
                    {
                        'name': '5.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/324.jpg'
                    },
                    {
                        'name': '6.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/109.jpg'
                    },
                    {
                        'name': '7.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/149.jpg'
                    },
                    {
                        'name': '8.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/910.jpg'
                    },
                    {
                        'name': '9.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/7a.jpg'
                    },
                    {
                        'name': '2.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/1213.jpg'
                    },
                    {
                        'name': '3.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/712.jpg'
                    },
                    {
                        'name': '4.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/520.jpg'
                    },
                    {
                        'name': '5.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/324.jpg'
                    },
                    {
                        'name': '6.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/109.jpg'
                    },
                    {
                        'name': '7.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/149.jpg'
                    },
                    {
                        'name': '8.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/910.jpg'
                    },
                    {
                        'name': '9.png',
                        'url': 'https://www.fotobeginner.com/wp-content/uploads/2015/03/7a.jpg'
                    }
                ],
            };
        },
        mounted() {
            this.setImgList();
        },
        methods: {
            setImgList() {
                this.$refs.imglist.setImageList(this.imageList);
            },
            toggleImgPreview (imageList, index) {
                this.$refs.imglistpreview.setImageList(imageList, index);
            }
        },
    }
</script>
