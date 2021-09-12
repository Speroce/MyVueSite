<template>
  <div class="main-style">
    <div class="cell">
      <div class="image">
        <img src="../assets/smoke.svg"/>
      </div>
    </div>
    <div class="cell">
      <div class="image">
        <img src="../assets/smoke.svg"/>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from 'vue'


export default defineComponent({
  name: "MainTab"
})
</script>

<style scoped>
.main-style {
  background: #62599A;
}
.cell {
  margin: 0.5rem;
  background: inherit;
  border-radius: 40% 0;
  overflow: hidden;
  filter: hue-rotate(150deg) invert(100%);
  transition: all 0.5s;
}
.cell:hover {
  filter: hue-rotate(0deg);
  border-radius: 0% 0%;
}
img {
  width: 100%;
  height: 100%;
}
.image {
  margin: 5%;
}
</style>