<template>
  <v-app>
    <h2>Invoice upload application made with vue and express</h2>
    <inputform></inputform>
  </v-app>
</template>

<script>
import inputform from './components/inputform.vue';

export default {
  name: "App",

  components: {
    inputform,
  }
};
</script>
<style >
h2{
  text-align: center;
  color: gray;
}

</style>
