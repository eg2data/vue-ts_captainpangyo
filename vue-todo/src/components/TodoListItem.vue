<template>
  <div>
    <li>
      <span class="item" :class="todoItemClass" @click="toggleTodo"> {{ item.title }} </span>
      <button @click="removeTodo">삭제</button>
    </li>
  </div>
</template>

<script lang="ts">
import Vue, {PropType} from 'vue'
import {Todo} from "@/App.vue";


export default Vue.extend ({
  props: {
    item: {
      type: Object as PropType<Todo>, // 어색어색
    },
    index: {
      type: Number,
    }
  },
  computed: { // 어색어색
    todoItemClass(): string | any { // 이 type은 뭘 기준으로 준건지 사실 잘 모르겠다. 아? 아래 함수에서 나올 수 있는 결과의 타입을 그대로 받은건가? 음?

      return this.item.done ? "completed" : null;
    }
  },
  methods: {
    toggleTodo() {
      this.$emit('toggle', this.item, this.index)
    },
    removeTodo() {
      this.$emit('remove', this.index) // 여기서는 todoItem이 필요없구나..!
    }
  }
})
</script>

<!--막히네 css..-->
<style scoped>
.item {
  cursor: pointer;
}
.completed {
  text-decoration: line-through;
}
</style>
