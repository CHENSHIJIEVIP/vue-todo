<template>
  <div>
    <input class="item" type="text" v-model="inputValue">
    <button @click="handleSubmit">提交</button>
    <ul>
       <todo-item v-for="item,index of list" :key="index" :content="item" :index="index" @delete="handleDelete"></todo-item>
    </ul>
    <div>{{msgs}}</div>
    <div @click="get">get</div>
    <div @click="post">post</div>
    <div @click="http">http</div>
    <div>{{msg}}</div>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem'
import axios from 'axios'

export default {
    components:{
        'todo-item' : TodoItem
    },
    data(){
        return {
            inputValue:'',
            list:[],
            msg:''
            ,msgs:''
        }
    },
    mounted(){
        axios.interceptors.request.use(req=>{
            console.log('request init.');
            return req;
        }),
        axios.interceptors.response.use(res=>{
            console.log('response init.');
            return res;
        })
    },
    methods:{
        handleSubmit(){
            this.list.push(this.inputValue)
            this.inputValue = ''
        },
        handleDelete(){
            this.list.splice('index',1)
        },
        get(){
            axios.get('../static/package.json',{
                params:{
                    userId:'999'
                },
                headers:{
                    token:'jack'
                }
            }).then(res=>{
                this.msg = res.data
                console.log(res.data)
            }).catch(error=>{
                console.log(error)
            })
        },
        post(){
            axios.post('../static/package.json',{
                userId:'888'
            },{
                headers:{
                    token:'jack'
                }
            }).then(res=>{
                this.msg = res.data;
            }).catch(error=>{
                console.log(error);
            })
        },
        http(){
            axios({
                url:'../static/package.json',
                method:'get',
                data:{
                    userId:'101'
                },
                params:{
                    userId:'102'
                },
                headers:{
                    token:'http-text'
                }
            }).then(res=>{
                this.msgs = res.data
            }).catch(error=>{
                console.log(error)
            })
        }

    }
}
</script>

<style>

</style>
