<template>
  <v-footer
    :elevation="4"
    style="
      margin-top: 2%;
      background: rgba(255, 255, 255, 0.5);
      backdrop-filter: blur(10px);
    "
  >
    <v-row justify="center" no-gutters>
      <v-btn v-for="link in links" color="grey" variant="text" class="mx-9">
        {{ link }}
      </v-btn>
      <v-col class="text-center mt-4" cols="12">
        {{ new Date().getFullYear() }} —
        <strong>Grafische Bedienoberflächen</strong>
      </v-col>
    </v-row>
  </v-footer>
</template>

<script>
export default {
  name: "footer",
  data: () => ({
    links: ["Paula Streit", "Annelie Pflüger", "Emily Rapp"],
  }),
};
</script>
