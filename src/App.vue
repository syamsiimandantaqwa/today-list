<script setup>
import { ref, onMounted, watch} from "vue";
import Todo from './components/Todo.vue'

const todos = ref([]);
const isActive = ref(false);
const title = ref("")
const desc = ref("")

const addTodo = (event) => {
  const data = {
    id: +new Date(),
    title: event.target.title.value,
    desc: event.target.desc.value,
    isDone: false
  }

  todos.value.push(data);
  saveTodos(todos.value);

  title.value = ""
  desc.value = ""
  isActive.value = false;
}

const completedTodo = (id, title, desc) => {
  const todoIndex = todos.value.findIndex(todo => todo.id === id);
  todos.value[todoIndex] = { id, title, desc, isDone: true};
  saveTodos(todos.value) 
}

const deleteTodo = (id) => {
  const data = todos.value.filter(todo => todo.id !== id);
  saveTodos(data);
  todos.value = getTodos();
}

const saveTodos = (todos) => {
  localStorage.setItem("today-list", JSON.stringify(todos));
}

const getTodos = () => {
  return JSON.parse(localStorage.getItem("today-list") || "[]");
}

onMounted(() => {
  todos.value = getTodos();
})

watch([title, desc], () => {
  todos.value = getTodos()
})

</script>

<template>
  <div class="container-xl flex mx-auto">
    <div class="min-h-[100vh] w-[250px] p-[20px] bg-cyan-500 ">
      <h1 class="font-bold mb-[50px]">TODAY-LIST</h1>
      <ul class="flex flex-col gap-y-[15px] text-sm">
        <li><a href="#" class="text-white">Home</a></li>
        <li><a href="#">Schedule</a></li>
        <li><a href="#">Calender</a></li>
      </ul>
    </div>

    <div class="bg-slate-200 relative w-full h-screen overflow-auto  p-[40px]">
      <button class="z-10 bg-cyan-500 text-[50px] absolute right-[20px] font-bold text-white rounded-full text-center transition w-[80px] h-[80px]" @click="isActive = !isActive" >
        {{ isActive ? "x" : "+" }}
      </button>
      <h1 class="font-bold text-orange-600 mb-[10px] capitalize text-[1.5rem]">Belum di kerjakan</h1>
      <div class="flex flex-wrap gap-[20px] mb-[50px] mx-auto">
        <template v-if="!todos[0]">
            <p>data masih kosong..</p>
        </template>

        <template v-for="todo in todos" :key="todo.id">    
          <div v-if="todo.isDone === false ">
            <Todo v-bind="todo" @delete="deleteTodo" @done="completedTodo"/>
          </div>
        </template>

          <!-- form input -->
        <form @submit.prevent="addTodo" v-show="isActive" class="flex flex-col gap-y-2 h-[150px]">
          <input required class="p-2 rounded-md" type="text" name="title" placeholder="masukan judul" v-model="title">
          <textarea required class="resize-none rounded-md p-2" rows="3"  placeholder="masukan deskripsi" name="desc" v-model="desc" ></textarea>
          <button type="submit" class="uppercase text-white rounded-full py-2 ring-1 bg-green-500">simpan</button>
        </form>
      </div>

      <h1 class="font-bold text-green-600 mb-[10px] capitalize text-[1.5rem]">sudah di kerjakan</h1>
      <div class="flex flex-wrap gap-[20px]">
      <template v-for="todo in todos" :key="todo.id">    
          <div v-if="todo.isDone === true ">
            <Todo v-bind="todo" @delete="deleteTodo" @done="completedTodo"/>
          </div>
        </template>
      </div>
   </div>
  </div>
</template>

