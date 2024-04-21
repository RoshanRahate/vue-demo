<script setup lang="ts">
import { ref, computed } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import {
  faSort,
  faArrowUpShortWide,
  faArrowDownShortWide,
  type IconDefinition
} from '@fortawesome/free-solid-svg-icons'

export interface TableColumnData {
  id: string
  name: string
  sortable?: boolean
  width?: string
  minWidth?: string
}

const props = defineProps<{
  columns: TableColumnData[]
  tableData: any[]
  emptyText?: string
}>()

const sortByField = ref<string>('')
const sortDesc = ref(false)
const sortIcon = ref<IconDefinition>(faSort)

const sortedData = computed(() => {
  if (!sortByField.value) return props.tableData

  return [...props.tableData].sort((a, b) => {
    if (sortDesc.value) {
      return a[sortByField.value] > b[sortByField.value] ? -1 : 1
    } else {
      return a[sortByField.value] > b[sortByField.value] ? 1 : -1
    }
  })
})

const sortColumn = (field: string) => {
  if (sortByField.value === field) {
    sortDesc.value = !sortDesc.value
    sortIcon.value = !sortDesc.value ? faArrowDownShortWide : faArrowUpShortWide
  } else {
    sortByField.value = field
    sortDesc.value = false
    sortIcon.value = faArrowDownShortWide
  }
}
</script>

<template>
  <div class="table-container">
    <table class="table alternate-rows">
      <thead>
        <tr class="header-row">
          <th v-for="(column, idx) in columns" :key="idx" class="table-heading">
            <div
              class="column-heading"
              @click="column.sortable && sortColumn(column.id)"
              :style="{ width: column.width, 'min-width': column.minWidth }"
            >
              <span class="heading-text">
                {{ column.name }}
              </span>
              <FontAwesomeIcon
                v-if="column.sortable"
                :icon="sortByField === column.id ? sortIcon : faSort"
              />
            </div>
          </th>
        </tr>
      </thead>
      <tbody v-if="sortedData.length" class="table-body">
        <tr v-for="(data, idx) in sortedData" :key="idx" class="table-row">
          <td
            v-for="(column, idx) in columns"
            :key="idx"
            class="table-data"
            :style="{ width: column.width, minWidth: column.minWidth }"
          >
            <slot :name="`table(${column.id})`" :data="data[column.id]">
              {{ data[column.id] || '-' }}
            </slot>
          </td>
        </tr>
      </tbody>
      <tbody v-else>
        <tr>
          <td colspan="100%" class="empty-data-text">{{ emptyText }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
/* TODO: Dark mode support for Table  */
.table-container {
  overflow-x: auto;
  width: 100%;
  border-radius: 0.3rem;
}

.table {
  width: 100%;
  text-align: left;
  background-color: var(--color-background);
  overflow-x: scroll;
  padding: 0px;
}

.alternate-rows tr:nth-child(even) {
  background-color: var(--color-background-soft);
}

.table-heading {
  padding: 0.5rem;
  margin: 0;
  min-width: fit-content;
  background-color: var(--color-background-mute);
}

.table-heading:first-child {
  border-top-left-radius: 0.3rem;
}

.table-heading:last-child {
  border-top-right-radius: 0.3rem;
}

.column-heading {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.heading-text {
  font-weight: bold;
}

.table-data {
  padding: 0.5rem;
}

.table-body {
  color: var(--color-text);
}

.empty-data-text {
  font-size: medium;
  font-weight: 600;
  text-align: center;
  padding: 0.5rem;
  background-color: var(--color-background-soft);
}
</style>
