<template>
  <h1 style="margin-top: 8%">Nächste Feiertage</h1>
  <v-divider class="my-4"></v-divider>
  <div class="card-container">
    <v-card
      width="100%"
      v-for="(item, index) in sortedTimelineItems.slice(0, 2)"
      :key="index"
      style="
        background: rgba(255, 255, 255, 0.6);
        backdrop-filter: blur(10px);
        padding: 2%;
        border-radius: 2%;
      "
    >
      <v-img
        class="align-end text-white"
        height="300"
        v-if="item.title === 'Karfreitag'"
        src="./img/Jesus.jpg"
        cover
        style="border-radius: 2%"
      ></v-img>
      <v-img
        class="align-end text-white"
        height="300"
        v-if="item.title === 'Ostermontag'"
        src="./img/Ostern.jpg"
        cover
        style="border-radius: 2%"
      ></v-img>
      <v-img
        class="align-end text-white"
        height="300"
        v-if="item.title === 'Frauentag'"
        src="./img/Frauen.jpg"
        cover
        style="border-radius: 2%"
      ></v-img>
      <v-img
        class="align-end text-white"
        height="300"
        v-if="item.title === 'Gründonnerstag'"
        src="./img/Essen.jpg"
        cover
        style="border-radius: 2%"
      ></v-img>
      <v-img
        class="align-end text-white"
        height="300"
        v-if="item.title === 'Neujahrstag'"
        src="./img/Neujahr.jpg"
        cover
        style="border-radius: 2%"
      ></v-img>
      <v-img
        class="align-end text-white"
        height="300"
        v-if="item.title === 'Heilige Drei Könige'"
        src="./img/dreiKonig.jpeg"
        cover
        style="border-radius: 2%"
      ></v-img>
      <v-card-title style="font-size: 1.5rem">{{ item.title }}</v-card-title>
      <v-card-text class="pt-6" style="font-size: large">{{
        item.start.toLocaleDateString()
      }}</v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  props: {
    sortedTimelineItems: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style>
.card-container {
  display: grid;
  grid-template-columns: 45% 45%;
  margin-top: 8%;
  margin-bottom: 8%;
  justify-items: center;
  gap: 10%;
}
</style>
