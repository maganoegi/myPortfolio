

<template>
  <div class="home">
    <CubeHomeThreeCanvas/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import CubeHomeThreeCanvas from "@/components/CubeHomeThreeCanvas.vue"; // @ is an alias to /src

export default defineComponent({
  name: "CubeHome",
  components: {
    CubeHomeThreeCanvas,
  },
});
</script>
