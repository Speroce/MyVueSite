<template>
  <div class="pos">
    <div class="buttons-bar">
      <button :disabled="isDisabled" @click="nowSnort" class="button">Фыркнул сейчас</button>
      <button :disabled="isDisabled" @click="todaySnort" class="button">Фыркнул сегодня</button>
    </div>
    <div class="list">
      <ul>
        <li v-for="time of times" :key="time">{{outputDateString(time)}}</li>
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
/* https://colorscheme.ru/#4731TkCGzw0w0 */
ul {
  background: #a99fe6;
  margin: auto;
}
li:not(li:last-child) {
  margin-bottom: 1rem;
}
.pos {
  margin: 5px;
}
.button {
  margin-right: 10px;
  background: #ffc15b;
  color: #2C1D85;
  border: 0px;
  border-radius: 1rem 0rem;
  overflow: hidden;
  transition: all 0.75s;
  box-shadow: 0 0 1rem #2C1D85;
}
.button:hover {
  border-radius: 0rem 1rem;
  background: #2C1D85;
  color: #ffc15b;
}
.button:disabled {
  background: #A99FE6;
  color: #FFFBA5;
  border-radius: 1rem 0rem;
  box-shadow: 0 0;

}
.buttons-bar {
  padding-top: 0.2rem;
  width: 100%;
  padding-bottom: 0.5rem;
}
.list {
  height: 250px; 
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: #2C1D85 #9385e6;
}
</style>
