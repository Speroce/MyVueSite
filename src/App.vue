<template>
  <div class="bar">
    <img src="./assets/speroceLogo.svg" class="first-logo"/>
    <img src="./assets/speroceLogo.svg" class="second-logo" />
  </div>
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
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  font-family: 'Courier New', Courier, monospace;
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  right: 0;
  background: #9385e6;
}
.bar {
  width: 100%;
  height: 10vh;
  background: #6a5acd;
  text-align: center;
}
.first-logo {
  position: fixed;
  max-height: 10vh;
  filter: hue-rotate(90deg);
  -webkit-filter: hue-rotate(90deg);
}
.second-logo {
  position: relative;
  top: 1px;
  left: 3px;
  max-height: 10vh;
  transition: left 0.2s, top 0.2s, filter 0.6s cubic-bezier(0,2,1,1);
  -webkit-transition: left 0.2s, top 0.2s, filter 0.6s cubic-bezier(0,2,1,1);
}
.second-logo:hover {
  left: 4px;
  top: 4px;
  filter: hue-rotate(-90deg);
  -webkit-filter: hue-rotate(-90deg);
}
</style>
