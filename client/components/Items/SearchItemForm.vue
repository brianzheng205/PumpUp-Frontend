<script setup lang="ts">
import { ref } from "vue";

const props = defineProps(["legendText"]);
const emit = defineEmits(["getItemsByUser"]);

const author = ref("");
</script>

<template>
  <form @submit.prevent="emit('getItemsByUser', author)" class="pure-form">
    <fieldset>
      <legend>{{ props.legendText }}</legend>
      <input id="author" type="text" v-model="author" placeholder="Username" />
      <button type="submit" class="pure-button pure-button-primary btn-primary">Search</button>
    </fieldset>
  </form>
</template>

<style scoped>
form {
  display: flex;
  gap: 0.5em;
  padding: 1em;
  align-items: center;
}
</style>
