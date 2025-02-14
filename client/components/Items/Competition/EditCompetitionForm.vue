<script setup lang="ts">
import { useEditStore } from "@/stores/editing";
import { fetchy } from "@/utils/fetchy";
import { computed, onBeforeMount, ref } from "vue";
import CompetitionForm from "./CompetitionForm.vue";

const props = defineProps<{
  competition: Record<string, string>;
}>();
const emit = defineEmits<{
  (e: "refreshCompetitions", author?: string): void;
}>();

const editStore = useEditStore();

const initialName = computed(() => props.competition.name);
const initialEndDate = computed(() => new Date(props.competition.endDate));
const initialIsLinked = ref<boolean | null>(null);

const editCompetition = async (name: string, endDate: Date, isLinked: boolean) => {
  const body: Record<string, string | boolean> = { isLinked: isLinked.toString() };

  if (name !== initialName.value) body.newName = name;
  if (endDate.getTime() !== initialEndDate.value.getTime()) body.endDate = endDate.toString();

  try {
    await fetchy(`/api/competitions/${props.competition.name}`, "PATCH", { body });
  } catch (_) {
    return;
  }
  emit("refreshCompetitions");
  editStore.resetEditing();
};

const getLink = async () => {
  try {
    initialIsLinked.value = await fetchy(`/api/links/exists`, "GET", { query: { itemId: props.competition._id } });
  } catch {
    return;
  }
};

onBeforeMount(getLink);
</script>

<template>
  <CompetitionForm :initialInfo="{ name: initialName, endDate: initialEndDate, isLinked: initialIsLinked }" :isEditing="true" @onSave="editCompetition" />
</template>
