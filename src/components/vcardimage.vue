<template>
  <v-card class="mx-auto" max-width="344">
    <v-img
      :src="image"
      height="200px"
    ></v-img>

    <v-card-title> Total Amount: ${{amount}} </v-card-title>

    <v-card-actions>
      <v-btn color="orange lighten-2" text> Comments </v-btn>

      <v-spacer></v-spacer>

      <v-btn icon @click="show = !show">
        <v-icon>{{ show ? "mdi-chevron-up" : "mdi-chevron-down" }}</v-icon>
      </v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text> {{comment}} </v-card-text>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<script>
export default {
  name: "vcardimage",
  props: ['comment','image', 'amount'],
  data() {
    return {
      show: false,
    };
  },
};
</script>

<style>
</style>