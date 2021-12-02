<template>
  <div>
    <header>
      <h1> Vue todo /w typescript blank_test </h1>
    </header>
    <main>
      <TodoInput
          :item="todoItem"
          @input="updateTodoText"
          @add="addTodoText">
      </TodoInput>
    </main>
    <div>
      <ul>
        <TodoListItem
            v-for="(todoItem, index) in todoItems"
            :key="index"
            :item="todoItem"
            @remove="removeTodoItem"
            :index="index"
            >
        </TodoListItem>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoInput from "@/components/TodoInput.vue";
    import TodoListItem from "@/components/TodoListItem.vue";

    const STORAGE_KEY = "vue-blank-test-1";
    const storage = {
      save(value: any[]) { // []를 빼먹었었는데, 되던데?
        const parsed = JSON.stringify(value)
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
          todoItem: "init",
          todoItems: [] as any[]
        }
      },
      methods: {
        updateTodoText(value: any) { // 난 string 줬었는데, 되던데?
          this.todoItem = value;
        },
        addTodoText() {
          const value = this.todoItem;
          this.todoItems.push(value);
          storage.save(this.todoItems);
          // localStorage.setItem(value, value);
          this.initTodoText();
        },
        removeTodoItem(index: number) {
          console.log(index) // 이 습관이 필요. 확인은 console.log
          this.todoItems.splice(index, 1);
          storage.save(this.todoItems)
        },
        initTodoText() {
          this.todoItem = "";
        },
        fetchTodoText() {
          this.todoItem = storage.fetch();
        }
      },
      created() {
        this.fetchTodoText();
      }
    })
</script>

<style scoped>

</style>