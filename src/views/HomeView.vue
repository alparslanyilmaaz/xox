<template>
  <div class="flex items-center justify-center w-full h-full">
    <router-link to="/game"
    class="text-white border-[#1e293b] border px-4 py-5
    hover:bg-white hover:text-black transition-all duration-200">
      Start Game
    </router-link>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'HomeView',
  components: {
  },
});
</script>
