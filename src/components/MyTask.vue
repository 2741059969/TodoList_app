<template>
  <div class="task">
      <span class="title" >{{title}}</span>
      <span class="description" >{{description}}</span>
      <span class="priority" >{{priority}}</span>
      <span class="deadline" >{{deadline}}</span>
      <span class="state">{{state?'已完成':'未完成'}}</span>
      <span @click="edit" class="edititem">编辑</span>
       <span  @click="del(id)" class="delitem">删除</span>
  </div>
</template>

<script>
export default {
  name: 'MyTask',
  props: ['title', 'description', 'priority', 'deadline', 'state', 'id'],
  methods: {
    edit (id) {
      // 把id传过去让父组件知道是哪一条数据要修改
      this.$emit('edit', this.id)
    },
    del (id) {
      // console.log(id)
      this.$emit('del', id)
    }
  }
}
</script>

<style lang="less" scoped>
.task{
  display: flex;
  height: 50px;
  margin-bottom: 10px;
  span{
    flex: 1;
    border: 1px solid grey;
    text-align: center;
    line-height: 50px;
    background-color: aqua;

    line-height: 50px;
    text-align: center;
  }
  .delitem{
    background-color: rgba(255, 0, 0, 0.685);
    flex: 0.35;
  }
  .edititem,.delitem{
  cursor: pointer;
  }
}

</style>
