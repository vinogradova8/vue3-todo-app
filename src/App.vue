<script>
import { title } from 'process';
import todos from './components/data/todos';

export default {
  data() {
    return {
      todos,
      title: '',
    };
  },

  computed: {
    activeTodos() {
      return this.todos.filter((todo) => !todo.completed);
    },

    completedTodos() {
      return this.todos.filter((todo) => todo.completed);
    },
  },

  methods: {
    handleSubmit() {},
  },
};
</script>

<template>
  <div class="todoapp">
    <h1 class="todoapp__title">todos</h1>

    <div class="todoapp__content">
      <header class="todoapp__header">
        <button
          type="button"
          class="todoapp__toggle-all"
          :class="{ active: activeTodos.length === 0 }"
          data-cy="ToggleAllButton"
        />

        <form @submit.prevent="handleSubmit">
          <input
            data-cy="NewTodoField"
            type="text"
            class="todoapp__new-todo"
            placeholder="What needs to be done?"
            v-model="title"
          />
        </form>
      </header>

      <section class="todoapp__main" data-cy="TodoList">
        <div
          v-for="(todo, index) of todos"
          :key="todo.id"
          data-cy="Todo"
          class="todo"
          :class="{ completed: todo.completed }"
        >
          <label class="todo__status-label">
            <input
              data-cy="TodoStatus"
              type="checkbox"
              v-model="todo.completed"
              class="todo__status"
            />
          </label>

          <form v-if="false">
            <input
              data-cy="TodoTitleField"
              type="text"
              class="todo__title-field"
              placeholder="Empty todo will be deleted"
              value="Todo is being edited now"
            />
          </form>

          <template v-else>
            <span data-cy="TodoTitle" class="todo__title">
              {{ todo.title }}
            </span>

            <button
              type="button"
              class="todo__remove"
              data-cy="TodoDelete"
              @click="todos.splice(index, 1)"
            >
              ×
            </button>
          </template>

          <div
            data-cy="TodoLoader"
            class="modal overlay"
            :class="{ 'is-active': false }"
          >
            <div class="modal-background has-background-white-ter" />
            <div class="loader" />
          </div>
        </div>
      </section>

      <footer class="todoapp__footer" data-cy="Footer">
        <span class="todo-count" data-cy="TodosCounter">
          {{ activeTodos.length }} items left
        </span>

        <nav class="filter" data-cy="Filter">
          <a href="#/" class="filter__link selected" data-cy="FilterLinkAll">
            All
          </a>

          <a href="#/active" class="filter__link" data-cy="FilterLinkActive">
            Active
          </a>

          <a
            href="#/completed"
            class="filter__link"
            data-cy="FilterLinkCompleted"
          >
            Completed
          </a>
        </nav>

        <button
          type="button"
          class="todoapp__clear-completed"
          data-cy="ClearCompletedButton"
          v-if="completedTodos.length > 0"
        >
          Clear completed
        </button>
      </footer>
    </div>

    <div
      data-cy="ErrorNotification"
      class="notification is-danger is-light has-text-weight-normal"
    >
      <button data-cy="HideErrorButton" type="button" class="delete" />
      Unable to load todos
      <br />
      Title should not be empty
      <br />
      Unable to add a todo
      <br />
      Unable to delete a todo
      <br />
      Unable to update a todo
    </div>
  </div>
</template>
