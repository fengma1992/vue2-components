<template>
    <div id="app">
        <img src="./assets/logo.png">
        <imageList ref="imglist"/>
    </div>
</template>

<script>
    
    import imageList from './components/imageList/index.vue'
    
    export default {
        name: 'app',
        components: {
            imageList
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
                    }
                ],
            };
        },
        mounted() {
            this.$refs.imglist.setImageList(this.imageList);
        },
    }
</script>
