<template lang="">
  <div class="todo-item">
    <label class="checkbox">
      <input type="checkbox" v-model="todo.completed" />
      <i class="checkbox-icon"></i>
    </label>
    <input type="text" class="message" v-model="todo.massage" />
    <button class="btnDelete" @click="emitDelete"></button>
  </div>
</template>
<script>
export default {
  props: ['todo'],
  emits: ['Delete'],

  methods: {
    emitDelete() {
      this.$emit('Delete', this.todo.id);
    },
    isVisible(todo) {
      if (todo) {
        if (this.filter == 'All') return true;
        else {
          return this.filter == 'Active' ? todo.completed : !todo.completed;
        }
      }
    },
  },
};
</script>
<style scoped>
.todo-item {
  display: grid;
  grid-template-columns: minmax(2rem, 15%) minmax(2rem, 75%) minmax(2rem, 10%);
  grid-template-rows: 35px;
  background-color: var(--todo-bg);
  border-bottom: 1px solid var(--todo-border);
  color: var(--todo-text);
  padding: 12px;
}

.checkbox {
  position: relative;
  width: 1.5rem;
  height: 1.5rem;
  align-self: center;
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

.todo-item:hover .btnDelete {
  visibility: visible;
}

.todo-item .checkbox input:checked + .checkbox-icon:after {
  background-image: url(/icons/icon-check.svg),
    linear-gradient(to right bottom, hsl(192, 100%, 67%), hsl(280, 87%, 65%));
  background-repeat: no-repeat;
  background-position: center;
  margin: 0px;
}

.message {
  all: unset;
}

.todo-item .checkbox:has(input:checked) + .message {
  color: var(--text-color-secondary);
  text-decoration: line-through;
}

.btnDelete {
  background-color: transparent;
  all: unset;
  background-image: url('/icons/icon-cross.svg');
  background-repeat: no-repeat;
  background-position: center;
  cursor: pointer;
  visibility: hidden;
  font-size: 16px;
}
</style>
