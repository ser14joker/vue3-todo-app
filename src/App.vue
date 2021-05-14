<template>
  <h1>TodoList with Vue3</h1>
  <form @submit.prevent="onSubmit">
    <label for="new-todo">New Todo</label>
    <input name="newTodo" id="new-todo" v-model="task.title" type="text" />
    <button>Add new todo</button>
  </form>

  <ul>
    <li
      v-for="task in todoList"
      v-bind:key="task.id"
      @click="markAsDone(task.id)"
      :class="{ done: task.done }"
    >
      <h3>{{ task.title }}</h3>
    </li>
  </ul>
</template>

<script lang="ts">
import { ref, Ref } from "vue";
import { Task } from "./models/Task";

interface TodoApp {
  onSubmit: () => void;
  markAsDone: (taskId: number) => void;
  task: Ref<Task>;
  todoList: Ref<Task[]>;
}
export default {
  setup(): TodoApp {
    const task = ref<Task>({} as Task);
    const todoList = ref<Task[]>([]);

    const onSubmit = () => {
      todoList.value.push({
        id: Date.now(),
        title: task.value.title,
        done: false,
        deleted: false,
      });
      task.value.title = "";
    };

    const markAsDone = (taskId: number) => {
      todoList.value = todoList.value.map((task) => ({
        ...task,
        done: taskId === task.id ? !task.done : task.done,
      }));
    };
    return {
      onSubmit,
      markAsDone,
      todoList,
      task,
    };
  },
};
</script>
<style>
.done {
  text-decoration: line-through;
  color: crimson;
}
li {
  cursor: pointer;
}
</style>
