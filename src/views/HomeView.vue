<script setup lang="ts">
import { ref, computed } from 'vue'
import MyTable, { type TableColumnData } from '../components/MyTable.vue'
import AddDataModal from '../components/AddDataModal.vue'
import MyInput from '@/components/MyInput.vue'
// import TagInput from '@/components/TagInput.vue'

interface TableData {
  id: number
  type: string
  name: string
  description: string
  price: number
  toppings: string[]
}

// default data
const dataArray = ref<TableData[]>([
  {
    id: 1,
    name: 'Chocolate Cake',
    type: 'Chocolate',
    toppings: ['Chocolate chips', 'Chocolate ganache'],
    price: 25.99,
    description: 'A delicious chocolate cake with chocolate chips and ganache.'
  },
  {
    id: 2,
    name: 'Vanilla Cake',
    type: 'Vanilla',
    toppings: ['Whipped cream', 'Sprinkles'],
    price: 20.49,
    description: 'A classic vanilla cake with whipped cream and sprinkles.'
  },
  {
    id: 3,
    name: 'Red Velvet Cake',
    type: 'Red Velvet',
    toppings: ['Cream cheese frosting', 'Red velvet crumbs'],
    price: 28.99,
    description: 'A rich red velvet cake with cream cheese frosting and red velvet crumbs.'
  },
  {
    id: 4,
    name: 'Carrot Cake',
    type: 'Carrot',
    toppings: ['Cream cheese frosting', 'Chopped nuts'],
    price: 30.99,
    description: 'A moist carrot cake with cream cheese frosting and chopped nuts.'
  },
  {
    id: 5,
    name: 'Fruit Cake',
    type: 'Mixed Fruit',
    toppings: ['Assorted fruits', 'Whipped cream', 'Cream cheese frosting', 'Chopped nuts'],
    price: 22.99,
    description: 'A delightful fruit cake with assorted fruits and whipped cream.'
  }
])

const columns: TableColumnData[] = [
  { id: 'id', name: 'Product Id', sortable: true, width: '100px' },
  { id: 'name', name: 'Name', sortable: true, width: '150px' },
  { id: 'type', name: 'Type', sortable: true, minWidth: '100px' },
  { id: 'toppings', name: 'Toppings', sortable: true, minWidth: '100px' },
  { id: 'description', name: 'Description', minWidth: '200px' },
  { id: 'action', name: 'Actions', minWidth: '200px' }
]
const searchInput = ref('')
const showModal = ref(false)

const filteredData = computed(() => {
  if (!searchInput.value) return dataArray.value

  return dataArray.value.filter((item) => {
    return JSON.stringify([item.name, item.type, item.description])
      .toLocaleLowerCase()
      .includes(searchInput.value.toLocaleLowerCase())
  })
})

const updateData = (data: TableData) => {
  dataArray.value.push(data)
}
</script>

<template>
  <div class="wrapper">
    <h1 class="header">Table Demo</h1>

    <MyInput
      class="search-input"
      v-model="searchInput"
      placeholder="Search Results"
      help-text="Search by ID, Type, Name or Toppings"
    />

    <button @click="showModal = true" class="button btn-primary add-btn">Add Data</button>

    <MyTable :columns="columns" :table-data="filteredData" empty-text="No results found">
      <template #table(toppings)="{ data }">
        <div class="pill-wrapper">
          <p v-for="(item, idx) in data" :key="idx" class="pill">{{ item }}</p>
        </div>
      </template>
    </MyTable>
    <AddDataModal v-model:visible="showModal" @submit="updateData" />
  </div>
</template>

<style scoped>
.wrapper {
  margin: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.header {
  display: flex;
  place-items: center;
  /* padding-right: calc(var(--section-gap) / 2); */
  margin: 0.5rem;
  font-weight: 500;
}

.search-input {
  align-self: center;
  width: 70%;
  margin-top: 1rem;
}
.pill-wrapper {
  display: flex;
  flex-wrap: wrap;
}
.pill {
  padding: 0.2rem 0.5rem;
  margin: 0.3rem;
  width: fit-content;
  border-radius: 2rem;
  background-color: bisque;
  color: #012d21;
  font-size: 15px;
  text-wrap: nowrap;
}
.add-btn {
  align-self: end;
  margin-bottom: 0.5rem;
}
</style>
