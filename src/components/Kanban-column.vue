<script setup>

import {computed} from "vue";

defineProps({
  statuses: Array,
});

document.addEventListener("DOMContentLoaded", () => {
  const root = document.documentElement;

  for (let i = 1; i <= 5; i++) {
    const randomColor = `hsl(${Math.random() * 360}, 70%, 60%)`;
    root.style.setProperty(`--random-color-${i}`, randomColor);
  }
});

</script>

<template>
 <div v-for="(status, index) in statuses" class="kanban-column">
   <div class="kanban-column__head">
     {{status}}
   </div>
   <div class="kanban-column__body">
     <button class="kanban-column__button">+ Добавить</button>
   </div>
 </div>
</template>

<style lang="scss" scoped>
  .kanban-column {
    border: 1px solid var(--color-column-border);
    border-radius: 8px;
    overflow-y: auto;
    &__head {
      background-color: var(--random-color, #3498db);
      color: var(--color-text);
      padding: 7px;
      font-weight: 700;
      text-align: center;
    }
    &__body {
      padding: 8px;
      background-color: var(--color-column-bg);
    }
    &__button {
      border: none;
      background-color: transparent;
      padding: 0;
      cursor: pointer;
      color: var(--color-blue);
    }
  }
</style>