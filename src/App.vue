<script setup>

import { ref, useTemplateRef } from 'vue';

const categories = ref([
  {id: 1, name: "Belanja"},
  {id: 2, name: "Olahraga"}
]);

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
    name: addCatInput.value
  });

  categories.value.push(addCatData.value);
  addCatInput.value = "";
  showAddCat();
}

</script>

<template>

  <div class="container-fluid d-flex p-0">

    <!-- CATEGORY -->
    <nav class="sidebar d-flex flex-column col-12 col-md-3 col-lg-2">
      <div>
        <!-- Logo -->
        <div class="logo">
          <a href="">
            <img src="./assets/Logo01.png">
          </a>
        </div>

        <!-- Category -->
        <div class="">
          <select class="nav-link">
            <option value="all" selected disabled>Semua Kategori</option>
            <option :value="category.name" v-for="category in categories" :key="category.id">{{ category.name }}</option>
          </select>
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

   <!-- TASK -->
    <main class="main-content col-12 col-md-6 col-lg-7">
      <!-- Header -->
      <div class="header">
        <h2>Tugasku</h2>
        <div class="header-icons">
          <i class="fas fa-plus" title="Tambah Tugas Baru"></i>
          <i class="fas fa-trash-alt" title="Hapus Tugas"></i>
        </div>
      </div>
      <!-- Task -->
      <ul class="task-list">
        <li>
          <label><input type="checkbox"/>Tugas</label>
          <i class="fas fa-trash-alt trash-icon"></i>
        </li>
        <li>
          <label><input type="checkbox"/>Tugas</label>
          <i class="fas fa-trash-alt trash-icon"></i>
        </li>
      </ul>
      <!-- Completed Task -->
      <div aria-controls="completed-tasks" aria-expanded="true" class="completed-header" role="button" tabindex="0">
        <strong>Tugas Selesai</strong>
        <i class="fas fa-chevron-down"></i>
      </div>

      <ul class="task-list" id="completed-tasks">
        <li>
          <label class="completed"><input checked="" type="checkbox"/>Tugas Selesai</label>
          <i class="fas fa-trash-alt trash-icon"></i>
        </li>
      </ul>

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

