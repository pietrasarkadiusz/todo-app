<template>
  <div class="container" :theme="lightTheme ? 'light' : 'dark'">
    <div class="home">
      <TodoHeader @ChangeTheme="lightTheme = !lightTheme" />
      <TodoAdd @AddTodo="addTodo" />
      <draggable class="todo-list" v-model="todoList" item-key="id">
        <template #item="{ element: todo }">
          <TodoItem :todo="todo" v-if="isVisible(todo)" @delete="deleteTodo" />
        </template>
      </draggable>
      <TodoFooter
        :remainingTodo="remainingTodo"
        @ClearCompleted="clearCompleted"
        :filter="filter"
        @ChangeFilter="changeFilter"
      />
      <div class="hint-text">
        <p>Drag and drop to reorder list</p>
      </div>
    </div>
  </div>
</template>

<script>
import TodoAdd from './components/TodoAdd.vue';
import TodoHeader from './components/TodoHeader.vue';
import TodoFooter from './components/TodoFooter.vue';
import TodoItem from './components/TodoItem.vue';
import { v4 as uuidv4 } from 'uuid';
import draggable from 'vuedraggable';

export default {
  data() {
    return {
      filter: 'All',
      lightTheme: 'true',
      todoList: [
        { id: uuidv4(), massage: 'first todo', completed: true },
        { id: uuidv4(), massage: 'second todo', completed: false },
        { id: uuidv4(), massage: 'third todo', completed: false },
      ],
    };
  },
  components: {
    TodoAdd,
    TodoHeader,
    TodoFooter,
    TodoItem,
    draggable,
  },
  methods: {
    addTodo(todo) {
      this.todoList.push(todo);
    },
    deleteTodo(id) {
      const findIndex = this.todoList.findIndex((a) => a.id === id);
      if (findIndex !== -1) this.todoList.splice(findIndex, 1);
    },
    changeFilter(filter) {
      this.filter = filter;
    },
    clearCompleted() {
      this.todoList = this.todoList.filter((item) => !item.completed);
    },
    isVisible(todo) {
      if (todo) {
        if (this.filter == 'All') return true;
        else {
          return this.filter == 'Active' ? !todo.completed : todo.completed;
        }
      }
    },
  },
  mounted() {
    if (localStorage.todoList) {
      this.todoList = JSON.parse(localStorage.todoList);
    }
  },
  computed: {
    remainingTodo() {
      return this.todoList.filter((item) => !item.completed).length;
    },
    filterTodoList() {
      if (this.filter == 'All') return this.todoList;
      else if (this.filter == 'Active')
        return this.todoList.filter((item) => !item.completed);
      else return this.todoList.filter((item) => item.completed);
    },
  },
  watch: {
    todoList: {
      handler(newList) {
        localStorage.todoList = JSON.stringify(newList);
      },
      deep: true,
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  overflow-y: scroll;
  background-color: var(--body-bg);
  background-image: var(--body-bg-img);
  background-repeat: no-repeat;
  background-size: 100% 300px;
  font-family: 'Josefin Sans', sans-serif;
  font-size: 18px;
}
.home {
  max-width: 560px;
  width: 100%;
  padding-inline: 1rem;
}

.todo-list {
  margin-top: 20px;
  box-shadow: 0px 50px 50px -1px rgba(0, 0, 0, 0.3);
}

.todo-list :first-child {
  border-radius: 6px 6px 0 0;
}
.hint-text {
  margin-top: 50px;
  text-align: center;
  color: var(--text-color-secondary);
  font-size: 0.8rem;
}

@media (min-width: 425px) {
  .container {
    background-image: var(--body-bg-img-desktop);
  }
}
</style>
