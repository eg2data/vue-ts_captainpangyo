<template>
  <div>
    <li>
      <span> {{ item }} </span>
      <button @click="removeTodo">삭제</button>
    </li>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend ({
      props: {
        item: {
          type: String,
        },
        index: { // 근데 이걸.. 위에서는 이용 안하네? 필수는 아닌가보군.
                 // 좀 이상하다. 받아와서, 아래에서 걍 다시 넘기기만하는게 끝이네
                 // 저 for문 내 item들과의 connection은 어디서 생겨?
          type: Number,
        }
      },
      methods: {
        removeTodo() {
          this.$emit('remove', this.index)
        }
      }

    })
</script>

<style scoped>

</style>