<template>
  <form @submit.prevent="onSubmit">
    <label for="new-todo">New Todo</label>
    <input name="newTodo" id="new-todo" v-model="task.title" type="text" />
    <button>Add new todo</button>
  </form>

  <ul>
    <li v-for="task in todoList" v-bind:key="task.title">
      {{ task.title }}
    </li>
  </ul>
</template>

<script lang="ts">
import { ref, Ref } from "vue";
import { Task } from "./models/Task";

interface TodoApp {
  onSubmit: () => void;
  task: Ref<Task>;
  todoList: Ref<Task[]>;
}
export default {
  setup(): TodoApp {
    const task = ref<Task>({} as Task);
    const todoList = ref<Task[]>([]);

    const onSubmit = () => {
      todoList.value.push({
        title: task.value.title,
        done: false,
        deleted: false,
      });
    };

    return {
      onSubmit,
      todoList,
      task,
    };
  },
};
</script>
