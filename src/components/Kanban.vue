<script setup>

  import KanbanColumn from '@/components/Kanban-column.vue';
  import { ref } from 'vue';

  const board = ref([
    {
      status: 'На согласовании',
      tasks: [
        {
          id: 0,
          text: 'Текст задачи',
        },
        {
          id: 1,
          text: 'Текст задачи 1',
        },
      ],
    },
    {
      status: 'Новые',
      tasks: []
    },
    {
      status: 'В процессе',
      tasks: [],
    },
    {
      status: 'Готово',
      tasks: [],
    },
    {
      status: 'Доработать',
      tasks: [],
    },
  ]);
</script>

<template>
  <div class="kanban">
    <KanbanColumn :board="board"/>
  </div>
</template>

<style scoped lang="scss">

.kanban {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 8px;
}

</style>