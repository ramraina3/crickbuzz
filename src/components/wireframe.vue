<template>
  <v-app id="inspire">
    <!-- <v-navigation-drawer v-model="drawer">
      <v-sheet class="pa-4" color="grey-lighten-4">
        <v-avatar class="mb-4" color="grey-darken-1" size="64"></v-avatar>

        <div>OMS</div>
      </v-sheet>

      <v-divider></v-divider>

      <v-list>
        <v-list-item
          v-for="[icon, text] in links"
          :key="icon"
          :prepend-icon="icon"
          :title="text"
          link
          @click="listItemSelected()"
        ></v-list-item>
      </v-list>
    </v-navigation-drawer> -->

    <v-app-bar>
      <!-- <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon> -->

      <v-app-bar-title>OMS Cricket</v-app-bar-title>
    </v-app-bar>

    <v-main>
      <datatable /> <!--data table for team entry has been implemented -->
      <!-- <spin/> -->
      <!--  -->
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from "vue";
import datatable from "./datatable.vue";
import spin from "./spin.vue";

const cards = ["Today", "Yesterday"];
const links = [["mdi-shuffle-variant", "Spin"]];

const drawer = ref(null);
</script>

<script>
export default {
  components: { datatable },
  data: () => ({ drawer: null }),
  methods: {
    listItemSelected() {
      console.log("Function working fine");
    },
  },
};
</script>