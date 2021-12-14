<template>
  <div>
    <ul>
      <li>
        <span class="item" :class="toggleComplete" @click="toggleTodo"> {{ item.title }} </span>
        <button type="button" @click="removeTodo">삭제</button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue, {PropType} from 'vue'
import {Todo} from "@/App.vue";

    export default Vue.extend ({
      props: {
        item: {
          type: Object as PropType<Todo>
        },
        index: {
          type: Number
        }
      },
      computed: {
        toggleComplete(): string | null {
          return this.item.done ? 'completed' : null
        }
      },
      methods: {
        removeTodo() {
          this.$emit('remove', this.index)
        },
        toggleTodo() {
          this.$emit('toggle', this.item, this.index)
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