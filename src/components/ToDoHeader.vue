<template>
  <header class="header">
    <h1>todos</h1>
    <input id="toggle-all" class="toggle-all" type="checkbox" v-model="isAllDone" />
    <label for="toggle-all"></label>
    <input class="new-todo"
    placeholder="输入任务名称-回车确认"
    autofocus
    v-model="title"
    @keydown.enter="onAdd"
    />
  </header>
</template>

<script>
import { computed, ref } from 'vue'

export default {
  name: 'TodoHeader',
  components: {},
  props: {
    list: {
      type: Array,
      required: true
    }
  },
  setup (props, { emit }) {
    let title = ref('')
    // 新增任务
    function onAdd () {
      // 非空判断
      if (!title.value.trim()) return
      emit('add', title.value)
      // 清空
      title.value = ''
    }

    // 全选与反选
    let isAllDone = computed({
      set (val) {
        props.list.forEach(item => item.done = val)
      },
      get () {
        return props.list.every(item => item.done)
      }
    })
    // 导出数据与方法
    return {
      title,onAdd,isAllDone
    }
  },
}
</script>
