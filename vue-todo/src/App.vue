<template>
  <div>
    <header>
      <h1> Vue-Ts blank test after two days </h1>
    </header>
    <main>
      <TodoInput :item="todoItem" @input="updateTodoItem" @add="addTodoItem"></TodoInput>
    </main>
    <div>
      <TodoListItem :item="todoItem" :index="index" v-for="(todoItem, index) in todoItems" :key="index" @remove="removeTodoItem" @toggle="toggleTodoItem"></TodoListItem>
    </div>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoInput from "@/components/TodoInput.vue";
    import TodoListItem from "@/components/TodoListItem.vue";

    const STORAGE_KEY = "vue-ts"
    const storage = {
      save(values: Todo[]) {
        const parsed = JSON.stringify(values)
        localStorage.setItem(STORAGE_KEY, parsed)
      },
      fetch(): Todo[] {
        const value = localStorage.getItem(STORAGE_KEY) || "[]";
        const result = JSON.parse(value);
        return result;
      }
    }

    export interface Todo {
      title: string,
      done: boolean
    }

    export default Vue.extend ({
      components: {TodoListItem, TodoInput},
      data() {
        return {
          todoItem: "",
          todoItems: [] as Todo[]
        }
      },
      methods: {
        updateTodoItem(value: string) {
          this.todoItem = value
        },
        addTodoItem() {
          const value = this.todoItem
          const values: Todo = {
            title: value,
            done: false
          }
          this.todoItems.push(values)
          storage.save(this.todoItems)
          this.initTodoItem()
        },
        initTodoItem() {
          this.todoItem = ""
        },
        fetchTodoItem() {
          this.todoItems = storage.fetch().sort((a, b) => {
            if (a.title < b.title) {
              return -1;
            }
            if (a.title > b.title) {
              return 1;
            }
            return 0;
          })
        },
        removeTodoItem(index: number) {
          this.todoItems.splice(index, 1)
          storage.save(this.todoItems)
        },
        toggleTodoItem(item: Todo, index: number) {
          this.todoItems.splice(index, 1, { // 이걸 빼먹고.. 하..
            ...item,
            done: !item.done
          })
          storage.save(this.todoItems)
        },
      },
      created() {
        this.fetchTodoItem()
      }
    })
</script>

<style scoped>

</style>