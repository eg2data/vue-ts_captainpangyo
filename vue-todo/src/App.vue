<template>
  <div>
    <header>
      <h1>Vue todo /w typescript 4th try.</h1>
    </header>
    <main>
      <TodoInput :item="todoText" @input="updateTodoText" @add="addTodoText"></TodoInput>
    </main>
    <div>
      <ul>
        <TodoListItem v-for="(todoItem, index) in todoItems" :key="index" :todoItem="todoItem"></TodoListItem>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoInput from "@/components/TodoInput.vue";
    import TodoListItem from "@/components/TodoListItem.vue";

    const STORAGE_KEY = "vue-fifth-try"
    const storage = {
      save(value: any[]) { // 여기서 좀 아리까리. 저 value는 꼭 필요한가에 대해 고민했어 작성 시에.
        // const parsed = JSON.stringify(this.todoItems);
        const parsed = JSON.stringify(value);
        localStorage.setItem(STORAGE_KEY, parsed);
      },
      fetch() {
        const value = localStorage.getItem(STORAGE_KEY) || "[]";
        const result = JSON.parse(value);
        return result;
      }
    }

    export default Vue.extend ({
      components: {TodoListItem, TodoInput},
      data() {
        return {
          todoText: "",
          todoItems: [] as any[]
        }
      },
      methods: {
        updateTodoText(text: any) {
          this.todoText = text;
          //this.initTodoText(); 잘못 넣으니까.. 안됐지.. 비었지 자꾸..
        },
        addTodoText() {
          const value = this.todoText;
          this.todoItems.push(value);
          storage.save(this.todoItems);
          // localStorage.setItem(value, value);
          this.initTodoText();
        },
        initTodoText() {
          this.todoText = ""
        },
        fetchTodoText() {
          this.todoItems = storage.fetch();
        }
      },
      created() {
        this.fetchTodoText();
      }
    })
</script>

<style scoped>

</style>