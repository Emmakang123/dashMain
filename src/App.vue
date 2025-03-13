<script setup>
import Header from './components/Header.vue';
import MainDash from './views/MainDash.vue';
import TopEvent from './views/TopEvent.vue';
import EventBox from './views/EventBox.vue';

</script>

<template>
  <Header> </Header>
  <div class="container">
    <MainDash></MainDash>
    <TopEvent></TopEvent>
    <!-- <EventBox></EventBox> -->
</div>
</template>

<style scoped>
</style>
