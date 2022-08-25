<template>
    <div>
        <button @click="showForm = true" type="submit" class="btn btn-primary">Add Article</button>
        <div v-if="showForm">
            <ArticleForm :article="selected_article" @close="close()"/>
        </div>
        <div v-else>
            <table class="table">
            <thead>
                <tr>
                <th scope="col">id</th>
                <th scope="col">Title</th>
                <th scope="col">Content</th>
                <th scope="col">Create at</th>
                <th scope="col">action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(article, index) in articles" :key="index">
                    <th scope="row">{{article.id}}</th>
                    <td>{{article.title}}</td>
                    <td>{{article.content}}</td>
                    <td>{{article.created_at}}</td>
                    <td>
                        <button @click="showUpdateForm(article)" type="submit" class="btn btn-primary">update</button>
                        <button @click="deleteArticle(article)" type="submit" class="btn btn-primary">delete</button>
                        <button v-if="!article.votes" @click="upvote" type="submit" class="btn btn-primary">Up vote</button>
                        <button v-else @click="addArticle" type="submit" class="btn btn-primary">Down vote</button>
                    </td>
                </tr>
            </tbody>
            </table>
        </div>
    </div>
</template>
<script>
import axios from 'axios'
import ArticleForm from './form.vue'
export default {
    components:{
        ArticleForm
    },
    data(){
        return{
            showForm:false,
            articles:[],
            selected_article:{}
        }
    },
    methods:{
        close(){
            this.showForm = false
            this.getArticles()
        },
        getArticles(){
            axios.get('articles').then(({data})=>{
                this.articles = data
                console.log(data)
            })
        },
        // getArticles(article){
        //     axios.put(`articles/upvote/${article.id}`).then(({data})=>{
        //         this.articles = data
        //         console.log(data)
        //     })
        // },
        deleteArticle(article){
            axios.delete(`articles/${article.id}`).then(({data})=>{
                this.getArticles()
            })
        },
        showUpdateForm(article){
            Object.assign(this.selected_article, article)
            this.showForm = true
        }
    },
    mounted(){
        this.getArticles()
    }
}
</script>