<script setup lang="ts">
import router from "@/router";
import { useUserStore } from "@/stores/user";
import { storeToRefs } from "pinia";
import UpdateUserForm from "../components/Setting/UpdateUserForm.vue";

const { currentUsername } = storeToRefs(useUserStore());
const { logoutUser, deleteUser } = useUserStore();

async function logout() {
  await logoutUser();
  void router.push({ name: "Home" });
}

async function delete_() {
  await deleteUser();
  void router.push({ name: "Home" });
}
</script>

<template>
  <main class="column">
    <h1>Settings for {{ currentUsername }}</h1>
    <article>
      <UpdateUserForm />
      <div class="row">
        <button class="pure-button pure-button-primary btn" @click="logout">Logout</button>
        <button class="button-error pure-button" @click="delete_">Delete User</button>
      </div>
    </article>
  </main>
</template>

<style scoped>
article {
  align-items: center;
}

.row {
  display: flex;
  gap: 1em;
}
</style>
