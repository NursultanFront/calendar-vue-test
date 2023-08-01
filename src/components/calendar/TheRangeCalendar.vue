<template>
  <VueDatePicker
    v-model="date"
    locale="ru"
    auto-apply
    :multi-calendars="props.multi"
    :inline="props.inline"
    :fixed-start="props.fixedStart"
    :range="props.range"
    :format="formatDate"
    :clearable="false"
    :enable-time-picker="false"
    :disabled-dates="disabledDates"
  />
</template>

<script setup lang="ts">
import { ref, computed, watchEffect, watch } from "vue";
import VueDatePicker from "@vuepic/vue-datepicker";

interface IProps {
  modelValue: Date[];
  inline: boolean;
  multi: boolean;
  range: boolean;
  fixedStart?: boolean;
}

const props = withDefaults(defineProps<IProps>(), {
  fixedStart: false,
});

const emit = defineEmits<{
  (event: "update:modelValue", value: Date[]): void;
}>();

const date = ref<Date[]>([]);

const disabledDates = (date: Date) => {
  const today = new Date();
  today.setHours(0, 0, 0, 0);
  return date < today;
};

const formateDateAll = (date: Date[]) => {
  const lastDate = date[date.length - 1];
  if (lastDate instanceof Date) {
    const options: Intl.DateTimeFormatOptions = {
      day: "numeric",
      month: "short",
      weekday: "short",
    };
    return lastDate.toLocaleDateString("ru-RU", options);
  }
  return "";
};

const formattLastDate = (date: Date[]) => {
  return date
    .map((date) => {
      if (date instanceof Date) {
        const options: Intl.DateTimeFormatOptions = {
          day: "numeric",
          month: "short",
          weekday: "short",
        };
        return date.toLocaleDateString("ru-RU", options);
      }
      return "";
    })
    .join(" - ");
};

const formatDate = (date: Date[]) => {
  return props.fixedStart ? formateDateAll(date) : formattLastDate(date);
};

watch(
  () => props.modelValue,
  (newValue) => {
    if (props.fixedStart && newValue.length > 0) {
      date.value = [...newValue];
    }
  }
);

watch(date, (newValue) => {
  if (newValue.length > 1 && newValue !== props.modelValue) {
    emit("update:modelValue", newValue);
  }
});
</script>

<style scoped></style>
