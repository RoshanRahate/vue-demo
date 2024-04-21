<script setup lang="ts">
import { ref, watch } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faXmark } from '@fortawesome/free-solid-svg-icons'
import MyInput from './MyInput.vue'
import TagInput from './TagInput.vue'

const dialogElement = ref<HTMLDialogElement>()
const emit = defineEmits(['submit'])

const visible = defineModel<boolean>('visible')

const id = ref('')
const type = ref('')
const name = ref('')
const toppings = ref<string[]>([])
const description = ref('')

watch(visible, () => {
  if (visible.value) {
    dialogElement.value?.showModal()
  } else {
    dialogElement.value?.close()
  }
})

const submitData = () => {
  emit('submit', {
    id: id.value,
    type: type.value,
    name: name.value,
    toppings: toppings.value,
    description: description.value
  })
  id.value = ''
  type.value = ''
  toppings.value = []

  name.value = ''
  description.value = ''
  visible.value = false
}
</script>

<template>
  <Teleport to="body">
    <dialog ref="dialogElement" @close="visible = false" class="dialog">
      <div class="header">
        <h2 class="heading">Add Data</h2>
        <button class="close-btn" @click="dialogElement?.close()">
          <FontAwesomeIcon :icon="faXmark" />
        </button>
      </div>
      <div class="divider" />

      <div class="form">
        <MyInput v-model="id" name="Product Id" placeholder="Id" />

        <MyInput v-model="type" name="Product Type" placeholder="Type" />

        <MyInput v-model="name" name="Product Name" placeholder="Name" />

        <TagInput
          v-model:tags="toppings"
          name="Toppings"
          placeholder="Commas seprated list of Toppings"
          help-text="Press Enter or Comma to add text as a tag"
        />

        <MyInput v-model="description" name="Description" placeholder="Description" />
      </div>
      <div class="divider" />
      <div class="footer">
        <button class="button btn-secondary" @click="dialogElement?.close()">Close</button>
        <button class="button btn-primary" @click="submitData">Save Data</button>
      </div>
    </dialog>
  </Teleport>
</template>

<style scoped>
.dialog {
  justify-self: center;
  align-self: center;
  width: 40%;
  border-radius: 8px;
  padding: 0;
  border: none;
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
  background-color: var(--color-background-soft);
}

.form {
  margin: 1rem;
  display: grid;
  grid-gap: 1rem;
}

.button {
  justify-self: right;
}

.divider {
  height: 2px;
  width: 100%;
  background-color: var(--color-background-mute);
}
.header {
  margin: 0.5rem 1rem;
  display: flex;
  justify-content: space-between;
}
.heading {
  color: var(--color-heading);
}
.footer {
  margin: 0.5rem 0.5rem;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

dialog::backdrop {
  background-color: rgb(0 0 0 / 25%);
}

.close-btn {
  border: none;
  background-color: var(--color-background-soft);
}
</style>
