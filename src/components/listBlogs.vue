<template>
    <div id="show-blogs">
        <h1>List blog Title</h1>
        <input type="text" v-model="search" placeholder="タイトルを検索" />
        <div v-if="filteredBlogs.length == 0">
            <h3>検索結果に該当するタイトルのブログはありません</h3>
        </div>
        <div v-for="blog in filteredBlogs" class="single-blog">
            <h2 v-rainbow>{{ blog.title | to-uppercase }}</h2>
            <article>{{ blog.body | snippet }}</article>
        </div>
    </div>
</template>

<script>
import searchMixin from '../mixins/searchMixin';

export default {
  data () {
        return {
            blogs: [],
            search: ''
        }
    },
    methods: {
    },
    created() {
        this.$http.get('https://vue-blog-7eddf.firebaseio.com/posts.json')
            .then(function(data){
                this.blogs = data.body.slice(0,10);
            })
    },
    computed: {
    },
    filters: {
        toUppercase(value){
            return value.toUpperCase();
        },
        snippet(value){
            return value.slice(0,100) + '..';
        }
    },
    directives: {
        'rainbow': {
              bind(el,binding,vnode){
                  el.style.color = "#" + Math.random().toString().slice(2,8);
            }
        }
    },
    mixins: [searchMixin]
}
</script>

<style>
#show-blogs {
    max-width: 800px;
    margin: 0 auto;
}

.single-blog {
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
</style>