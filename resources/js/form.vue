<template>
    <div>
        <div class="mb-3">
            <label for="formGroupExampleInput" class="form-label">Title</label>
            <input v-model="payload.title" type="text" class="form-control" id="formGroupExampleInput" placeholder="Example input placeholder">
        </div>
        <div class="mb-3">
            <label for="formGroupExampleInput2" class="form-label">Content</label>
            <input v-model="payload.content" type="text" class="form-control" id="formGroupExampleInput2" placeholder="Another input placeholder">
        </div>
        <div class="col-12">
            <button v-if="payload.id" @click="updateArticle" type="submit" class="btn btn-primary">Update</button>
            <button v-else @click="addArticle" type="submit" class="btn btn-primary">Create</button>
        </div>
    </div>
</template>
<script>
import axios from 'axios'
export default {
    props:{
        article:{

        }
    },
    data(){
        return{
            payload:{}
        }
    },
    methods:{
        addArticle(){
            console.log(this.payload.content,"this is payload")
            axios.post('articles', this.payload).then(({data})=>{
                this.$emit('close')
            })
        },
        updateArticle(){
            console.log(this.payload.content,"this is payload")
            delete this.payload.created_at
            delete this.payload.updated_at
            axios.put(`articles/${this.payload.id}`, this.payload).then(({data})=>{
                this.$emit('close')
            })
        },
    },
    mounted(){
        console.log(axios,"axios sndsdshdshdg")
        this.payload = this.article
    },
    watch:{
        "article":{
            handler(val){
                console.log(val)
                this.payload = val
            }
        }
    }
}
</script>