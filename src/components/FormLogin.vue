<script setup lang="ts">
import { ref } from "vue";
import type { Ref } from "vue";
import { useUserStore } from "../store/user";
import { useAppStore } from "../store/app";

const userStore = useUserStore();
const appStore = useAppStore();

const email: Ref<string> = ref("");

const login = () => {
  if (email.value === "") {
    appStore.showDialog({
      title: "Email is required",
      contents:
        "A one time login password link willl be sent to your email address.",
    });
  } else {
    userStore.login(email.value);
    appStore.showDialog({
      title: "One Time Password Login",
      contents: `A one time password login has been sent to the following email address: <strong>${email.value}</strong>. Using the link in the email, proceed to the app. Feel free to close this window 😊. If the email is incorrect, please try again.`,
      fullscreen: true,
    });
  }
};
</script>

<template>
  <v-container>
    <v-card>
      <v-card-title>Login</v-card-title>
      <v-card-text>
        <v-form @submit.prevent="login">
          <v-text-field
            v-model="email"
            label="Email"
            type="email"
            autofocus
          ></v-text-field>
          <v-btn type="submit" color="primary">Login</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </v-container>
</template>
