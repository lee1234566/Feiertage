<template>
  <v-timeline>
    <v-timeline-item
      v-for="(item, index) in timelineItems"
      :key="index"
      dot-color="#263238"
      size="extra-small"
    >
      <v-card style="padding: 2%">
        <v-card-title>{{ item.title }}</v-card-title>
        <v-card-subtitle style="font-size: large">{{
          item.start.toLocaleDateString()
        }}</v-card-subtitle>
      </v-card>
    </v-timeline-item>
  </v-timeline>
</template>

<script>
export default {
  props: {
    timelineItems: {
      type: Array,
      required: true,
    },
  },
};
</script>
