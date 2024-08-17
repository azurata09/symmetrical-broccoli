<script setup lang="ts">
const props = defineProps<{
  idol: string
}>()
</script>

<template>
  <div class="idolbox">
    {{ props.idol }}
  </div>
</template>

<style scoped>
.idolbox {
  border: 1px solid black;
  padding: 10px;
  /* margin: 10px; */
}
</style>
