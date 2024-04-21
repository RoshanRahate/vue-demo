<script setup lang="ts">
import { ref } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faXmark } from '@fortawesome/free-solid-svg-icons'

defineProps<{
  id?: string
  name?: string
  placeholder?: string
  password?: boolean
  modelValue?: string | number
  helpText?: string
}>()

const tags = defineModel('tags', { type: Array<string>, default: [] })

const tagInput = ref('')

/**
 * Handles keydown event on text input
 * Add input value as tag on enter or comma press
 * remove the tags on backspace
 * @param event keydown event lister
 */
const handleKeyDown = (event: KeyboardEvent) => {
  const value = tagInput.value.trim()
  if (!value && event.key === 'Backspace' && tags.value.length > 0) {
    removeTag(tagInput.value.length - 1)
  }
  if ((value && event.key === 'Enter') || event.key === ',') {
    event.preventDefault()
    const newTags = value.split(',').map((tag) => tag.trim())
    tags.value.push(...newTags)
    tagInput.value = ''
  }
}

const removeTag = (index: number) => {
  tags.value.splice(index, 1)
}
</script>

<template>
  <div>
    <p v-if="name" class="field-name">{{ name }}</p>
    <div class="tag-container">
      <ul v-if="tags.length" class="tag-list">
        <li v-for="(tag, index) in tags" :key="index">
          <span
            >{{ tag }}
            <button class="close-btn" @click.prevent="removeTag(index)">
              <FontAwesomeIcon :icon="faXmark" />
            </button>
          </span>
        </li>
      </ul>
      <input
        class="input"
        type="text"
        v-model="tagInput"
        @keydown="handleKeyDown"
        :placeholder="placeholder"
      />
    </div>
    <p v-if="helpText" class="help-text">{{ helpText }}</p>
  </div>
</template>

<style scoped>
.tag-container {
  border: 1px solid #ccc;
  /* padding: 5px; */
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  border-radius: 8px;
}
.tag-container input {
  border: none;
  outline: none;
  flex: 1;
}
.tag-list {
  list-style: none;
  padding: 0;
  margin: 5px;
  display: inline-flex;
  flex-wrap: wrap;
}
.tag-list li {
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  border-radius: 3px;
  padding: 3px 5px;
  margin: 2px;
}
.field-name {
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--color-text);
  margin-bottom: 0.2rem;
}

.help-text {
  align-self: self-start;
  font-size: small;
  color: var(--color-text);
}

.close-btn {
  border: none;
}
</style>
