<template>
  <h1>ToDo App</h1>
  <AddTodo
    v-model:todo-item="todoItem"
    @add-todo="addTodo"
    :editedTodoIndex="editedTodoIndex"
  />
  <h2>ToDo List</h2>

  <TodoList
    :todos="todos"
    @edit-todo="editTodo"
    @delete-todo="deleteTodo"
    @select-todo="selectTodo"
    @deleteSelectedTodos="deleteSelectedTodos"
  />
</template>

<script>
import { ref } from "vue";
import AddTodo from "./components/AddTodo.vue";
import TodoList from "./components/TodoList.vue";

export default {
  components: { AddTodo, TodoList },
  setup() {
    const todos = ref([]);
    const todoItem = ref(null);
    const checkedTodoItems = ref([]);
    const editedTodoIndex = ref(null);

    const addTodo = () => {
      if (todoItem.value)
        if (editedTodoIndex.value != null) {
          // edit
          todos.value[editedTodoIndex.value] = todoItem.value;
        } else {
          // add
          todos.value.push(todoItem.value);
          todoItem.value = "";
        }
      reset();
    };

    const editTodo = (i) => {
      editedTodoIndex.value = i;
      todoItem.value = todos.value[i];
    };

    const selectTodo = (i) => {
      if (
        !checkedTodoItems.value.length ||
        !checkedTodoItems.value.find((todo, key) => i === key)
      ) {
        checkedTodoItems.value.push(i);
      } else {
        checkedTodoItems.value = checkedTodoItems.value.filter(
          (todo, key) => i !== key
        );
      }
    };

    const deleteTodo = (i) => {
      if (window.confirm("Are you sure?"))
        todos.value = todos.value.filter((todo, key) => key !== i);
      reset();
    };

    const deleteSelectedTodos = () => {
      if (window.confirm("Are you sure?"))
        todos.value = todos.value.filter((todo, index) => {
          return !checkedTodoItems.value.includes(index);
        });
      reset();
    };

    const reset = () => {
      todoItem.value = null;
      editedTodoIndex.value = null;
    };

    return {
      todos,
      todoItem,
      addTodo,
      editTodo,
      selectTodo,
      deleteTodo,
      editedTodoIndex,
      checkedTodoItems,
      deleteSelectedTodos,
    };
  },
};
</script>

<style lang="scss">
$border: 2px solid
  rgba(
    $color: white,
    $alpha: 0.35,
  );
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: #27292d;
$textColor: white;
$primaryColor: #a0a4d9;
$secondTextColor: #1f2023;
body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: $backgroundColor;
  color: $textColor;
  #app {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;
    h1 {
      font-weight: bold;
      font-size: 28px;
      text-align: center;
    }
    form {
      display: flex;
      flex-direction: column;
      width: 100%;
      label {
        font-size: 14px;
        font-weight: bold;
      }
      input,
      button {
        height: $size5;
        box-shadow: none;
        outline: none;
        padding-left: $size2;
        padding-right: $size2;
        border-radius: $size1;
        font-size: 18px;
        margin-top: $size1;
        margin-bottom: $size2;
      }
      input {
        background-color: transparent;
        border: $border;
        color: inherit;
      }
    }
    button {
      cursor: pointer;
      background-color: $primaryColor;
      border: 1px solid $primaryColor;
      color: $secondTextColor;
      font-weight: bold;
      outline: none;
      border-radius: $size1;
    }
    h2 {
      font-size: 22px;
      border-bottom: $border;
      padding-bottom: $size1;
    }
    ul {
      padding: 10px;
      li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: $border;
        padding: $size2 $size4;
        border-radius: $size1;
        margin-bottom: $size2;
        span {
          cursor: pointer;
        }
        .done {
          text-decoration: line-through;
        }
        button {
          font-size: $size2;
          padding: $size1;
        }
      }
    }
    h4 {
      text-align: center;
      opacity: 0.5;
      margin: 0;
    }
  }
}
</style>
