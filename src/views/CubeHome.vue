

<template>
  <div class="home" v-on:mousemove="mouseMove">
    <CubeHomeThreeCanvas/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import {EVENT_EMITTER} from '../libs/constants' 

import CubeHomeThreeCanvas from "@/components/CubeHomeThreeCanvas.vue"; // @ is an alias to /src

export default defineComponent({
  name: "CubeHome",
  
  components: {
    CubeHomeThreeCanvas,
  },

  methods: {
    mouseMove(event: any) {
      EVENT_EMITTER.emit("mouseMovedTo", event)
    }
  },

  mounted() {
    // EVENT_EMITTER.on()
  },
});
</script>

<style scoped lang="scss">
    .home {
      position: absolute;
      top: 0;
      bottom: 0;
      right: 0;
      left: 0;
    }
</style>
