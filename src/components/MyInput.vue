<script setup lang="ts">
defineProps<{
  id?: string
  name?: string
  placeholder?: string
  password?: boolean
  modelValue?: string | number
  helpText?: string
}>()

const modelvalue = defineModel<string | number>('modelValue')
</script>

<template>
  <div>
    <p v-if="name" class="field-name">{{ name }}</p>
    <input
      class="input"
      v-bind="$attrs"
      v-model="modelvalue"
      :placeholder="placeholder"
      :type="password ? 'password' : 'text'"
    />
    <p v-if="helpText" class="help-text">{{ helpText }}</p>
  </div>
</template>

<style scoped>
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
</style>
