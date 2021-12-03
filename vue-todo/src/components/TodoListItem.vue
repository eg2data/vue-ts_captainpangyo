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
          type: Object as PropType<Todo>,
        },
        index: {
          type: Number,
        }
      },
      computed: {
        todoItemClass(): string | any {
          return this.item.done ? "completed" : null;
        }
      },
      methods: {
        toggleTodo() {
          this.$emit('toggle', this.item, this.index)
        },
        removeTodo() {
          this.$emit('remove', this.index)
        }
      }
    })
</script>

<style scoped>
.item {
  cursor: pointer;
}
.completed {
  text-decoration: line-through;
}
</style>