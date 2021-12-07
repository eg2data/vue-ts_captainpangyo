<template>
  <div>
    <ul>
      <li>
<!--        completed를 저렇게 연결했으면, 앞에선 지웟어야지 어휴-->
        <span class="item " :class="todoComplete" @click="toggleTodo"> {{item.title}} </span>
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
        todoComplete(): string | any { // 아.. 여기 return을 꼭 줘야한다는 사실!!!!!!
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