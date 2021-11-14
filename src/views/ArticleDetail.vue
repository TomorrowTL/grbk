/* 文章更新与删除组件 */
<template>
<div>
    <BlogHeader/>

    <div v-if="article !== null" class="grid-container">
        <div>
            <h1 id="title">{{ article.title }}</h1>
            <div class="upserve">
                <p id="subtitle">
                本文由 {{ article.author.username }} 发布于 {{ formatted_time(article.created) }} 
                </p>
                <div v-if="isSuperuser" class="update">
                    <router-link :to="{ name: 'ArticleEdit', params: { id: article.id }}">更新与删除</router-link>
                </div>
            </div>
            
            <div v-html="article.body_html" class="article-body"></div>
        </div>
        <div>
            <h3>目录</h3>
            <div v-html="article.toc_html" class="toc"></div>
        </div>
    </div>

    <Comments :article="article" />

    <BlogFooter/>
</div>
    

</template>

<script>
    import BlogHeader from '@/components/BlogHeader.vue'
    import BlogFooter from '@/components/BlogFooter.vue'
    import Comments from '@/components/Comments.vue'

    import axios from 'axios';


    export default {
        name: 'ArticleDetail',
        components: {BlogHeader, BlogFooter, Comments},
        data: function () {
            return {
                article: null
            }
        },
        mounted() {
            axios
                .get('/api/article/' + this.$route.params.id)
                .then(response => (this.article = response.data))
        },
        methods: {
            formatted_time: function (iso_date_string) {
                const date = new Date(iso_date_string);
                return date.toLocaleDateString()
            }
        },
        computed: {
            isSuperuser() {
                return localStorage.getItem('isSuperuser.myblog') === 'true'
            }
        }
    }
</script>

<style scoped>
    .grid-container {
        display: grid;
        grid-template-columns: 3fr 1fr;
    }


    #title {
        text-align: center;
        font-size: x-large;
    }
    .upserve{
        position: relative;
    }

    #subtitle {
        position: absolute;
        right: 105px;
        top: -5px;
        text-align: center;
        color: gray;
        font-size: small;
        overflow: hidden; 
    }
    
    .update{
        width: 100px;
        height: 40px;
        position: absolute;
        right: 0;
        background-color: rgb(14, 172, 211);
        overflow: hidden; 
        border-radius: 15%;
        line-height: 40px;
        text-align: center;
    }
    .update a{
        text-decoration: none;
        color: rgb(255,255,255,1);
    }

</style>

<style>
    .article-body p img {
        max-width: 100%;
        border-radius: 50px;
        box-shadow: gray 0 0 20px;
    }

    .toc ul {
        list-style-type: none;
    }

    .toc a {
        color: gray;
    }
</style>