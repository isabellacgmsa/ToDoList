<template>
  <div>
    <h1>Minha To-Do List</h1>
    <Checklist />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Checklist from './components/Checklist.vue';

export default defineComponent({
  components: {
    Checklist,
  },
});
</script>
