<template>
  <div>
    <header>
      <h1> Vue blank_test /w typescript </h1>
    </header>
    <main>
      <TodoInput :item="todoItem" @input="updateTodoItem" @add="addTodoItem"></TodoInput>
    </main>
    <div>
      <ul>
        <TodoListItem v-for="(todoItem, index) in todoItems" :key="index" :item="todoItem" :index="index" @toggle="toggleTodoItem" @remove="removeTodoItem"></TodoListItem>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoInput from "@/components/TodoInput.vue";
    import TodoListItem from "@/components/TodoListItem.vue";

    const STORAGE_KEY = "vue-typescript blank_test"
    const storage = {
      save(value: Todo[] ) {
        const parsed = JSON.stringify(value);
        localStorage.setItem(STORAGE_KEY, parsed);
      },
      fetch(): Todo[] {
        const value = localStorage.getItem(STORAGE_KEY) || "[]";
        const result = JSON.parse(value);
        return result;
      }
    }

    export interface Todo {
      title: string,
      done: boolean,
    }

    export default Vue.extend ({
      components: {TodoListItem, TodoInput},
      data() {
        return {
          todoItem: "init",
          todoItems: [] as Todo[],
        }
      },
      methods: {
        updateTodoItem(value: string) {
          this.todoItem = value;
        },
        addTodoItem() {
          const value = this.todoItem;
          const values: Todo = { // 아하 type 정의를 했어야!!
            title: value,
            done: false,
          }
          this.todoItems.push(values);
          storage.save(this.todoItems);
          // localStorage.setItem(value, value);
          this.initTodoItem();
        },
        initTodoItem() {
          this.todoItem = "";
        },
        fetchTodoItem() {
          this.todoItems = storage.fetch().sort((a, b) => {
            if (a.title < b.title) {
              return -1;
            }
            if (a.title > b.title) {
              return 1;
            }
            return 0
          });
        },
        toggleTodoItem(todoItem: Todo, index: number) {
          this.todoItems.splice(index, 1, {
            ...todoItem,
            done: !todoItem.done
          });
          storage.save(this.todoItems)
        },
        removeTodoItem(value: number) {
          this.todoItems.splice(value, 1);
          storage.save(this.todoItems);
        }
      },
      created() {
        this.fetchTodoItem();
      }
    })
</script>

<style scoped>

</style>