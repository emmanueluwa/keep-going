<script lang="ts" setup>
import { onMounted } from "vue";
import { useAppStore } from "../src/store/app";
import { storeToRefs } from "pinia";
import { useSupabaseClient } from "../src/composables/supabase";
import { useUserStore } from "./store/user";

import AppMenu from "./components/AppMenu.vue";

const userStore = useUserStore();
const appStore = useAppStore();

const { dialog, pageTitle } = storeToRefs(appStore);
const currentYear = new Date().getFullYear();

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
    <v-app-bar app style="position: relative">
      <v-app-bar-nav-icon @click="appStore.toggleDrawer()"></v-app-bar-nav-icon>
      <v-toolbar-title>💪 Keep Going - {{ pageTitle }}</v-toolbar-title>
    </v-app-bar>
    <v-main
      ><router-view />
      <v-dialog v-model="dialog.visible" :fullscreen="dialog.fullscreen">
        <v-card
          ><v-card-title>{{ dialog.title }}</v-card-title>
          <v-card-text><p v-html="dialog.contents"></p></v-card-text>
          <v-card-actions>
            <v-btn color="primary" @click="appStore.hideDialog">Close</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-main>
    <v-footer app>
      <span>&copy; {{ currentYear }} 💪 Keep Going Fitness Tracker</span>
    </v-footer>
  </v-app>
</template>
