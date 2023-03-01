<template>
<div class="editform">
<p>标题:<input type="text" v-model="obj.title"></p>
<p>描述:<input type="text" v-model="obj.description"></p>
<p>优先级:<input type="text" v-model="obj.priority"></p>
<p>截止:<input type="text" v-model="obj.deadline"></p>
<p>是否完成:<input type="checkbox" v-model="obj.state"></p>
<p><button @click="hide">取消</button> <button @click="savechange">确认修改</button></p>
</div>

</template>

<script>
export default {
  name: 'EditForm',
  props: ['id'],
  data () {
    return {
      // 在表单中把用户输入的数据与obj中的数据双向绑定了
      obj: {
        title: '',
        description: '',
        priority: '',
        deadline: '',
        state: false,
        id: this.id
      }
    }
  },
  methods: {
    hide () {
      // 点击取消则通知父组件隐藏表单，并清空输入的表单数据
      this.$emit('hide')
      this.obj = {
        title: '',
        description: '',
        priority: '',
        deadline: '',
        state: false,
        id: this.id
      }
    },
    // 输入结束时用户点击确认修改触发这个函数
    savechange () {
      this.obj.id = this.id
      // console.log(this.obj.id, this.id)
      // 把修改后的数据传给父组件通知父组件根据id修改自己的数组中对应id的数据
      this.$emit('editform', this.obj)
      this.obj = {
        title: '',
        description: '',
        priority: '',
        deadline: '',
        state: false,
        id: this.id
      }
    }
  }
}
</script>

<style lang="less" scoped>
.editform{
  width: 50%;
  // margin: 0 auto;
  position: absolute;
  top: 20%;
  left:30%;
  z-index: 10;
  background-color: yellow;
}
</style>
