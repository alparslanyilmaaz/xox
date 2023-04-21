<!-- eslint-disable max-len -->
<template>
  <div class="fixed top-0 left-0 flex items-center justify-center w-full h-screen bg-black/50">
    <div class="px-5 py-10 bg-white rounded-md">
      <p class="text-2xl font-bold" v-if="!tied">
        {{ winner }} is the winner.
      </p>
      <p class="text-2xl font-bold" v-if="tied">
        Game is tied
      </p>
      <div class="flex gap-4 mt-10">
        <router-link to="/" class="px-10 py-4 text-xl text-white transition-all duration-300 bg-blue-500 rounded-md hover:bg-green-900">
          Go to homepage
        </router-link>
        <button @click="callback" class="px-10 py-4 text-xl text-white transition-all duration-300 bg-green-500 rounded-md hover:bg-green-900">
          Rematch
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { PropType, defineComponent } from 'vue';

export default defineComponent({
  props: {
    tied: Boolean,
    winner: String,
    callback: {
      type: Function as PropType<(payload: MouseEvent) => void>,
      required: true,

    },
  },
});
</script>
