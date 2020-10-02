<template>
    <v-container>
        <v-row>
            <v-col class="my-4" cols="12">
                <h1 class="text-center display-2 font-weight-bold mb-3">{{ building.Nom }}</h1>
                <v-img
                    :src="require('@/assets/schema_batiment.jpg')"
                    class="my-3"
                    contain
                    height="200"
                />
            </v-col>

            <v-col class="mb-5">
                <v-row align="center" justify="center">
                    <card-occupation-zone v-for="zone in building.Zones" :key="zone.Index" :zone="zone"/>
                </v-row>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
import CardOccupationZone from "./CardOccupationZone";

export default {
  name: "LayoutOccupationZone",
  components: {
    CardOccupationZone,
  },
  props: {
    building: Object,
  },
};
</script>
