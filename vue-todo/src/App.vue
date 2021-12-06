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
  fetch(): Todo[] { // 여기서 엄청 버벅임. 뭐지~~~? 갑자기? => | 아니고 || 아오
    const value = localStorage.getItem(STORAGE_KEY) || "[]";
    const result = JSON.parse(value);
    return result;
  }
}

export interface Todo { // export!!!! 그래야 내릴 수 있지.

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
      this.todoItems.push(values); // ? 왜 이 자체의 결과는 number지? 뭐지?? => push는 배열의 크기를 return한다. 오...
      storage.save(this.todoItems); // ? 여기가 console.log(this.todoItems.push) 니까 push가 2번 되더라. 위에서 한 번 여기서 한 번. console.log 안에 있는데도.. 이게 잘 와닿지가 않았음ㅇ

      // localStorage.setItem(value, value);
      this.initTodoItem();
    },
    initTodoItem() {
      this.todoItem = "";
    },
    fetchTodoItem() {
      this.todoItems = storage.fetch().sort((a, b) => { // this.todoItems = 요게 없으니까 새로고침하면 걍 초기화될 뿐..!
        if (a.title < b.title) { // 기준은 아직도 모르겠따. 부등화와, 아래 -1, 1 등의 숫자의 의미.

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
        ...todoItem, // 확 와닿진 않는 문법
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