<template>
  <div>
    <label for="input-todo">오눌 할 일: </label>
    <input id="input-todo" type="text" :value="item" @input="handleInput"/>
    <button type="button" @click="addTodo">추가</button>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend ({
      props: {
        item: {
          type: String,
          required: true
        }
      },
      methods: {
        handleInput(event: InputEvent) {
          if (!event.target) {
            return;
          }
          this.$emit('input', event.target.value);
          // const eventTarget = event.target as HTMLInputElement;
          // this.$emit('input', eventTarget.value);
        },
        addTodo() {
          this.$emit('add') // button과 input의 value는 자동으로 연결되어 있나?????
                                  // 뭘 넘기는지 적지 않는 것이 그래서 생소함
        }
      }
    })
</script>

<style scoped>

</style>