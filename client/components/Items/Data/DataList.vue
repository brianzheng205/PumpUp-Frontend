<script setup lang="ts">
import Data from "./Data.vue";

const props = defineProps<{
  loaded: boolean;
  data: Array<Record<string, string>>;
}>();

const emit = defineEmits<{
  (e: "refreshData"): void;
}>();

const refreshData = () => {
  emit("refreshData");
};
</script>

<template>
  <section class="data" v-if="loaded && data.length !== 0">
    <article v-for="d in data" :key="d._id">
      <Data :data="d" @refreshData="refreshData" />
    </article>
  </section>
  <p v-else-if="loaded">No data found</p>
  <p v-else>Loading...</p>
</template>

<style scoped>
section {
  display: flex;
  flex-direction: column;
  gap: 1em;
}

section,
p,
.row {
  width: 100%;
}

article {
  background-color: var(--base-bg);
  border-radius: 1em;
  display: flex;
  flex-direction: column;
  gap: 0.5em;
  padding: 1em;
}

.data {
  padding: 1em;
}

.row {
  display: flex;
  justify-content: space-between;
  max-width: 60em;
}
</style>
