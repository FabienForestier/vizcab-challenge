<template>
    <v-app>
        <v-app-bar app color="secondary" dark>
            <div class="d-flex align-center">
                <v-img
                    alt="Vizcab Logo"
                    class="shrink mr-2"
                    contain
                    :src="require('@/assets/logo.svg')"
                    transition="scale-transition"
                    width="40"
                />
            </div>

            <v-spacer />

            <v-btn href="https://vizcab.io" target="_blank" text>
                <span class="mr-2">vizcab.io</span>
                <v-icon>mdi-open-in-new</v-icon>
            </v-btn>
        </v-app-bar>

        <v-main>
            <layout-occupation-zone v-for="building in buildings" :key="building.Index" :building="building"/>
        </v-main>
    </v-app>
</template>

<script>
import LayoutOccupationZone from "./components/LayoutOccupationZone";
import * as data from '@/data/rset.json';

export default {
  name: "App",
  components: {
    LayoutOccupationZone,
  },
  data: function() {
    return {
      buildings: data.Batiments,
    };
  },
};
</script>
