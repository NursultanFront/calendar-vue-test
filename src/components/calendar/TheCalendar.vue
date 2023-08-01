<template>
  <VueDatePicker
    v-model="date"
    locale="ru"
    multi-calendars
    :inline="props.inline"
    auto-apply
    :format="formatDate"
    :clearable="false"
    :enable-time-picker="false"
    :disabled-dates="disabledDates"
  />
</template>

<script setup lang="ts">
import { watch, ref } from "vue";
import VueDatePicker from "@vuepic/vue-datepicker";

interface IProps {
  modelValue: Date | null;
  inline: boolean;
  multi: boolean;
}

const props = defineProps<IProps>();

const emit = defineEmits<{
  (event: "update:modelValue", value: Date): void;
}>();

const date = ref<Date | null>(null);

const disabledDates = (date: Date) => {
  const today = new Date();
  today.setHours(0, 0, 0, 0);
  return date < today;
};

const formatDate = (date: Date) => {
  const options: Intl.DateTimeFormatOptions = {
    day: "numeric",
    month: "short",
    weekday: "short",
  };
  return date.toLocaleDateString("ru-RU", options);
};

watch(date, () => {
  if (date.value) {
    emit("update:modelValue", date.value);
  }
});
</script>

<style scoped></style>
