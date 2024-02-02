<template lang="">
  <div class="todo-add">
    <label for="checkbox" class="checkbox">
      <input type="checkbox" id="checkbox" v-model="todo.completed" />
      <i class="checkbox-icon"></i>
    </label>
    <input
      type="text"
      class="message"
      placeholder="Create a new todo..."
      v-model="todo.massage"
      @keyup.enter="addTodo"
    />
  </div>
</template>
<script>
import { v4 as uuidv4 } from 'uuid';

export default {
  emits: ['AddTodo'],
  data() {
    return {
      todo: {
        massage: '',
        completed: false,
      },
    };
  },
  methods: {
    addTodo() {
      if (this.todo.massage.length == 0) return;
      const newTodo = { id: uuidv4(), ...this.todo };
      const test = JSON.parse(JSON.stringify(newTodo));
      this.$emit('AddTodo', newTodo);
      this.todo.massage = '';
      this.todo.completed = false;
    },
  },
};
</script>
<style scoped>
.todo-add {
  display: grid;
  align-items: center;
  grid-template-columns: minmax(2rem, 15%) minmax(2rem, 85%);
  grid-template-rows: 35px;
  background-color: var(--todo-bg);
  color: var(--todo-text);
  border-radius: 6px;
  padding: 12px;
  margin-top: 40px;
}

.checkbox {
  position: relative;
  width: 1.5rem;
  height: 1.5rem;
  justify-self: center;
}

.checkbox input {
  position: absolute;
  display: none;
}

.checkbox-icon {
  position: absolute;
  width: 100%;
  height: 100%;
  background: var(--todo-border);
  border: none;
  border-radius: 50%;
}

.checkbox-icon:after {
  content: '';
  position: absolute;
  inset: 0;
  margin: 2px;
  border-radius: inherit;
  background: var(--todo-bg);
}

.checkbox-icon:hover {
  cursor: pointer;
  background: linear-gradient(
    to right bottom,
    hsl(192, 100%, 67%),
    hsl(280, 87%, 65%)
  );
}

.todo-add .checkbox input:checked + .checkbox-icon:after {
  background-image: url(/icons/icon-check.svg),
    linear-gradient(to right bottom, hsl(192, 100%, 67%), hsl(280, 87%, 65%));
  background-repeat: no-repeat;
  background-position: center;
  margin: 0px;
}

.message {
  all: unset;
}
</style>
