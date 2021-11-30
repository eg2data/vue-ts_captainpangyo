<template>
  <div>
    <h1>blank_test</h1>
    <TodoInput :test="todoText" ></TodoInput>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoInput from "@/components/TodoInput.vue";

    export default Vue.extend ({
      components: {TodoInput},
      data() {
        return {
          todoText: "reset"
        }
      },
      methods: {

      },
    })
</script>

<style scoped>

</style>
