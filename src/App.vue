<template>
    <div id="app">
        <pull-down ref="pulldown"></pull-down>

        <header>新闻列表</header>

        <div class="news-container">
            <div class="news" v-for="news in list">
                <img :src="news.img" alt="" class="news-image">
                <div class="news-info">
                    <div class="news-title">{{ news.title }}</div>
                    <div class="news-remark">{{ news.source }}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import PullDown from './PullDown.vue';
    import axios from 'axios';

    export default {
        name: 'app',
        components: {
            [PullDown.name]: PullDown
        },
        data () {
            return {
                list: []
            }
        },
        methods: {
            loadData: function(callback) {
                axios.get('http://182.92.167.237:8001/news').then(resp => {
                    let result = resp.data;
                    let list = result.list;

                    this.list = list;

                    callback();
                }).catch(() => {
                    callback();
                });
            }
        },
        mounted() {
            this.$refs.pulldown.bindElement(null, () => {
                return new Promise((resolve, reject) => {
                    this.loadData(() => {resolve();});
                });
            });

            this.loadData();
        }
    }
</script>

<style>
    header {
        position: fixed;
        left: 0;
        width: 100%;
        top: 0;
        height: 40px;
        color: #ffffff;
        font-size: 16px;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #63c47d;
    }

    .news-container {
        padding-top: 40px;
    }

    .news {
        display: flex;
        align-items: stretch;
        padding: 10px;
        border-bottom: 1px solid #eeeeee;
    }
    
    .news:active {
        background-color: #f3f3f3;
    }

    .news-image {
        flex-shrink: 0;
        width: 95px;
        height: 62px;
        display: block;
        margin-right: 10px;
    }

    .news-info {
        padding: 5px 0;
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }

    .news-title {
        font-size: 14px;
        color: #333;
        line-height: 1.2;
    }

    .news-remark {
        font-size: 12px;
        color: #999;
    }
</style>
