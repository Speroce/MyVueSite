<template>
  <div class="bar"/>
  <SmokeMeter/>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import SmokeMeter from './components/SmokeMeter.vue'

export default defineComponent({
  name: 'App',
  components: {
    SmokeMeter
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  right: 0;
}
.bar {
  width: 100%;
  height: 10vh;
  background: slateblue;
}
</style>
