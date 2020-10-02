<template>
  <v-card color="primary" dark>
    <v-card-title>{{ zone.Nom }}</v-card-title>
    <v-card-text>
      <v-row>
        <v-col class="pr-4">
          <span>Nombre de logements :</span><br />
          <v-slider
            v-model="housingNumber"
            class="align-center"
            max="50"
            min="1"
            hide-details
          >
            <template v-slot:append>
              <v-text-field
                v-model="housingNumber"
                class="mt-0 pt-0"
                hide-details
                single-line
                type="number"
                style="width: 60px"
              ></v-text-field>
            </template>
          </v-slider>
        </v-col>
      </v-row>

      <v-row>
        <v-col class="pr-4">
          <span>Ratio habitable :</span><br />
          <v-slider
            v-model="ratio"
            class="align-center"
            max="1"
            min="0"
            step="0.1"
            hide-details
          >
            <template v-slot:append>
              <v-text-field
                v-model="ratio"
                class="mt-0 pt-0"
                hide-details
                single-line
                type="number"
                style="width: 60px"
              ></v-text-field>
            </template>
          </v-slider>
        </v-col>
      </v-row>
      <span>Surface utile (m²) : {{ zoneUsableArea }}</span
      ><br />
      <span>Surface habitable (m²) : {{ zoneLivingArea }}</span
      ><br />
      <span
        >Surface moyenne par logement (m²) :
        {{ averageAreaPerHousing }}</span
      ><br />
      <span class="ml-2" v-for="group in zone.Groupes" :key="group.Index">{{ group.Nom}}: {{ group.SURT }} m²<br /></span>
      <span class="font-weight-medium">Nombre d'occupants : {{ inhabitantsNumber }}</span><br />
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: "CardOccupationZone",
  data: function() {
    return {
      // Those variables can be modified by the sliders
      housingNumber: 1,
      ratio: 0.7,
    };
  },
  props: {
    zone: Object,
  },
  computed: {
    zoneUsableArea: function() {
      // The usable Area of a zone correspond to the sum of its groups' surfaces
      return this.zone.Groupes.reduce((sum, groupArea) => sum + groupArea.SURT, 0);
    },
    zoneLivingArea: function() {
      // The living area of a zone is computed like this
      // LivingArea = UsableArea * LivingRatio
      return this.zoneUsableArea * this.ratio;
    },
    averageAreaPerHousing: function() {
      // The average area per housing is computed this way
      // AverageArePerHousing = LivingArea / zoneNumberOfHousing
      return this.zoneLivingArea / this.housingNumber;
    },
    maxAdultsPerHousing: function() {
      let maxAdultNb = 0;

      if (this.averageAreaPerHousing < 10) {
        maxAdultNb = 1;
      } else if (10 <= this.averageAreaPerHousing && this.averageAreaPerHousing < 50) {
        maxAdultNb = 1.75 - 0.01875 * (50 - this.averageAreaPerHousing);
      } else if (50 <= this.averageAreaPerHousing) {
        maxAdultNb = 0.035 * this.averageAreaPerHousing;
      }

      return maxAdultNb;
    },
    inhabitantsNumber: function() {
      let inhabitants;

      if (this.maxAdultsPerHousing < 1.75) {
        inhabitants = this.housingNumber *this.maxAdultsPerHousing;
      } else {
        inhabitants = this.housingNumber * (1.75 + 0.3 * (this.maxAdultsPerHousing - 1.75));
      }

      return inhabitants;
    }
  },
  created() {
    // Initializing housingNumber and ration with the known value
    // for the zone
    this.housingNumber = this.zone.Nb_logement;
    this.ratio = this.zone.Ratio_habitable;
  }
};
</script>