<template>
  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <input type="checkbox" :id="index" @click="$emit('select-todo', index)" />
      <label :for="index"></label>
      <span>{{ todo }}</span>

      <div class="actions">
        <button type="button" class="mr-5" @click="$emit('edit-todo', index)">
          Edit
        </button>

        <button
          type="button"
          class="btn-danger"
          @click="$emit('delete-todo', index)"
        >
          Remove
        </button>
      </div>
    </li>
  </ul>

  <button @click="$emit('delete-selected-todos')" v-if="todos.length > 0">
    Delete selected todos
  </button>
  <h4 v-if="todos.length === 0">Empty list.</h4>
</template>

<script>
export default {
  props: ["todos"],
  emits: ["select-todo", "edit-todo", "delete-todo", "delete-selected-todos"],
};
</script>
