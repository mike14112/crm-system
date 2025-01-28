<script setup lang="ts">
import { ref } from 'vue';
import TheForm from './components/TheForm.vue';
// interface TodoRequest {
//   title?: string;
//   isDone?: boolean;  // изменение статуса задачи происходит через этот флаг
// }

interface Todo {
  id: number;
  title: string;
  created: string; // ISO date string
  isDone: boolean;
}

interface TodoInfo {
  all: number
  completed: number
  inWork: number
}

interface MetaResponse<T, N> {
  data: T[]
  info: N
  meta: {
    totalAmount: number
  }
}
const todos = ref<Todo[]>()
const todoInfo = ref<TodoInfo>({ all: 0, completed: 0, inWork: 0 })

const fetchTodos = async (): Promise<MetaResponse<Todo, TodoInfo>> => {
  const response = await fetch("https://easydev.club/api/v2/todos");
  const data: MetaResponse<Todo, TodoInfo> = await response.json();
  console.log(data.data)
  todos.value = data.data
  todoInfo.value.all = data.info.all
  todoInfo.value.completed = data.info.completed
  todoInfo.value.inWork = data.info.inWork
  return data
};
fetchTodos()

</script>

<template>
  <header class="header">
    <TheForm />
  </header>
  <main>
    <ul class="list">
      <li class="list-item">
        Все ({{ todoInfo.all }})
      </li>
      <li class="list-item">
        В работе ({{ todoInfo.inWork }})
      </li>
      <li class="list-item">
        сделано ({{ todoInfo.completed }})
      </li>
    </ul>
  </main>
</template>

<style scoped style="scss">
.header {
  display: flex;
  justify-content: center;
  align-items: center;
}

.list {
  display: flex;
  justify-content: center;
  gap: 50px;
}

.list-item {
  list-style: none;
  cursor: pointer;
}
</style>
