<template>
  <div>
    <div class="buttons-bar">
      <button :disabled="isDisabled" @click="nowSnort" class="button">
        Фыркнул сейчас
      </button>
      <button :disabled="isDisabled" @click="todaySnort" class="button">
        Фыркнул сегодня
      </button>
    </div>
    <div class="list">
      <ul>
        <li v-for="time of times" :key="time">{{ outputDateString(time) }}</li>
      </ul>
    </div>
    <div>
      Фырков за сегодня: <strong>{{ snortsForToday }}</strong>
      <br />
      В среднем фырков в день: <strong>{{ averageSnorts }}</strong>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, reactive } from "vue";

const isDisabled = ref(true);
const today = new Date().toDateString();

const times = reactive(new Array<string>());
void fetch("/querry.json/", { method: "GET" })
  .then((responce) => responce.json())
  .then((array: string[]) => {
    times.splice(0, 0, ...array);
    isDisabled.value = false;
  });

const snortsForToday = computed(() => {
  let i = 0;
  for (; i < times.length; i++) {
    if (new Date(times[i]).toDateString() !== today) {
      break;
    }
  }
  return i;
});

const averageSnorts = computed(() => {
  const subSnorx = times.slice(0, Math.min(50, times.length));
  const daysNumber = subSnorx
    .map(snorkDate => new Date(snorkDate).toDateString())
    .filter((date, index, arr) => arr.indexOf(date) === index).length;
    return subSnorx.length / daysNumber;
});

const nowSnort = () => {
  const now = new Date();
  times.unshift(now.toString());
  fetch("/api/", { method: "POST", body: JSON.stringify(times) });
};

const todaySnort = () => {
  const _today = new Date().toDateString();
  times.unshift(_today.toString());
  fetch("/api/", { method: "POST", body: JSON.stringify(times) });
};

const outputDateString = (str: string) => {
  const date = new Date(str);
  if (date.toDateString() == str) return date.toLocaleDateString();
  return date.toLocaleString();
};
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
.button {
  margin-right: 10px;
  background: #ffc15b;
  color: #2c1d85;
  border: 0px;
  border-radius: 1rem 0rem;
  overflow: hidden;
  transition: all 0.75s;
  box-shadow: 0 0 1rem #2c1d85;
}
.button:hover {
  border-radius: 0rem 1rem;
  background: #2c1d85;
  color: #ffc15b;
}
.button:disabled {
  background: #a99fe6;
  color: #fffba5;
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
  scrollbar-color: #2c1d85 #9385e6;
}
</style>
