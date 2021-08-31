<template>
  <div class="pos">
    <button @click="nowSnort" :style="{marginRight: 10 + 'px'}">Фыркнул сейчас</button>
    <button @click="todaySnort" :style="{marginRight: 10 + 'px'}">Фыркнул сегодня</button>
    <br/>
    <div :style="{height: '250px', overflowY: 'auto'}">
      <ul :style="{background: '#eee',}">
        <li v-for="time of times" :key="time" :style="{marginBottom: '1rem'}">{{outputDateString(time)}}</li>
      </ul>
    </div>
    <div>
      Фырков за сегодня: <strong>{{snortsForTodatCount}}</strong>
      <br/>
      В среднем фырков в день: <strong>{{average}}</strong>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'SmokeMeter',
  data() {
    return {
      times: new Array<string>(),
      today: '',
      snortsForTodatCount: 0,
      average: 0
    }
  },
  async created() {
    //this.times = ['Wed Jul 21 2001', 'Wed Jul 21 2021 15:14:20 GMT+0700 (Новосибирск, стандартное время)']
    this.today = (new Date).toDateString();
    fetch("/api/")
      .then(responce => responce.json())
      .then(responce => this.times = responce);
    this.snortsForToday();
  },

  methods: {
    nowSnort() {
      const now = new Date;
      this.times.unshift(now.toString());
      fetch("/api/", {method: "POST", body: JSON.stringify(this.times)});
      this.snortsForToday();

    },
    todaySnort() {
      const _today = (new Date).toDateString();
      this.times.unshift(_today);
      fetch("/api/", {method: "POST", body: JSON.stringify(this.times)});
      this.snortsForToday();

    },
    outputDateString(str: string) {
      const date = new Date(str);
      if (date.toDateString() == str)
        return date.toLocaleDateString();
      return date.toLocaleString();
    },
    snortsForToday() {
      let i = 0;
      for (let time of this.times) {
        if ((new Date(time)).toDateString() == this.today)
          i++;

      }
      this.snortsForTodatCount = i;
      this.averageSnorts();
    },
    averageSnorts() {
      let k = new Array<string>();
      for (let time of this.times) {
        const day = (new Date(time)).toDateString();
        if (!k.includes(day)) {
          k.push(day);
        }
      }
      this.average = this.times.length / k.length;
    }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos {
  width: 100%;
  height: 90vh;
  position: relative;
}
</style>
