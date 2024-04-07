<script lang="ts" setup>
import { onMounted } from "vue";
import { useSupabaseClient } from "../src/composables/supabase";
import { useUserStore } from "./store/user";

import AppMenu from "./components/AppMenu.vue";

const userStore = useUserStore();

onMounted(async () => {
  const { data } = await useSupabaseClient.auth.getSession();

  if (data && data.session) userStore.setUserSession(data.session);

  //keep track of state change to handle updated tokens/invalidation
  useSupabaseClient.auth.onAuthStateChange((_, _session) => {
    userStore.setUserSession(_session);
  });
});
</script>
<template>
  <v-app>
    <app-menu />
    <v-main><router-view /></v-main
  ></v-app>
</template>
