<template>
<section class="search">
  <input type="text" class="query heading" v-model="query">
  <button @click="search()" class="iconWrap">
    <span class="icon">
      <img draggable="false" src="src/assets/search/search.svg" alt="search" title="Search">
    </span>
  </button>

</section>
</template>

<script lang="ts">
import {defineComponent} from "vue";

export default defineComponent( {
  name: "Search",
  data() {
    return {
      query: "",
    }
  },
  methods: {
    search(): void {
      if (this.query.length === 0) return;
      let link = this.searchLink;
      link = link.replace("{query}", this.query);
      const newTab = window.open(link, "_blank");
      if (newTab) newTab.focus();
      this.query = "";
    }
  },
  props: {
    searchLink: {
      type: String,
      default: "https://www.google.pl/search?q={query}"
    }
  }
})
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  border: 2px solid #fff;
  border-radius: 0.3rem;
  max-width: 100%;
  outline: 0 solid;
  transition: outline-width 0.1s linear;
  &:focus-within {
    outline: 3px solid #fff;
  }
  .query {
    padding-inline: 0.5rem;
    height: 100%;
    width: 100%;
    background-color: transparent;
    border: none;
    color: #fff;
    padding-block: 0.25rem;
    &:focus {
      outline: none;
    }
    &::placeholder {
      color: rgba(255,255,255, 0.15);
    }
  }
  .iconWrap {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    background-color: transparent;
    border: none;
    &:focus {
      outline: none;
      filter: drop-shadow(0 0 5px #fff);
    }
    .icon {
      width: 100%;
      max-width: 4rem;
      max-height: 4rem;
      cursor: pointer;
      img {
        width: 100%;
        height: 100%;
        filter: invert(100%);
      }
    }
  }

}
</style>