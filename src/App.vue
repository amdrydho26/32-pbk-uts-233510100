<script setup>

import { computed, ref, useTemplateRef } from 'vue';
// DATA
let categories = ref([
  {id: 1, name: "Belanja", active: false},
  {id: 2, name: "Olahraga", active: false}
]);

const tasks = ref([
  {id: 1, name: "Beras", category: "Belanja", done: false},
  {id: 2, name: "Sabun", category: "Belanja", done: false},
  {id: 3, name: "Tisu", category: "Belanja", done: true}
])

// CATEGORY
let isAddCat = ref(false);
let addCatClass = ref('addCat');
const inCat = useTemplateRef('inputCategory');
let addCatInput = ref('');

function showAddCat(){
  if (addCatClass.value == 'addCat'){
    addCatClass.value = 'addCatActive';
    inCat.value.focus();
  } else {
    addCatClass.value = 'addCat';
    addCatInput.value = "";
  }
  
  isAddCat.value =! isAddCat.value;
}

function addCat(){
  const addCatData = ref({
    id: (categories.value.length + 1),
    name: addCatInput.value,
    active: false
  });

  categories.value.push(addCatData.value);
  addCatInput.value = "";
  showAddCat();
}

function activeCategory(category){
  console.info("Active");
}

// TASK

const complatedTasks = computed(()=>{
  return tasks.value.filter((data) => data.done == true)
})

</script>

<template>

  <div class="container-fluid d-flex p-0">

    <!-- C A T E G O R Y -->

    <nav class="sidebar d-flex flex-column col-12 col-md-3 col-lg-2">
      <div>

        <!-- Logo -->

        <div class="logo">
          <a href="">
            <img src="./assets/Logo01.png">
          </a>
        </div>

        <!-- Category -->

        <div class="category">
            <ul>
              <li v-for="category in categories" :key="category.id" @click="activeCategory(category.id)">
                <button>{{ category.name }}</button>
              </li>
            </ul>
        </div>

      </div>

      <!-- Add Category -->

      <div>
        <button @click="showAddCat" class="bottom-btn" :class="addCatClass"> <i v-if="isAddCat == false" class="bi bi-chevron-up"></i><i v-if="isAddCat == true" class="bi bi-chevron-down"></i>Tambah Kategori Baru </button>
        <div class="bottom-btn">
          <input v-model="addCatInput" ref="inputCategory" aria-label="Nama Kategori Baru" placeholder="Nama Kategori Baru" type="text"/>
          <button @click="addCat" type="button"><i class="bi bi-check-lg"></i></button>
        </div>
      </div>
   </nav>

   <!-- T A S K -->

    <main class="main-content col-12 col-md-6 col-lg-7">

      <!-- Header -->

      <div class="header">

        <h1>Tugasku</h1>
        <div class="header-icons">
          <i class="bi bi-plus" title="Tambah Tugas Baru"></i>
        </div>

      </div>

      <!-- Task -->

      <div class="task">
        <ul class="task-list">
          <li v-for="task in tasks" :key="task.id">
            <label :class="{ done: task.done }"><input type="checkbox" v-model="task.done" />{{ task.name }}</label>
            <button><i class="bi bi-x" title="Hapus Tugas"></i></button>
          </li>
        </ul>

        <!-- Completed Task -->

        <div class="completed-header">
          <p>Tugas Selesai</p>
          <div class="header-icons">
            <i class="bi bi-chevron-down"></i>
          </div>
        </div>

        <ul class="task-list">
          <li v-for="complatedTask in complatedTasks" :key="complatedTask.id" style="opacity: 50%;">
            <label :class="{ done: complatedTask.done }"><input type="checkbox" v-model="complatedTask.done" />{{ complatedTask.name }}</label>
            <button><i class="bi bi-x" title="Hapus Tugas"></i></button>
          </li>
        </ul>
      </div>

    </main>

    <!-- CREATE UPDATE -->
    <aside class="add-task-section col-12 col-md-3 col-lg-3">
      <h5>Tambah Tugas Baru</h5>
      <input aria-label="Nama Tugas" placeholder="Nama Tugas" type="text"/>
      <textarea aria-label="Deskripsi Tugas" placeholder="Deskripsi Tugas"></textarea>
      <button type="button">Tambahin ke Tugasku</button>
    </aside>

  </div>

</template>

