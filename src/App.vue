<script setup>
import CardList from "./components/sections/CardList.vue";
import TheHeader from "./components/layout/TheHeader.vue";
</script>

<template>
  <TheHeader />
  <main class="main">
    <CardList />
  </main>
</template>

<style>
* {
  padding: 0px;
  margin: 0px;
  border: none;
}

*,
*::before,
*::after {
  box-sizing: border-box;
}
ul,
ul li {
  list-style: none;
}

html {
  font-size: 10px;
  font-family: Georgia, serif;
}
.main {
  min-height: calc(100vh - (9rem * 2 + 79px));
}


</style>
