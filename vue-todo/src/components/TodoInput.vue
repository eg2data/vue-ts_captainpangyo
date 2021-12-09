<template>
  <div>
    <label for="todo-list">오늘 할 일: </label>
    <input id="todo-list" type="text" :value="item" @input="handleInput"/>
    <button type="button" @click="addTodo">추가</button>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend ({
      props: {
        item: {
          type: String,
          required: true // 근데 이게 무슨 의미가 있지? 어디서 확인 가능할까?
        }
      },
      methods: {
        handleInput(event: InputEvent) {
          const eventTarget = event.target as HTMLInputElement;
          this.$emit('input', eventTarget.value)
        },
        addTodo() {
          this.$emit('add')
        }
      }
    })
</script>

<style scoped>

</style>