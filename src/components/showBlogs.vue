<template>
    <div id="show-blogs">
        <h1>ブログ一覧</h1>
        <input type="text" v-model="search" placeholder="タイトルを検索" />
        <div v-if="filteredBlogs.length == 0">
            <h3>検索結果に該当するタイトルのブログはありません</h3>
        </div>
        <div v-for="blog in filteredBlogs" class="single-blog">
            <router-link v-bind:to="'/blog/' + blog.id"><h2 v-rainbow>
                {{ blog.title | to-uppercase }}
                </h2></router-link>
            <article>{{ blog.content | snippet }}</article>
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
                return data.json();
            })
            .then(function(data){
                var blogsArray = [];
                for (let key in data) {
                    data[key].id = key
                    blogsArray.push(data[key]);
                }
                this.blogs = blogsArray;
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