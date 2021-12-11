<template>
  <div>
    <ul>
      <li>
        <span class="item" :class="toggleComplete" @click="toggleTodo"> {{ todoItem.title }} </span>
        <button type="button" @click="removeItem">삭제</button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue, {PropType} from 'vue'
import {Todo} from "@/App.vue";

    export default Vue.extend ({
      props: {
        todoItem: {
          type: Object as PropType<Todo>
        },
        index: {
          type: Number
        }
      },
      computed: {
        toggleComplete(): string | null {
          return this.todoItem.done ? 'complete' : null;
        }
      },
      methods: {
        removeItem() {
          this.$emit('remove', this.index)
        },
        toggleTodo() {
          this.$emit('toggle', this.todoItem, this.index)
        }
      }
    })
</script>

<style scoped>
.item {
  cursor: pointer;
}
.complete {
  text-decoration: line-through;
}
</style>