<template>
  <div id="addtask">
     <label >标题<input type="text" v-model="info.title" ></label>
     <label >描述<input type="text" v-model="info.description" ></label>
     <label >优先级<input type="text" v-model="info.priority" ></label>
     <label>截止<input type="text" v-model="info.deadline" ></label>
     <label>已完成<input type="checkbox" v-model="info.state" ></label>
     <button @click="addtask">添加</button>
  </div>
</template>

<script>
export default {
  name: 'AddTask',
  data () {
    return {
      info: {
        title: '',
        description: '',
        priority: '',
        deadline: '',
        state: false

      }
    }
  },
  methods: {
    addtask () {
      // 点击添加按钮的时候把数据传递给父组件，父组件拿到数据后加入id然后生成新数据再加入到数组中
      this.$emit('add', this.info)
      // 清空任务添加组件中的输入信息
      this.info = {
        title: '',
        description: '',
        priority: '',
        deadline: '',
        state: false

      }
    }
  }

}
</script>

<style lang="less" scoped>
#addtask{
  display: flex;
  label{
    flex: 1;
    // border: 1px solid black;
  }
}
</style>
