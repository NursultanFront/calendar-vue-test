<script setup lang="ts">
import { ref, watch } from "vue";
import TheCalendar from "@/components/calendar/TheCalendar.vue";
import TheRangeCalendar from "./components/calendar/TheRangeCalendar.vue";
import "@vuepic/vue-datepicker/dist/main.css";

const dateIn = ref<Date | null>(null);
const dateOut = ref<Date[]>([]);
const date = ref<Date[]>([]);

watch(dateIn, (newVal) => {
  if (newVal) {
    dateOut.value = [newVal];
  }
});
</script>

<template>
  <div class="date-picker">
    <p>{{ dateIn }}</p>
    <p>{{ dateOut.toString() }}</p>
    <div class="dis">
      <TheCalendar v-model="dateIn" :inline="false" :multi="true" />
      <TheRangeCalendar
        v-model="dateOut"
        :inline="false"
        :multi="true"
        :range="true"
        :fixed-start="true"
      />
      <TheRangeCalendar
        v-model="date"
        :inline="false"
        :multi="true"
        :range="true"
      />
    </div>
  </div>
</template>

<style scoped></style>
