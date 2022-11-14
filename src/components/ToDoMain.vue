<template>
  <ul class="todo-list">
    <li :class="{ completed: item.done, editing:item.id === currentId }"
    @dblclick="onEdit(item)" v-for="item in list"
    :key="item.id"
    >
      <div class="view">
        <input class="toggle" type="checkbox" v-model="item.done" />
        <label>{{ item.title }}</label>
        <button class="destroy" @click="$emit('del', item.id)"></button>
      </div>
      <input class="edit" v-model="item.title" @blur="currentId = ''" ref="inputRefs"  />
    </li>
  </ul>
</template>
  
<script>
import { ref, nextTick } from 'vue';

export default {
  name: "TodoMain",
  components: {},
  props: {
    list: {
      type: Array,
      required: true
    }
  },
  setup (props) {
    // 编辑当前行的id
    let currentId = ref('')
    function onEdit (item) {
      currentId.value = item.id
      let index = props.list.findIndex(t => t.id === item.id)
      nextTick(() => {
        console.log(inputRefs.value[index])
        inputRefs.value[index].focus()
      })
    }
    // 在数组循环中绑定ref
    let inputRefs = ref([])
    return {
      onEdit,currentId,inputRefs
    }
  }
}
</script>