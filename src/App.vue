<script setup>

import { computed, ref, useTemplateRef } from 'vue';
// DATA
let categories = ref([
  {id: 1, name: "Belanja"},
  {id: 2, name: "Olahraga"},
  {id: 3, name: "Tugas Kuliah"}
]);

const tasks = ref([
  {id: 1, name: "Beras", descript: "Anak Daro", category: "Belanja", done: false},
  {id: 2, name: "Sabun", descript: "Detol", category: "Belanja", done: false},
  {id: 3, name: "Tisu", descript: "", category: "Belanja", done: true},
  {id: 4, name: "Jogging", descript: "", category: "Olahraga", done: false},
  {id: 5, name: "Fitness", descript: "", category: "Olahraga", done: true},
  {id: 6, name: "Badminton", descript: "", category: "Olahraga", done: false},
  {id: 7, name: "PBK", descript: "", category: "Tugas Kuliah", done: false},
  {id: 8, name: "PBO", descript: "", category: "Tugas Kuliah", done: false}
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
    name: addCatInput.value
  });

  categories.value.push(addCatData.value);
  addCatInput.value = "";
  showAddCat();
}

function removeCategory(cat){
  tasks.value = tasks.value.filter((tasks) => tasks.category !== cat.name);
  categories.value = categories.value.filter((categories) => categories !== cat);
}

// CONNECT TASK & CATEGORY

let activeCategory = ref('');

const filteredTasks = computed(() => {
  return tasks.value.filter((tasks) => tasks.category == activeCategory.value);
});

// TASK

const complatedFilteredTasks = computed(()=>{
  return filteredTasks.value.filter((filteredTasks) => filteredTasks.done == true)
});

const unComplatedFilteredTasks = computed(()=>{
  return filteredTasks.value.filter((filteredTasks) => filteredTasks.done == false)
});

function removeTask(task){
  return tasks.value = tasks.value.filter((tasks) => tasks !== task);
}

let showComplatedTask = ref(true);



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
              <li v-for="category in categories" :key="category.id" @click="activeCategory = category.name">
                <button> 
                  <div v-if="category.name == activeCategory" class="active-category">{{ category.name }}</div>
                  <div v-else >{{ category.name }}</div>
                  <i class="bi bi-x" @click="removeCategory(category)" title="Hapus Kategori"></i>
                </button>
              </li>
            </ul>
        </div>
      </div>

      <!-- Add Category -->

      <div>
        <button @click="showAddCat" class="bottom-btn" :class="addCatClass"> <i v-if="isAddCat == false" class="bi bi-chevron-up"></i><i v-if="isAddCat == true" class="bi bi-chevron-down"></i>Tambah Kategori Baru </button>
        <div class="bottom-btn">
          <input v-model="addCatInput" maxlength="20" ref="inputCategory" aria-label="Nama Kategori Baru" placeholder="Nama Kategori Baru" type="text"/>
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
          <li v-for="unComplatedFilteredTask in unComplatedFilteredTasks" :key="unComplatedFilteredTask.id">
            <label :class="{ done: unComplatedFilteredTask.done }"><input type="checkbox" v-model="unComplatedFilteredTask.done" />
              <div class="task-content">
                {{ unComplatedFilteredTask.name }} 
                <span class="task-content-descript">
                  {{ unComplatedFilteredTask.descript }}
                </span>
              </div>
            </label>
            <button @click="removeTask(unComplatedFilteredTask)"><i class="bi bi-x" title="Hapus Tugas"></i></button>
          </li>
        </ul>

        <!-- Completed Task -->

        <div class="completed-header" @click="showComplatedTask =! showComplatedTask">
            <p>Tugas Selesai</p>
          <div class="header-icons">
            <i v-if="showComplatedTask" class="bi bi-chevron-up"></i>
            <i v-else class="bi bi-chevron-down"></i>
          </div>
        </div>

        <ul class="task-list" v-if="showComplatedTask">
          <li v-for="complatedFilteredTask in complatedFilteredTasks" :key="complatedFilteredTask.id" style="opacity: 50%;">
            <label :class="{ done: complatedFilteredTask.done }"><input type="checkbox" v-model="complatedFilteredTask.done" />
              <div class="task-content">
                {{ complatedFilteredTask.name }} 
                <span class="task-content-descript">
                  {{ complatedFilteredTask.descript }}
                </span>
              </div>
            </label>
            <button @click="removeTask(complatedFilteredTask)"><i class="bi bi-x" title="Hapus Tugas"></i></button>
          </li>
        </ul>
      </div>

    </main>

    <!-- CREATE UPDATE -->
    <aside class="add-task-section col-12 col-md-3 col-lg-3">
      <h5>Tambah Tugas Baru</h5>
      <p></p>
      <input aria-label="Nama Tugas" placeholder="Nama Tugas" type="text"/>
      <textarea aria-label="Deskripsi Tugas" placeholder="Deskripsi Tugas"></textarea>
      <button type="button">Tambahin ke Tugasku</button>
    </aside>

  </div>

</template>

