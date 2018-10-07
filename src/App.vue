<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoMain ref="main" :todos="todos"
             @add="add" @remove="remove" @update="update"></TodoMain>
    <TodoFooter :todos="todos" @clear="removeAll"></TodoFooter>
  </div>
</template>

<script>
import axios from 'axios';
import TodoHeader from './components/TodoHeader.vue';
import TodoMain from './components/TodoMain.vue';
import TodoFooter from './components/TodoFooter.vue';

const URL = 'http://localhost:3000/data';

export default {
  data() {
    return {
      todos: []
    };
  },
  created() {
    axios.get(URL).then(response => this.todos = response.data);
  },
  methods: {
		add(title) {
      axios.post(URL, { title, done: false }).then(
        response => this.todos.push(response.data)
      );
    },
    update(todo) {
      axios.patch(URL + '/' + todo.id, todo);
    },
    remove(id, i) {
      axios.delete(URL + '/' + id).then(() => this.todos.splice(i, 1));
    },
    removeAll() {
      axios.get(URL).then(
        response => {
          const data = response.data;
          if (data.length) {
            axios.delete(URL + '/' + data[0].id).then(this.removeAll);
          } else {
            this.todos = [];
            this.$refs.main.cancel();
          }
        }
      );
    }
  },
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter
  }
}
</script>

<style>
body {
  margin: 0;
  text-align: center;
  background: #f0f0f0;
}
#app {
  margin: 0 10px;
}
.button {
  cursor: pointer;
}
.shadow {
  box-shadow: 5px 5px 7px rgba(100, 100, 100, 0.5);
}
</style>