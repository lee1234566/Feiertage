<template>
  <div id="kalenderbox">
    <v-row class="fill-height">
      <v-col>
        <v-sheet height="auto">
          <v-calendar
            ref="calendar"
            v-model="today"
            color="red"
            type="month"
            :events="formatEvents(feiertagsListe)"
            @keydown.native="handleKeydown"
          ></v-calendar>
        </v-sheet>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  props: {
    feiertagsListe: Array,
    required: true,
  },
  data() {
    return {
      today: new Date(), // Initialisiert das Datum auf den aktuellen Tag
    };
  },
  methods: {
    handleKeydown(event) {
      // Überprüfe, ob die Pfeiltasten gedrückt wurden
      if (event.key === "ArrowLeft" || event.key === "ArrowRight") {
        // Navigiere zu vorherigem oder nächstem Monat, abhängig von der gedrückten Pfeiltaste
        const newDate = new Date(this.today);
        newDate.setMonth(
          event.key === "ArrowLeft"
            ? newDate.getMonth() - 1
            : newDate.getMonth() + 1
        );
        this.today = newDate;
      }
    },
    formatEvents(events) {
      // Formatiere die Ereignisse im richtigen Format für v-calendar
      return events.map((event) => ({
        title: event[0],
        start: new Date(event[1].datum),
        end: new Date(event[1].datum),
        allDay: true,
      }));
    },
    updateCalendarToYear() {
      // Aktualisiere das Jahr des Kalenders basierend auf dem ersten Ereignis im feiertagsListe-Array
      if (this.feiertagsListe.length > 0) {
        const firstEventYear = new Date(
          this.feiertagsListe[0][1].datum
        ).getFullYear();
        const newDate = new Date(this.today);
        newDate.setFullYear(firstEventYear);
        this.today = newDate;
      }
    },
  },
  watch: {
    // Überwache Änderungen im feiertagsListe-Array und aktualisiere das Kalenderjahr
    feiertagsListe: "updateCalendarToYear",
  },
  mounted() {
    // Rufe die Methode nach dem Laden der Komponente auf
    this.updateCalendarToYear();
  },
};
</script>

<style>
#kalenderbox {
  height: 100%;
  width: auto;
  padding: 3%;
}
</style>
