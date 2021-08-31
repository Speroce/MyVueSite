<template>
  <div class="pos">
    <button :disabled="isDisabled" @click="nowSnort" class="button">Фыркнул сейчас</button>
    <button :disabled="isDisabled" @click="todaySnort" class="button">Фыркнул сегодня</button>
    <br/>
    <div :style="{height: '250px', overflowY: 'auto'}">
      <ul :style="{background: '#eee',}">
        <li v-for="time of times" :key="time" style="marginBottom: 1rem">{{outputDateString(time)}}</li>
      </ul>
    </div>
    <div>
      Фырков за сегодня: <strong>{{snortsForTodayCount}}</strong>
      <br/>
      В среднем фырков в день: <strong>{{averageSnortsCount}}</strong>
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
      snortsForTodayCount: 0,
      averageSnortsCount: 0,
      isDisabled: true
    }
  },
  created() {
    console.log("SmokeMeter created!");
    this.today = (new Date).toDateString();
    (async () => {
      this.times = await fetch("/querry.json/",  {method: "GET"}).then(responce => responce.json()).finally() as string[];
      this.isDisabled = false;
      this.snortsForToday();
    })();
  },

  methods: {
    nowSnort() {
      const now = new Date;
      this.times.unshift(now.toString());
      fetch("http://speroce.pythonanywhere.com/api/", {method: "POST", body: JSON.stringify(this.times)});
      this.snortsForToday();

    },
    todaySnort() {
      const _today = (new Date).toDateString();
      this.times.unshift(_today);
      fetch("http://speroce.pythonanywhere.com/api/", {method: "POST", body: JSON.stringify(this.times)});
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
      this.snortsForTodayCount = i;
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
      this.averageSnortsCount = this.times.length / k.length;
    }
  },
})
</script>

<style scoped>
.pos {
  margin: 5px;
}
.button {
  margin-right: 10px;
}
</style>
