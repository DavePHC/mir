<script setup>

const vFocus = {
  mounted: (el) => el.focus()
}

defineProps({
  task: Object,
});

</script>

<template>
  <div class="task">
    {{task.text}}
  </div>
</template>

<style scoped>

</style>