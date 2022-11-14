<template>
  <div class="todoapp">
    <ToDoHeader @add="add" :list="list"></ToDoHeader>
    <ToDoMain :list="filterList" @del="del"></ToDoMain>
    <ToDoFooter @clear="clear" :list="list" :type="type" @updateType="type = $event"></ToDoFooter>
  </div>

</template>

<script>
import { computed, ref, watch } from 'vue';
import ToDoFooter from './components/ToDoFooter.vue';
import ToDoHeader from './components/ToDoHeader.vue';
import ToDoMain from './components/ToDoMain.vue';

export default {
  name: 'App',
  components: {
    ToDoHeader,
    ToDoMain,
    ToDoFooter
  },
  setup () {
    // 定义数据
    // let list = ref([
    //   { id: 1, title: '吃饭', done: false },
    //   { id: 2, title: '写代码', done: true }
    // ])
  // 还原数据
  let list = ref(JSON.parse(localStorage.getItem('list') || '[]'))

    //  新增
    function add (title) {
      list.value.push({ id: list.value.length + 1, title, done: false })
    }
    // 删除单个
    function del (id) {
      // 通过splice方法删除元素
      let index = list.value.findIndex(item => item.id === id)
      if (index > -1) {
        list.value.splice(index, 1)
      }
    }
    // 删除已完成
    function clear () {
      list.value = list.value.filter(item => !item.done)
    }
    // 定义三种状态
    let type = ref('all') // all done undone
      // 定义根据条件过滤后的数据
    let filterList = computed(() => {
      return list.value.filter(t => {
                /*
        if(type.value === 'all'){
          return true 
        }else if(type.value ==='done' && t.done){
          return true 
        }else if(type.value ==='undone' && !t.done){
          return true 
        }else{
          return false
        }
        */
        if (type.value === 'all' || (type.value === 'done' && t.done) || (type.value === 'undone' && !t.done)) {
        return true
        } else {
        return false
       }
      })
    })
    // 做监听器  持久化
    watch(list.value, val => {
      localStorage.setItem('list', JSON.stringify(val))
    })
    //  数据和方法需要导出才能使用
    return {
      list, add, del, clear, type,filterList
    }
  }
}
</script>
