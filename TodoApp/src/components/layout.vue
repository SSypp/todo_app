<template>
    <div class="container">
        <div class="header">
            <h1 class="header_h1">TodoList</h1>
            <input class='header_input' type="text" v-model="msg" @keyup.enter='handleAddItem'>
            <button class="header_btn" @click="handleAddItem">添加</button>
        </div>
        <div class="content" v-show="hasNodata">
            <div class="running">
                <h1 class="running_h1">进行中</h1>
                <ul>
                    <li class="running_li" v-for="(listItem,key) of lists" :key="key" v-if="!listItem.checked">  
                        <label>
                            <input type="checkbox" v-model="listItem.checked" @change='handleSaveList'>
                            <i>✓</i>
                            <p class="msg">{{listItem.title}}</p>
                        </label>
                        <button class="move_btn" @click="handleMove(key)">移除</button>
                        
                    </li>
                </ul>
            </div>
            <div class="finished">
                <h1 class="finished_h1">已完成</h1>
                <ul>
                    <li class="running_li" v-for="(listItem,key) of lists" :key="key" v-if="listItem.checked"> 
                        <label>
                            <input type="checkbox" v-model="listItem.checked" @change='handleSaveList'>
                            <i>✓</i>
                            <p class="msg">
                                <del>{{listItem.title}}</del>
                            </p>
                        </label>
                        <button class="move_btn" @click="handleMove(key)">移除</button>
                        
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'layout',
    data () {
        return {
            msg: '',
            lists: [
                {
                    title: '今晚要做50个俯卧撑！！'
                },
                {
                    title: '今天要学习webpack4!!'
                },
                {
                    title: '记得去掘金逛逛！！'
                }
            ]
        }
    } ,
    methods:{
        handleAddItem:function(){
            if(this.msg){
                this.lists.push({
                title: this.msg
                })
            }else{
                alert('任务不能为空！请输入任务')
            }
            this.msg = ''
            localStorage.setItem('lists',JSON.stringify(this.lists))
        },
        handleSaveList:function(){
            localStorage.setItem('lists',JSON.stringify(this.list))
        },
        handleMove:function(key){
            this.lists.splice(key,1)
            localStorage.setItem('lists',JSON.stringify(this.lists))
        }
    },
    mounted () {
        var lists = JSON.parse(localStorage.getItem('lists'))
        if(lists){
            this.lists = lists
        }
    },
    computed:{
        hasNodata:function(){
            return this.lists.length
        }
    }
}
</script>
<style scoped>
.container{
    width:100%;
    height:auto;
}
.header{
    width:100%;
    height:80px;
    line-height:80px;
    background-color:pink;
}
.header_h1{
    display: inline-block;
    font-size:40px;
    font-weight:bolder;
    color:white;
    margin-left:30%;
}
.header_input{
    display:inline-block;
    width:20%;
    height:40px;
    border-radius:8px;
    margin-left:2%;
    margin-right:2%;
    margin-top:-20px;
    padding-left:1%;
    padding-right:1.5%;
    font-size:22px;
    color:pink;
}
.header_btn{
    width:6%;
    height:40px;
    background-color:white;
    border-radius:8px;
    margin-top:-20px;
    font-size:26px;
    line-height:40px;
    color:pink;
}
.content{
    width:100%;
    height: 100%;
}
.finished,.running{
    margin-left:30%;
    margin-top:3%;
}
.finished_h1,.running_h1{
    font-size:30px;
    font-weight:bolder;
    color:pink;
}
.running_li{
    height:60px;
    line-height:60px;
    width:59%;
    background:pink;
    margin-top:1%;
    border-left:10px solid #ccc;
    border-radius: 8px;
    box-shadow: 0 0 5px pink;
    font-size:20px;
    font-weight: bold;
}
label i{
    font-size: 20px;
    font-style: normal;
    display: inline-block;
    width: 20px;
    height: 20px;
    text-align: center;
    line-height: 20px;
    color: pink;
    vertical-align: middle;
    margin-left:2%;
    border:white 1px solid;
    cursor: pointer;
    box-shadow: 0 0 5px #ccc;
    }
input[type="checkbox"]{
    display: none;
}
input[type="checkbox"]:checked + i{
    background: white;
}
.msg{
    display: inline-block;
    margin-left:20%;
    color:white;
    cursor: pointer;
}
.msg del{
    text-decoration: line-through;
    text-decoration-style: initial;
}
.move_btn{
    float:right;
    margin-right:2%;
    width:60px;
    height:30px;
    margin-top:15px;
    background:white;
    border-radius:5px;
    color:pink;
    box-shadow: 0 0 5px #ccc;
}
</style>
