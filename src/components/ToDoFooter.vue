<template>
    <footer class="footer">
      <span class="todo-count">剩余<strong>{{ count }}</strong></span>
      <ul class="filters">
        <li>
          <a :class="{selected: type === 'all'}" href="javascript:;" @click="$emit('updateType', 'all')">全部</a>
        </li>
        <li>
          <a :class="{selected: type === 'undone'}" href="javascript:;" @click="$emit('updateType', 'undone')">未完成</a>
        </li>
        <li>
          <a :class="{selected: type === 'done'}" href="javascript:;" @click="$emit('updateType', 'done')">已完成</a>
        </li>
      </ul>
      <button class="clear-completed" @click="$emit('clear')" >清除已完成</button>
    </footer>
  </template>
  
  <script>
import { computed } from 'vue';

  export default {
    name: "TodoFooter",
  components: {},
  props: {
    list: {
      type: Array,
      required:true
    },
    // 状态
    type: {
      type: String,
      required: true
    }
    },
    setup (props) {
      // 通过props接受的数据，没有value，虽然他在父组件通过ref定义的
      let count = computed(() => props.list.filter(item => !item.done).length)

      // 返回数据与方法
      return {
        count
      }
    }
  }
  </script>