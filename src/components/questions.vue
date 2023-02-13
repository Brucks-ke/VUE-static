<template>
    <h1>{{ title }}</h1>
    <p>
        请输入你的{{title}}：<input type="text" v-model="qName">
        <button @click="add">添加</button>
    </p>
    <ul>
        <li v-for="(item,index) in questionList" :key="index">{{ item }}</li>
    </ul>
    <button @click="callDemo">父组件的方法</button>
</template>

<script>
    import {ref,reactive} from 'vue'
    export default {
        props:["title"],
        emits:["demo"],
        setup(props,ctx){
            const questionList = reactive([])
            const qName = ref("")
            const title = ref(props.title)
            function add() {
                questionList.push(qName.value)
            }
            // 获取数量
            function getNums() {
                const num = questionList.length
                return num
            }
            function callDemo (){
                ctx.emit('demo')
            }
            return {
                questionList,
                qName,
                add,
                title,
                getNums,
                callDemo
            }
        }
    }
    addEventListener
</script>

<style>

</style>