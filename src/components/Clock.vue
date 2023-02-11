<template>
  <section class="clock">
    <div class="time heading">
      {{ timeFormat.format(this.time) }}
    </div>
    <div class="date">
      {{ dateFormat.format(this.time) }}
    </div>
  </section>
</template>

<script lang="ts">

import {defineComponent} from "vue";

export default defineComponent({
  name: "Clock",
  data() {
    return {
      lang: navigator.language,
      dateFormat: new Intl.DateTimeFormat(navigator.language, {
        year: "numeric",
        month: "long",
        day: "numeric",
        weekday: "long",
      }),
      timeFormat: new Intl.DateTimeFormat(navigator.language, {
        hourCycle: "h23",
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
      }),
      time: new Date(),
    }
  },
  mounted() {
    setInterval(() => {
      this.time = new Date();
    }, 100);
  },
})
</script>

<style lang="scss" scoped>
.clock {
  text-align: center;
}
@media screen and (min-width:800px) {
  .clock {
    text-align: start;
    min-width: 20rem;
  }
}
</style>