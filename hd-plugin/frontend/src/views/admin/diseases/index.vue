<script
    setup
    lang='ts'
>
import { t } from '@/locales';
import List from './List.vue'
import { useDashboardStore } from '@/store'

import { onMounted, ref,computed } from 'vue';
const dashboardStore = useDashboardStore()
const showModelAdd =  computed(() => dashboardStore.showModelAdd)  
const plants = computed(() => dashboardStore.listPlants)
const loading = ref(true)
onMounted(async () => {
  await dashboardStore.getAllPlantAction()
  loading.value = false
  console.log(plants.value)
})

// const plants: Dashboard.Plant[] = [
//     {
//         name: 'Plants 1',
//         description: 'HTTP 500 Internal Server Error Allow: GET, POST, HEAD, OPTIONS Content-Type: application/json Vary: Accept no such table: chat_plants',
//         image: '',
//         isActive: true,
//         createdAt: "2024-01-13T14:47:13.055778Z",
//         updatedAt: "2024-01-13T14:47:13.055778Z"
//     },
//     {
//         name: 'Plants 2',
//         description: 'HTTP 500 Internal Server Error Allow: GET, POST, HEAD, OPTIONS Content-Type: application/json Vary: Accept no such table: chat_plants',
//         image: '',
//         isActive: true,
//         createdAt: "2024-01-13T14:47:13.055778Z",
//         updatedAt: "2024-01-13T14:47:13.055778Z"
//     },
//     {
//         name: 'Plants 3',
//         description: 'HTTP 500 Internal Server Error Allow: GET, POST, HEAD, OPTIONS Content-Type: application/json Vary: Accept no such table: chat_plants',
//         image: '',
//         isActive: true,
//         createdAt: "2024-01-13T14:47:13.055778Z",
//         updatedAt: "2024-01-13T14:47:13.055778Z"
//     }
// ]
</script>

<template>
    <List/>
  <!-- <div class="container_dashboard">

<div class="header_dashboard">
    {{ t('dashboard.plants') }}
  </div>

  <div
        v-if="loading"
        class="flex flex-col justify-center items-center h-full"
    >
 
        <div class="spinner mt-8"></div>
    </div>


  <div v-if="showModelAdd !== true" class="flex gap-2 cursor-pointer justify-end items-center">
    <NButton
      @click="dashboardStore.showModelAdd = true"
      type="primary"
    >
      <div class="flex gap-2  items-center">
        <SvgIcon
          icon="mdi:add-bold"
          class=" text-base"
        />
        <div class="hidden md:block">{{ t('university.addUniversity') }}</div>
      </div>

    </NButton>


  </div>
  <ListPlants/>
    <div class="rounded-lg  flex gap-4">
        <template  v-if="showModelAdd !== true"    v-for="(plant, index) in plants"
          :key="index">
        
        <CardsPlants :row="plant"/>
    </template>
    <template  v-if="showModelAdd === true" >
      <AddPlants/>
    </template>
   
    </div>
    </div> -->
</template>


<style scoped>
.spinner {
    width: 56px;
    height: 56px;
    display: grid;
}

.spinner::before,
.spinner::after {
    content: "";
    grid-area: 1/1;
    background: var(--c) 50% 0,
        var(--c) 50% 100%,
        var(--c) 100% 50%,
        var(--c) 0 50%;
    background-size: 13.4px 13.4px;
    background-repeat: no-repeat;
    animation: spinner-3hs4a3 1s infinite;
}

.spinner::before {
    --c: radial-gradient(farthest-side, #474bff 92%, #0000);
    margin: 4.5px;
    background-size: 9px 9px;
    animation-timing-function: linear;
}

.spinner::after {
    --c: radial-gradient(farthest-side, #474bff 92%, #0000);
}

@keyframes spinner-3hs4a3 {
    100% {
        transform: rotate(.5turn);
    }
}
</style>