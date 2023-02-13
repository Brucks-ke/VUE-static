<template>
  <p>
    <span @click="showContent(1)" style="cursor:pointer">文章列表</span>&nbsp;|<span @click="showContent(2)"
      style="cursor:pointer">问题列表</span>&nbsp;|<span @click="showContent(3)" style="cursor:pointer">笔记列表</span>&nbsp;
  </p>
  <div v-show="show1Status">
    <p>请输入文章的标题：<input type="text" v-model="aName">
      <button @click="add">添加</button>
    </p>
    <ul>
      <li v-for="(item, index) in aList" :key="index">
        {{ item }}
        <input type="text" v-show="editStatus" v-model="editAname">
        <!-- edit修改 -->
        <button @click="edit" v-if="editStatus === false">修改</button>
        <button @click="save(index)" v-else>保存</button>
      </li>
    </ul>
  </div>
  <div v-show="show2Status">
    <questions-vue :title="sName" ref="questions" @demo="demo"></questions-vue>
    <button @click="getQuestionNum()">获取问题的数量</button>
  </div>
  <div v-show="show3Status">
    <questions-vue :title="fName" ref="books" @demo="demo"></questions-vue>
  </div>
</template>

<script>
import { reactive, ref ,provide} from 'vue'
// 导入
import questionsVue from './components/questions.vue'
export default {
  components: {
    questionsVue
  },
  setup() {
    const fName = ref("笔记")
    const sName = ref("问题")
    // 定义文章列表标题
    const show1Status = ref(true)
    // 定义问题列表变量
    const show2Status = ref(false)
    // 定义笔记列表变量
    const show3Status = ref(false)
    // 1.现在父组件里面定义一个空
    const questions = ref()
    const aList = reactive(["Vue环境的搭建", "Vue的项目的创建"])
    const aName = ref("")
    let editStatus = ref(false)
    function add() {
      aList.push(aName.value)
    }
    function edit() {
      editStatus.value = true
    }
    const editAname = ref("")
    // 保存函数
    function save(index) {
      aList.splice(index, 1, editAname.value)
      editAname.value = ""
    }
    // 文章函数
    function showContent(index) {
      if (index == 1) {
        show1Status.value = true
        show2Status.value = false
        show3Status.value = false
      }
      else if (index == 2) {
        show1Status.value = false
        show2Status.value = true
        show3Status.value = false
      }
      else {
        show1Status.value = false
        show2Status.value = false
        show3Status.value = true
      }
    }
    function getQuestionNum(){
      // 去调用子组件里面的getNums()方法
      // questions就代表子组件
      const num1 = questions.value.getNums()
      alert(`问题的数量是${num1}`)
    }
    function demo(){
      console.log("这是父组件的方法");
    }
    const xx = ref("这是父组件中的值")
    provide("xx",xx.value)
    return {
      aList,
      ref,
      aName,
      add,
      edit,
      editStatus,
      editAname,
      save,
      show1Status,
      show2Status,
      show3Status,
      showContent,
      sName,
      fName,
      questions,
      getQuestionNum,
      demo,
      xx
    }
  }
}
</script>


<style>
#app {
  width: 100%;
}

* {
  margin: 0;
  list-style: none;
}

.parent {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: repeat(5, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
  width: 100%;
}

.div1 {
  grid-area: 1 / 1 / 2 / 2;
}

.div2 {
  grid-area: 1 / 2 / 2 / 6;
}

.inBlock {
  display: inline-block;
  margin-right: 50px;
}
</style>
