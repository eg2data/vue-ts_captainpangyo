<template>
  <div>
    <label for="todo-input"></label>
    <input id="todo-input" type="text" :value="test" @input="updateTodoText" />
    <button type="button">add</button>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend ({
      props: ['test'],
      methods: {
        updateTodoText(event) {
          event.target.value;

        }
      }

    })
</script>

<style scoped>

</style>
