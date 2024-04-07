<script lang="ts" setup>
import { onMounted } from "vue";
import { useSupabaseClient } from "../src/composables/supabase";
import { useUserStore } from "../src/store/user";

const userStore = useUserStore();

onMounted(async () => {
  const { data } = await useSupabaseClient.auth.getSession();
  if (data && data.session && data.session.user) {
    await userStore.insertProfile(data.session);
    userStore.setUserSession(data.session);
  }

  //keep track of state change to handle updated tokens/invalidation
  useSupabaseClient.auth.onAuthStateChange((_, _session) => {
    userStore.setUserSession(_session);
  });
});
</script>
<template>
  <router-view />
</template>
