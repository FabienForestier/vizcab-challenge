<template>
  <v-card color="primary" dark>
    <v-card-title>{{ zone.Nom }}</v-card-title>
    <v-card-text>
      <span>Nombre de logements : {{ zone.Nb_logement }}</span><br />
      <span>Ratio habitable : {{ zone.Ratio_habitable }} </span><br />
      <span>Surface utile (m²) : {{ computeZoneUsableArea }}</span
      ><br />
      <span>Surface habitable (m²) : {{ computeZoneLivingArea }}</span
      ><br />
      <span
        >Surface moyenne par logement (m²) :
        {{ computeAverageAreaPerHousing }}</span
      ><br />
      <span class="ml-2">Groupe 1 :</span><br />
      <span class="ml-2">Groupe 2 :</span><br />
      <span class="font-weight-medium">Nombre d'occupants :</span><br />
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
    computeZoneUsableArea: function() {
      // The usable Area of a zone correspond to the sum of its groups' surfaces
      return this.zone.Groupes.reduce((sum, groupArea) => sum + groupArea.SURT, 0);
    },
    computeZoneLivingArea: function() {
      // The living area of a zone is computed like this
      // LivingArea = UsableArea * LivingRatio
      return this.computeZoneUsableArea * this.zone.Ratio_habitable;
    },
    computeAverageAreaPerHousing: function() {
      // The average area per housing is computed this way
      // AverageArePerHousing = LivingArea / zoneNumberOfHousing
      return this.computeZoneLivingArea / this.zone.Nb_logement;
    }
  }
};
</script>