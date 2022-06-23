<template>
  <form>
    <label>New ToDo </label>
    <input
      :value="todoItem"
      @input="$emit('update:todoItem', $event.target.value)"
      name="todo_item"
      autocomplete="off"
    />
    <button type="button" @click="$emit('add-todo')">
      {{ editedTodoIndex != null ? "Update" : "Add" }}
    </button>
  </form>
</template>

<script>
export default {
  props: ["todoItem", "editedTodoIndex"],
  emits: ["update:todoItem", "add-todo"],
};
</script>
