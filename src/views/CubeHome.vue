

<template>
  <div class="home" id="container" v-on:mousemove="mouseMove">
    <div class="canvasContainer">
      <CubeHomeThreeCanvas/>
    </div>
    <div class="navButton" id="aboutNavButton">About</div>
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
      let container = document.getElementById('threeCubeContainer');
      const absX = event.clientX;
      const absY = event.clientY;
      const relX = absX / container.clientWidth; 
      const relY = absY / container.clientHeight; 
      EVENT_EMITTER.emit("mouseMovedTo", {relX: relX, relY: relY})
    }
  },

  mounted() {
    // let dynamicTextureScript = document.createElement('script');
    // dynamicTextureScript.setAttribute('src', 'threex.dynamictexture.js')
    // document.head.appendChild(dynamicTextureScript)
    // EVENT_EMITTER.on()
  },
});
</script>

<style scoped lang="scss">
  .canvasContainer {
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    z-index: -1;
  }

  .navButton {
    z-index: 0;
    background: white;
    width: 300px;
    height: 50px;
    text-align: center;
    font-size: 40px;
    // position: absolute;
    // top: 0;
    // bottom: 0;
    // right: 0;
    // left: 0;
  }

  #aboutNavButton {
    top: 0;
  }
</style>
