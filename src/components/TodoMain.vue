<template>
  <ul>
    <li>
      <span></span>
      <input v-model="newTitle" @keyup.enter="add()" @keyup.esc="reset()"
             type="text" placeholder="여기에 할 일을 써넣으십시오.">
      <span>
        <template v-if="newTitle">
          <i @click="add()" class="button far fa-hdd" title="저장"></i>
          <i @click="reset()" class="button fas fa-undo" title="취소"></i>
        </template>
      </span>
      <span></span>
    </li>
    <li v-for="(todo, i) in todos"
          :key="todo.id" :class="['shadow', { done: todo.done }]">
      <i @click="setDone(todo)" class="button fas fa-check" title="완료?"></i>
      <template v-if="i === iEdit">
        <input v-model="titleEdit" type="text"
               @keyup.enter="save(todo)" @keyup.esc="cancel()">
        <span>
          <i @click="save(todo)" class="button far fa-hdd" title="저장"></i>
          <i @click="cancel()" class="button fas fa-undo" title="취소"></i>
        </span>
      </template>
      <template v-else>
        <span @dblclick="edit(todo, i)">{{ todo.title }}</span>
        <span>
          <i v-if="iEdit < 0"
             @click="edit(todo, i)" class="button far fa-edit" title="수정"></i>
          <i @click="remove(todo.id, i)" class="button far fa-trash-alt" title="삭제"></i>
        </span>
      </template>
      <span></span>
    </li>
  </ul>
</template>

<script>
export default {
  props: ['todos'],
  data() {
    return {
      newTitle: '',
      titleEdit: '',
      iEdit: -1
    };
  },
  methods: {
    add() {
      const title = this.newTitle.trim();
      if (title) {
				this.$emit('add', title)
      }
      this.reset();
    },
    reset() {
      this.newTitle = '';
    },
    setDone(todo) {
      todo.done = !todo.done;
      this.$emit('update', todo);
    },
    edit(todo, i) {
      this.titleEdit = todo.title;
      this.iEdit = i;
    },
    remove(id, i) {
      if (i < this.iEdit) {
        this.iEdit--;
      }
      this.$emit('remove', id, i);
    },
    save(todo) {
      if (todo.title !== this.titleEdit) {
        todo.title = this.titleEdit;
        this.$emit('update', todo);
      }
      this.cancel();
    },
    cancel() {
      this.iEdit = -1;
    }
  },
}
</script>

<style scoped>
ul {
  margin: 0 20px;
  padding: 0;
  list-style-type: none;
}
li {
  margin: 0 7px 7px 0;
  line-height: 50px;
  width: 100%;
  border:1px solid silver;
  border-radius: 5px;
  background: white;
  display: table;
}
li > * {
  display: table-cell;
}
li > :first-child {
  width: 40px;
}
li > :nth-child(3) {
  width: 55px;
  text-align: right;
}
li > :last-child {
  width: 10px;
}
li > :nth-child(2) {
  text-align: left;
  padding: 0 0 0 6px;
}
li:first-child > :nth-child(2) {
  text-align: center;
}
input {
  margin: 0 -1px;
  height: 26px;
  width: 100%;
  border: 1px solid silver;
  border-radius: 3px;
  font-family: "Malgun Gothic";
  font-size: 16px;
}
.fa-undo, .fa-trash-alt {
  color: #de4343;
}
.fa-check {
  color: orangered;
}
.done .fa-check {
  color: cyan;
}
.done > :nth-child(2) {
  text-decoration: line-through;
}
</style>