
<template>
  <div>
    <Checklist />
  </div>
</template>

<script lang="ts">
import './../tailwind.css'

import { defineComponent } from 'vue';
import Checklist from './components/Checklist/Checklist.vue';

export default defineComponent({
  components: {
    Checklist,
  },
});
</script>
