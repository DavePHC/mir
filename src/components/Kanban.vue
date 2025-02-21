<script setup>

  import KanbanColumn from "@/components/Kanban-column.vue";
  import {ref} from 'vue'

  const statuses = ref(['На согласовании', 'Новые', 'В процессе', 'Готово', 'Доработать' ]);


</script>

<template>
  <div class="kanban">
    <KanbanColumn :statuses="statuses" />
  </div>
</template>

<style scoped lang="scss">

.kanban {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 8px;
}

</style>