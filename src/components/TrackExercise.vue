<script setup lang="ts">
import { ref, computed } from "vue";
import type { Ref } from "vue";
import { VDatePicker } from "vuetify/components";
import type { Routine } from "../types/fitness";
import AddRoutine from "./AddRoutine.vue";

const routines: Ref<Routine[]> = ref([]);
const showDialogRoutine: Ref<boolean> = ref(false);
const addRoutineToExercise = (newRoutine: any) => {
  showDialogRoutine.value = false;
  routines.value.push(newRoutine);
};

const showDialogDate: Ref<boolean> = ref(false);
const selectedDate: Ref<any[] | undefined> = ref(undefined);

const formattedDate: Ref<string> = computed(() => {
  if (selectedDate?.value?.length) {
    return new Intl.DateTimeFormat("en-us", {
      weekday: "long",
      year: "numeric",
      month: "long",
      day: "numeric",
    }).format(selectedDate.value[0]);
  }
  return "";
});
</script>

<template>
  <v-container>
    <v-row class="mb-6 align-center justify-space-between">
      <v-btn @click="showDialogDate = true">
        <span v-if="selectedDate">Change date</span>
        <span v-else>select date</span>
      </v-btn>
      {{ formattedDate }}
      <v-dialog v-model="showDialogDate" center>
        <v-date-picker
          v-model="selectedDate"
          show-adjacent-months
          @click:cancel="showDialogDate = false"
          @click:save="showDialogDate = false"
          style="margin: 0 auto"
        ></v-date-picker>
      </v-dialog>
    </v-row>
    {{ routines }}
    <v-row class="mb-6">
      <v-btn
        block
        size="x-large"
        @click="showDialogRoutine = true"
        v-if="selectedDate"
        >Add routine</v-btn
      >
      <v-dialog v-model="showDialogRoutine" fullscreen>
        <v-card
          ><v-card-text
            ><add-routine @add="addRoutineToExercise"
          /></v-card-text>
          <v-card-actions
            ><v-btn color="primary" @click="showDialogRoutine = false"
              >Close</v-btn
            ></v-card-actions
          ></v-card
        >
      </v-dialog>
    </v-row>
  </v-container>
</template>
