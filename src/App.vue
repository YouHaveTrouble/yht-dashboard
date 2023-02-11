<template>
  <main>
    <Clock/>
    <Weather/>
    <Search/>
  </main>
</template>

<script>
import {defineComponent} from "vue";
import Clock from "@/components/Clock.vue";
import Weather from "@/components/Weather.vue";
import Search from "@/components/Search.vue";

export default defineComponent({
  components: {
    Search,
    Weather,
    Clock,
  }
})
</script>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  flex-wrap: wrap;
  padding: 1rem;
  gap: 2.5rem;
}

@media screen and (min-width:800px) {
  main {
    flex-direction: row;
    align-items: center;
    padding: 5rem;
    gap: 5rem;
    height: 100%;
  }
}
</style>
