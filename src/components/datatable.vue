
<template>
  <v-app>
    <v-data-table
      v-show="this.showtable == true"
      :headers="headers"
      :items="players"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ props }">
              <v-btn
                icon="mdi-shuffle-variant"
                class="mb-2"
                color="primary"
                @click="shuffleBtn()"
              >
              </v-btn>
              <v-btn
                icon="mdi-plus"
                class="mb-2"
                color="primary"
                v-bind="props"
              >
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-text-field
                      v-model="editedItem.name"
                      label="Player name"
                    ></v-text-field>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue-darken-1" variant="text" @click="close">
                  Cancel
                </v-btn>
                <v-btn color="blue-darken-1" variant="text" @click="save">
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5"
                >Are you sure you want to delete this item?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue-darken-1" variant="text" @click="closeDelete"
                  >Cancel</v-btn
                >
                <v-btn
                  color="blue-darken-1"
                  variant="text"
                  @click="deleteItemConfirm"
                  >OK</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon class="me-2" size="small" @click="editItem(item)">
          mdi-pencil
        </v-icon>
        <v-icon size="small" @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>
    <div v-show="this.spinDialog == true">
      <v-container class="bg-surface-variant">
        <v-row no-gutters>
          <v-divider inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-icon @click="backtodatatable()">mdi-arrow-left-bold</v-icon>
        </v-row>
      </v-container>

      <div class="text-center">
        <h1>Total number of players: {{ this.players.length }}</h1>
        <v-btn
          :disabled="Btnclicked == true"
          icon="mdi-swap-horizontal-bold"
          size="large"
          @click="teamshuffle()"
          hide-details
        ></v-btn>
        <br>
        <br>
        <br>
        <br>
        <div class="text-center">
            <v-layout style="text-center" v-if="refreshStatus">
            <v-progress-linear     
                :size="90"
                color="amber"
                indeterminate
            ></v-progress-linear>
            </v-layout>
        </div>
        <v-card class="mx-auto" max-width="700">
          <h2
            v-if="Btnclicked == true && refreshStatus == false"
            class="teamname"
          >
            Team A
          </h2>
          <v-list
            style="text-align: left"
            class="teamlist"
            v-show="aftershuffle == true && refreshStatus == false"
            :items="Ateam"
          ></v-list>
          <h2
            v-if="Btnclicked == true && refreshStatus == false"
            class="teamname"
          >
            Team B
          </h2>
          <v-list
            style="text-align: left"
            class="teamlist"
            v-show="aftershuffle == true && refreshStatus == false"
            :items="Bteam"
          ></v-list>
          <h2
            v-show="
              Btnclicked == true &&
              teamcdisable == true &&
              refreshStatus == false
            "
            class="teamname"
          >
            Common
          </h2>
          <v-list
            style="text-align: left"
            class="teamlist"
            v-show="
              aftershuffle == true &&
              teamcdisable == true &&
              refreshStatus == false
            "
            :items="Cteam"
          ></v-list>
        </v-card>
      </div>
    </div>
  </v-app>
</template>
<script>
export default {
  data: () => ({
    refreshStatus: false,
    teamcdisable: false,
    commonplayer: null,
    Btnclicked: false,
    aftershuffle: false,
    Ateam: [],
    Bteam: [],
    Cteam: [],
    showtable: true,
    spinDialog: false,
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        title: "Players",
        align: "start",
        sortable: true,
        key: "name",
        width: "95%",
      },

      { title: "Actions", key: "actions", sortable: false },
    ],
    players: [
      {
        name: "AYYAS",
      },
      {
        name: "RAM",
      },
      {
        name: "AMRE",
      },
      {
        name: "KARTHIKEYAN",
      },
      {
        name: "SAI",
      },
      {
        name: "MOHAN",
      },
      {
        name: "KARTHI",
      },
      {
        name: "KEERTHI",
      },
      {
        name: "SARAVANAN",
      },
      {
        name: "SURESH",
      },
      {
        name: "RAGAVAN",
      },
      {
        name: "JEEVA",
      },
      {
        name: "ARJUN",
      },
      {
        name: "ABI",
      },
      {
        name: "JANA",
      },
      {
        name: "BARATH",
      },
    ],
    editedIndex: -1,
    editedItem: {
      name: "",
    },
    defaultItem: {
      name: "",
    },
  }),
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  //   created() {
  //     this.initialize();
  //   },

  methods: {
    sliceAndGetRemainder(array, number) {
      // Get the sliced portion
      const sliced = array.slice(0, number);
      // Get the remainder portion
      const remainder = array.slice(number);

      return { sliced, remainder };
    },
    findLargestArray(...arrays) {
      let largestArray = arrays[0]; // Assume the first array is the largest

      for (const arr of arrays) {
        if (arr.length > largestArray.length) {
          largestArray = arr; // Update the largest array
        }
      }

      return largestArray;
    },
    findShortestArray(...arrays) {
      let minIndex = 0; // Assume the first array is the shortest
      let minLength = arrays[0].length; // Store its length

      // Loop through arrays to find the shortest one
      for (let i = 1; i < arrays.length; i++) {
        if (arrays[i].length < minLength) {
          minIndex = i; // Update the index of the shortest array
          minLength = arrays[i].length; // Update the minimum length
        }
      }

      return { index: minIndex, array: arrays[minIndex], length: minLength };
    },
    randomlyDivideArray(array) {
      // Shuffle the array
      const shuffledArray = array.slice().sort(() => Math.random() - 0.5);

      // Split the array into two parts
      const midIndex = Math.floor(shuffledArray.length / 2);
      const part1 = shuffledArray.slice(0, midIndex);
      const part2 = shuffledArray.slice(midIndex);
      return [part1, part2];
    },

    // initialize() {
    //   this.players =
    // },
    async teamshuffle() {
      this.refreshStatus = true;

      var teammembers = [];
      await this.players.map((e) => {
        teammembers.push(e.name);
      });

      this.aftershuffle = true;
      this.Btnclicked = true;

      //   need to be work
      const [group1, group2] = this.randomlyDivideArray(teammembers);
      if (group1.length == group2.length) {
        this.teamcdisable = false;
      } else {
        this.teamcdisable = true;
        const largestArray = this.findLargestArray(group1, group2);
        const shortestArray = this.findShortestArray(group1, group2);
        var difference = this.sliceAndGetRemainder(
          largestArray,
          shortestArray.length
        );
        this.Cteam = difference.remainder;
      }
      this.Ateam = group1;
      this.Bteam = group2;
        setTimeout(() => {
            this.refreshStatus = false;
        }, 3000);
    },
    backtodatatable() {
      this.spinDialog = false;
      this.showtable = true;
    },
    shuffleBtn() {
      this.showtable = false;
      this.spinDialog = true;
      //   to be work for reload shuffle
      this.aftershuffle = false;
      this.Btnclicked = false;
    },
    editItem(item) {
      this.editedIndex = this.players.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.players.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.players.splice(this.editedIndex, 1);
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.players[this.editedIndex], this.editedItem);
      } else {
        this.players.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>
<style scoped>
.v-progress-circular {
  margin: 1rem;
}
.teamname {
  text-align: left;
}
</style>