<template>
    <div id="add-blog">
        <h2>新しい記事を投稿する</h2>
        <form v-if="!submitted">
            <label>タイトル</label>
            <input v-model.lazy="blog.title" type="text" required />
            <label>内容:</label>
            <textarea v-model.lazy="blog.content"></textarea>
        <div id="checkboxes">
            <label>日記</label>
            <input type="checkbox" value="日記" v-model="blog.categories"/>
            <label>食レポ</label>
            <input type="checkbox" value="食レポ" v-model="blog.categories"/>
            <label>技術記事</label>
            <input type="checkbox" value="技術記事" v-model="blog.categories"/>
            <label>つぶやき</label>
            <input type="checkbox" value="つぶやき" v-model="blog.categories"/>
        </div>
        <label>著者:</label>
        <select v-model="blog.author">
            <option v-for="author in authors">{{ author }}</option>
        </select>
        <button v-on:click.prevent="post">追加</button>
        </form>
        <div v-if="submitted">
            <h3>追加して頂きありがとうございます</h3>
        </div>
        <div id="preview">
            <h3>プレビュー</h3>
            <p>タイトル: {{ blog.title }}</p>
            <p>内容:</p>
            <p>{{ blog.content }}</p>
            <p>カテゴリ:</p>
            <ul>
                <li v-for="category in blog.categories">{{ category }}</li>
            </ul>
            <p>著者: {{ blog.author }}</p>
        </div>
    </div>
</template>

<script>


export default {
    data () {
        return {
            blog: {
                title: "",
                content: "",
                categories: [],
                author: ""
            },
            authors: ['Keita Iwasaki','The Vue','The Angular'],
            submitted: false
        }
    },
    methods: {
        post: function() {
            this.$http.post('https://jsonplaceholder.typicode.com/posts',{
                title: this.blog.title,
                body: this.blog.content,
                userId: 1
            }).then(function(data){
                console.log(data);
                this.submitted = true;
            });
        }
    }
}
</script>

<style>
#add-blog *{
    box-sizing: border-box;
}
#add-blog{
    margin: 20px auto;
    max-width: 500px;
}
label{
    display: block;
    margin: 20px 0 10px;
}
input[type="text"], textarea{
    display: block;
    width: 100%;
    padding: 8px;
}
#preview{
    padding: 10px 20px;
    border: 1px dotted #ccc;
    margin: 30px 0;
}
h3{
    margin-top: 10px;
}
#checkboxes input{
    display: inline-block;
    margin-right: 10px;
}
#checkboxes label{
    display: inline-block;
}
</style>