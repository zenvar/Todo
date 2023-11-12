<template>
  <main class="app">
    <section class="greeting">
      <h3 class="title">✍️无所事事</h3>
    </section>

    <div class="input-section">
      <section class="create-todo">
        <form @submit.prevent="addTodo">
          <h3>你干嘛~🙂?</h3>
          <input
            type="text"
            placeholder="建议：唱跳rap💃打篮球🏀"
            v-model="text"
          />

          <input type="submit" value="整点活儿🍟" />
        </form>
      </section>
    </div>

    <div class="todo-section">
      <section class="todo-list">
        <h2 v-show="todos.length === 0">“今日无事”--1789年7月14日，法国国王路易十六偷得浮生半日闲在日记中如是写道😪</h2>

        <div class="list">
          <div
            v-for="todo in todos"
            :class="`todo-item ${todo.done && 'done'}`"
          >
            <label>
              <input type="checkbox" v-model="todo.done" />
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.todo" />
            </div>

            <div class="actions">
              <button class="delete" @click="deleteTodo(todo)">Delete</button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
// ref --> for state management
// onMounted --> to execute a command once the page starts
// computed --> for mathematical computing
// watch --> an observable which watches for page changes

const todos = ref([]);
const text = ref("");

function addTodo() {
  if (text.value.trim() === "") {
    return;
  }

  todos.value.unshift({
    todo: text.value,
    done: false,
  });

  text.value = "";
}

function deleteTodo(todo) {
  todos.value = todos.value.filter((x) => x !== todo);
}

watch(
  todos,
  (newTodoValue) => {
    localStorage.setItem("todos", JSON.stringify(newTodoValue));
  },
  { deep: true }
);

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>