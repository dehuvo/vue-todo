<template>
  <footer v-if="todos.length" class="button shadow">
    <span @click="clear()">할 일 모두 지우기</span>
  </footer>
</template>

<script>
export default {
  props: ['todos'],
  methods: {
    clear() {
      this.$emit('clear');
    }
  }
}
</script>

<style scoped>
footer {
  margin: 18px auto;
  line-height: 40px;
  width: 10rem;
  border: 1px solid sliver;
  border-radius: 20px;
  color: #e20303;
  background: white;
}
</style>