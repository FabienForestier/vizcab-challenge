<template>
  <v-card color="primary" dark>
    <v-card-title>{{ zone.Nom }}</v-card-title>
    <v-card-text>
      <span>Nombre de logements : {{ zone.Nb_logement }}</span><br />
      <span>Ratio habitable : {{ zone.Ratio_habitable }} </span><br />
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
      return this.zoneUsableArea * this.zone.Ratio_habitable;
    },
    averageAreaPerHousing: function() {
      // The average area per housing is computed this way
      // AverageArePerHousing = LivingArea / zoneNumberOfHousing
      return this.zoneLivingArea / this.zone.Nb_logement;
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
        inhabitants = this.zone.Nb_logement *this.maxAdultsPerHousing;
      } else {
        inhabitants = this.zone.Nb_logement * (1.75 + 0.3 * (this.maxAdultsPerHousing - 1.75));
      }

      return inhabitants;
    }
  }
};
</script>