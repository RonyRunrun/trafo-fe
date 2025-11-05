<script setup lang="ts">
import { ref } from "vue";

const count = ref(0);
</script>

<template>
  <div class="mt-4">
    <Button type="button" @click="count++">count is {{ count }}</Button>
  </div>
</template>
