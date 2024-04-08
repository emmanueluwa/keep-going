<script setup lang="ts">
import { storeToRefs } from "pinia";
import { useUserStore } from "../store/user";
import { useAppStore } from "../store/app";

const appStore = useAppStore();
const userStore = useUserStore();
const { drawer } = storeToRefs(appStore);
const { userIsLoggedIn } = storeToRefs(userStore);

const goToPage = (page: string): void => {
  appStore.navigateToPage(page);
};
</script>

<template>
  <v-navigation-drawer v-model="drawer" app>
    <v-list dense v-if="userIsLoggedIn">
      <v-list-item @click="userStore.logout()">
        <template v-slot:prepend>
          <v-icon icon="mdi-account-arrow-right"></v-icon>
        </template>
        <v-list-item-title>Log out</v-list-item-title>
      </v-list-item>
    </v-list>
    <v-list dense v-else>
      <v-list-item @click="goToPage('/login')">
        <template v-slot:prepend>
          <v-icon icon="mdi-account"></v-icon>
        </template>
        <v-list-item-title>Login</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>
