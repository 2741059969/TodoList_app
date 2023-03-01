<template>
  <div id="app">
    <input type="search" v-model="searchinfo" @blur="bluraction"><span @click="search_task" class="searchbtn">搜索</span>
    <my-task v-for="item in vrilist"
    :title="item.title"
    :description="item.description"
    :priority="item.priority"
    :deadline="item.deadline"
    :state="item.state"
    :id="item.id"
    :key="item.id"
    @del="Del"
    @edit="Edit"
    ></my-task>
    <add-task @add="Additem"></add-task>
    <edit-form :id="editid" v-show="isShow" @hide="Hide" @editform="EditForm"></edit-form>
  </div>
</template>

<script>
import MyTask from '@/components/MyTask.vue'

import AddTask from './components/AddTask.vue'
import EditForm from './components/EditForm.vue'
export default {
  name: 'App',
  data () {
    return {
      // 这两个值用于控制表单的出现和传入表单对应的id
      isShow: false,
      editid: -1,
      // 数组数据
      todolist: [
        { title: 'hello', description: 'world', priority: '高', deadline: '2023-3-26', state: false, id: 0 },
        { title: 'hello', description: 'world', priority: '高', deadline: '2023-3-26', state: false, id: 1 }, { title: 'hello', description: 'world', priority: '低', deadline: '2023-3-26', state: false, id: 2 },
        { title: 'hello', description: 'world', priority: '中', deadline: '2023-3-26', state: false, id: 3 },
        { title: 'hello', description: 'world', priority: '低', deadline: '2023-3-26', state: false, id: 4 }

      ],
      // 一个会变化的列表数据，和双向绑定的输入信息
      vrilist: [],
      searchinfo: ''
    }
  },
  computed: {
    sortlist () {
      // 调用排序函数返回新的排序好的数组数据
      return this.paixu([...this.todolist])
    },
    // 获取最大的id值用于在添加新数据时作为新数据的id
    maxid () {
      let m = 0
      this.todolist.forEach((item) => {
        if (item.id > m) {
          m = item.id
        }
      })
      return m
    }
  },
  // 函数
  created () {
    this.vrilist = this.todolist
  },
  methods: {
    // 定义搜索框失去焦点时的行为，如果失去焦点并且搜索框里没有内容，那么恢复原来的排列
    bluraction () {
      if (!this.searchinfo) {
        this.vrilist = this.todolist
      }
    },
    // 定义搜索关键词函数，有关键词返回true
    searchkeyword (childstr, str) {
      return str.indexOf(childstr) !== -1
    },
    // 搜索功能
    search_task () {
      this.vrilist = this.todolist.filter((item) => {
        for (const i in item) {
          if (i === 'state' || i === 'id' || i === 'deadline') {
            continue
          }
          if (this.searchkeyword(this.searchinfo, item[i])) { return true }
        }
        return false
      })
    },

    // 在任务组件中点击编辑并发送自定义事件，父组件中接收到然后触发下面这个函数，拿到传过来的id
    Edit (id) {
      // 点击编辑显示表单用于修改数据
      this.isShow = true
      // 把传过来的id赋值给表单的标记id，出现的表单将与该条数据关联
      this.editid = id
      // console.log(this.editid, id)
    },
    // 编辑表单组件完成后点确认修改时触发这个函数
    EditForm (item) {
      // 定义一个索引变量用于存放即将要改变的数据在数组中的索引
      let ind = -1
      // 查找要更改的数据在数组中的索引位置
      this.todolist.some((i, index) => {
        if (i.id === item.id) {
          ind = index
          // console.log(index, ind)
          return true
        } else {
          return false
        }
      })
      // set方法使更改后的数据是响应式的，能触发视图更新
      this.$set(this.todolist, ind, item)
      // 改完数据后隐藏表单的显示
      this.isShow = false
    },
    // 编辑表单数据后取消修改触发这个函数
    Hide () {
      this.isShow = false
    },
    // 当用户在任务添加组件中输入了数据并点击添加后在父组件中触发下面这个函数往数组中追加一条新数据
    Additem (data) {
      this.todolist.push({ ...data, id: this.maxid + 1 })
    },
    // 当用户在任务组件中点击了删除会发送自定义事件并传过来要删除的数据的id,父组件监听到触发这个函数
    Del (id) {
      this.todolist.forEach((item, index) => {
        if (item.id === id) {
          this.todolist.splice(index, 1)
        }
      })
    },
    // 对数组进行排序，并返回这个数组
    paixu (arr) {
      for (let i = 0; i < arr.length - 1; i++) {
        for (let j = 0; j < arr.length - 1 - i; j++) {
          const prioarr = ['低', '中', '高']
          if (prioarr.indexOf(arr[j].priority) < prioarr.indexOf(arr[j + 1].priority)) {
            const temp = arr[j]
            arr[j] = arr[j + 1]
            arr[j + 1] = temp
          }
          // else if (prioarr.indexOf(arr[j].priority) === prioarr.indexOf(arr[j + 1].priority)) {
          //   if()
          // }
        }
      }
      return arr
    }
  },
  components: {
    MyTask,
    AddTask,
    EditForm
  }
}
</script>
<style lang="less">

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  .searchbtn{
    border: 1px solid grey;
    margin-left: 10px;
  }
}
</style>
