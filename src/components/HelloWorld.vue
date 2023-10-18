<script setup lang="ts">
import { ref, computed, defineProps, watch } from 'vue';

let todos = ref([
  {
    name: 'paint room',
    status: 'not completed',
  },
  {
    name: 'do dishes',
    status: 'not completed',
  },
  {
    name: 'do something',
    status: 'not completed',
  },
  {
    name: 'do this',
    status: 'not completed',
  },
  {
    name: 'do that',
    status: 'not completed',
  },
]);

watch(
  () => todos.value,
  (value) => {
    value.sort((a, b) => {
      if (a.status === 'not completed') return -1;
      if (b.status === 'not completed') return 0;
      return 1;
    });
  },
  {
    deep: true,
  }
);

let input = ref('');
let editInput = ref('');

function addTodo() {
  todos.value.push({
    name: input.value,
    status: 'not completed',
  });

  input.value = '';
}

function clickTodo(todo) {
  const foundTodo = todos.value.find((item) => item.name === todo.name);
  if (foundTodo.status === 'completed') {
    foundTodo.status = 'not completed';
  } else {
    foundTodo.status = 'completed';
  }
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((item) => todo.name !== item.name);
}

let showEditInput = ref(false);
let initialEditInput = ref('');
function editTodo(todo) {
  showEditInput.value = true;
  editInput.value = todo.name;
  initialEditInput.value = todo.name;
}

function submitEditTodo() {
  console.log(initialEditInput.value);
  const foundTodo = todos.value.find(
    (item) => item.name === initialEditInput.value
  );
  foundTodo.name = editInput.value;
  showEditInput.value = false;
}

1 === 1; // true
1 !== 1; // false
</script>

<template>
  <div class="flex flex-col items-center">
    <div
      v-for="todo in todos"
      :class="{
        'text-green-600': todo.status === 'completed',
      }"
      class="flex gap-5 mb-3 items-center"
    >
      <div @click="clickTodo(todo)">{{ todo.name }}</div>
      <div @click="editTodo(todo)" class="bg-orange-600 text-white rounded p-1">
        Edit
      </div>
      <div @click="deleteTodo(todo)" class="bg-red-600 text-white rounded p-1">
        Delete
      </div>
    </div>
    <input
      type="text"
      placeholder="Todo name"
      v-model="input"
      class="border rounded mb-3 p-2"
    />
    <div
      @click="addTodo"
      class="bg-green-600 rounded w-fit text-white p-2 cursor-pointer"
    >
      Add todo
    </div>
    <div v-if="showEditInput" class="flex-col flex items-center">
      <input
        type="text"
        v-model="editInput"
        class="border rounded mb-3 p-2 mt-3"
      />
      <div
        @click="submitEditTodo"
        class="bg-green-600 rounded w-fit text-white p-2 cursor-pointer"
      >
        Edit todo
      </div>
    </div>
  </div>
</template>
