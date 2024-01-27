<template>
  <v-app
    style="background: linear-gradient(to right, #ffffff, #e3f2fd, #bbdefb)"
  >
    <Menu></Menu>
    <v-main>
      <form class="selection">
        <v-select
          label="Bundesland auswählen"
          :items="bundeslandItems.map((item) => item.title)"
          v-model="selectedItem"
          item-text="title"
          @update:modelValue="loadKalender"
        ></v-select>
        <v-select
          label="Jahr auswählen"
          :items="jahre"
          v-model="selectedJahr"
          item-text="null"
          item-value="null"
          @update:modelValue="loadKalender"
        ></v-select>
      </form>
      <v-container>
        <Karten :sortedTimelineItems="sortedTimelineItems"></Karten>
        <h1>Alle Feiertage</h1>
        <v-divider class="my-4"></v-divider>
        <v-row>
          <v-col>
            <v-card
              style="
                background: rgba(255, 255, 255, 0.5);
                backdrop-filter: blur(10px);
                padding: 2%;
              "
            >
              <v-tabs v-model="tab">
                <v-tab value="one">Timeline</v-tab>
                <v-tab value="two">Kalender</v-tab>
              </v-tabs>
              <v-card-text>
                <v-window v-model="tab">
                  <v-window-item value="one">
                    <Timeline :timelineItems="timelineItems"></Timeline>
                  </v-window-item>
                  <v-window-item value="two">
                    <Kalender :feiertagsListe="feiertagsListe"></Kalender>
                  </v-window-item>
                </v-window>
              </v-card-text>
            </v-card>
            <v-divider class="my-4"></v-divider>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <Footer></Footer>
  </v-app>
</template>

<script>
import axios from "axios";
import Karten from "./components/karten.vue";
import Footer from "./components/footer.vue";
import Menu from "./components/menu.vue";
import Timeline from "./components/timeline.vue";
import Kalender from "./components/kalender.vue";

const bundeslandItems = [
  { title: "Deutschland", value: "NATIONAL" },
  { title: "Baden-Württemberg", value: "BW" },
  { title: "Bayern", value: "BY" },
  { title: "Berlin", value: "BE" },
  { title: "Brandenburg", value: "BB" },
  { title: "Bremen", value: "HB" },
  { title: "Hamburg", value: "HH" },
  { title: "Hessen", value: "HE" },
  { title: "Mecklenburg-Vorpommern", value: "MV" },
  { title: "Niedersachsen", value: "NI" },
  { title: "Nordrhein-Westfalen", value: "NW" },
  { title: "Rheinland-Pfalz", value: "RP" },
  { title: "Saarland", value: "SL" },
  { title: "Sachsen", value: "SN" },
  { title: "Sachsen-Anhalt", value: "ST" },
  { title: "Schleswig-Holstein", value: "SH" },
  { title: "Thüringen", value: "TH" },
];
const jahre = [2024, 2025, 2026, 2027];

export default {
  computed: {
    sortedTimelineItems() {
      const today = new Date();
      return this.timelineItems
        .filter((item) => item.start >= today)
        .sort((a, b) => a.start - b.start);
    },
  },
  components: {
    Karten, 
    Footer,
    Menu,
    Timeline,
    Kalender,
  },
  data() {
    return {
      timelineItems: [],
      selectedItem: "Deutschland",
      jahre,
      feiertagsListe: [],
      bundeslandItems,
      selectedJahr: "2024",
      loading: false,
      tab: null,
    };
  },

  mounted() {
    this.loadKalender();
  },
  methods: {
    async loadKalender() {
      console.log(this.selectedItem);
      const bundeslandItem = this.bundeslandItems.find(
        (item) => item.title === this.selectedItem
      );
      const bundeslandKurz = bundeslandItem ? bundeslandItem.value : "";

      if (this.selectedItem && this.selectedJahr) {
        this.loading = true;
        let apiUrl = "https://feiertage-api.de/api/";

        try {
          let response = await axios.get(`
              ${apiUrl}?jahr=${this.selectedJahr}&nur_land=${bundeslandKurz}
            `);
          if (typeof response.data === "object" && response.data !== null) {
            this.feiertagsListe = Object.entries(response.data);
            this.timelineItems = this.transformDataForTimeline(
              this.feiertagsListe
            );
            console.log(response);
          } else {
            console.log("Ungültiges Format der API-Daten:", response.data);
          }
        } catch (e) {
          console.log("Fehler beim Laden der Daten:", e);
        } finally {
          this.loading = false;
        }
      }
    },

    transformDataForTimeline(data) {
      if (Array.isArray(data)) {
        return data
          .map((item) => {
            const title = item[0];
            const dateInfo = item[1];

            if (dateInfo && dateInfo.datum) {
              const dateString = dateInfo.datum;
              const start = new Date(dateString);
              return {
                title: title,
                start: start,
              };
            } else {
              console.error("Ungültiges Format der Daten:", item);
              return null; 
            }
          })
          .filter((item) => item !== null);
      } else {
        console.error("Ungültiges Format der Daten:", data);
        return [];
      }
    },
  },
};
</script>

<style>
* {
  font-family: "neue-haas-grotesk-display", sans-serif;
  font-weight: 700;
  font-style: normal;
  color: #263238;
}

.selection {
  display: grid;
  grid-template-columns: 50% 50%;
  position: fixed;
  width: 100%;
  z-index: 1000;
}
</style>
