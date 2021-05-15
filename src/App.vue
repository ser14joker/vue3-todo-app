<template>
  <h1>TodoList with Vue3</h1>
  <form @submit.prevent="onSubmit">
    <label for="new-todo">New Todo</label>
    <input name="newTodo" id="new-todo" v-model="task.title" type="text" />
    <button>Add new todo</button>
  </form>

  <ol>
    <li v-for="task in todoList" v-bind:key="task.id">
      <div class="task-container">
        <h3 v-bind:class="{ done: task.done }" @click="markAsDone(task.id)">
          {{ task.title }}
        </h3>
        <button v-on:click="deleteTask(task.id)">X</button>
      </div>
    </li>
  </ol>
</template>

<script lang="ts">
import { ref, Ref } from "vue";
import { Task } from "./models/Task";

interface TodoApp {
  onSubmit: () => void;
  markAsDone: (taskId: number) => void;
  deleteTask: (taskId: number) => void;
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

    const deleteTask = (taskId: number) => {
      const index = todoList.value.findIndex(
        (task: Task) => task.id === taskId
      );
      todoList.value.splice(index, 1);
    };

    return {
      onSubmit,
      markAsDone,
      deleteTask,
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
.task-container {
  display: flex;
  align-items: center;
  gap: 12px;
}
</style>
