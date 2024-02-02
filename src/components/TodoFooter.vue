<template lang="">
  <div class="todo-footer">
    <div class="footer">
      <p>{{ remainingTodo }} items left</p>
      <div class="filter">
        <button
          @click="emitFilter('All')"
          :class="{ selected: filter === 'All' }"
        >
          All
        </button>
        <button
          @click="emitFilter('Active')"
          :class="{ selected: filter === 'Active' }"
        >
          Active
        </button>
        <button
          @click="emitFilter('Completed')"
          :class="{ selected: filter === 'Completed' }"
        >
          Completed
        </button>
      </div>
      <button @click="$emit('ClearCompleted')">Clear completed</button>
    </div>
    <div class="footer-mobile">
      <div class="filter">
        <button
          @click="emitFilter('All')"
          :class="{ selected: filter === 'All' }"
        >
          All
        </button>
        <button
          @click="emitFilter('Active')"
          :class="{ selected: filter === 'Active' }"
        >
          Active
        </button>
        <button
          @click="emitFilter('Completed')"
          :class="{ selected: filter === 'Completed' }"
        >
          Completed
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['remainingTodo', 'filter'],
  emits: ['ChangeFilter', 'ClearCompleted'],

  methods: {
    emitFilter(filter) {
      this.$emit('ChangeFilter', filter);
    },
  },
};
</script>
<style scoped>
.footer *,
.footer-mobile * {
  background-color: transparent;
  all: unset;
}

.todo-footer * {
  background-color: var(--todo-bg);
  color: var(--text-color-secondary);
  font-size: 0.8rem;
  padding: 4px;
}

.footer-mobile {
  margin-top: 20px;
  border-radius: 6px;
  padding: 12px;
  box-shadow: 0px 10px 50px -1px rgba(0, 0, 0, 0.3);
}

.footer {
  display: grid;
  align-items: center;
  justify-items: center;
  grid-template-columns: 25% minmax(12px, 50%) 25%;
  grid-template-rows: 35px;
  padding: 12px;
  border-radius: 0 0 6px 6px;
}

.filter {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  font-weight: 700;
}

.footer .filter {
  visibility: hidden;
}

.todo-footer button {
  cursor: pointer;
}

.todo-footer button:hover {
  color: var(--text-color-hover);
}

.filter .selected {
  color: var(--text-color-selected);
}

@media (min-width: 425px) {
  .footer {
    padding: 6px;
  }

  .footer-mobile {
    display: none;
    padding: 6px;
  }

  .footer .filter {
    visibility: visible;
  }
}
</style>
