<script setup>
import { Icon } from "@iconify/vue";
import { uid } from "uid";
import { ref, watch, computed } from "vue";
import TodoCreator from "../components/TodoCreator.vue";
import TodoItem from "../components/TodoItem.vue";

const todoList = ref([]);

// to watch for reactive state, in this case todoList if it keeps updating
watch(todoList, () => {
  
},
{
  deep: true,
})

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted)
})

const createTodo = (todo) => {
    todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: null,
  });

};

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
};

const updateTodo = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
};

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
};

const deleteTodo = (todo) => {
  todoList.value = todoList.value.filter(
    (todoFilter) => todoFilter.id !== todo.id
  );
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo">
      <template #button-content>Create</template>
    </TodoCreator>
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @toggle-complete="toggleTodoComplete"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all of your to-dos!</span>
    </p>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>