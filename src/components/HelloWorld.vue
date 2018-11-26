<template>
  <v-container>
    <v-layout
      text-xs-center
      wrap
    >
      <v-flex xs12>

        <!--
        <v-img
          :src="require('../assets/eos-logo.svg')"
          class="my-3"
          contain
          height="200"
        ></v-img>
        -->
      </v-flex>

      <v-flex mb-4>
        <h1 class="display-2 font-weight-bold mb-3">
          Currently Casted Votes:
        </h1>

        
        <v-data-table
          :headers="headers"
          :items="referendum"
          class="elevation-1"
        >

          <template slot="items" slot-scope="props">
            <tr @click="showIPFSImage(props.item)">
              <td class="text-xs-left">{{ props.item.dpi }}</td>
              <td class="text-xs-left">{{ props.item.vname }}</td>
              <td class="text-xs-left">{{ props.item.ipfs }}</td>
            </tr>
          </template>
        </v-data-table>

        <v-btn color="purple darken-2" v-on:click="getContractData()" class="white--text">Refresh</v-btn>

        
      </v-flex>

    <v-dialog
      v-model="dialog"
      
    >
      <v-card>
        <v-card-title class="headline">Stored Image in IPFS:</v-card-title>

        <v-layout justify-center>
          <v-img
          v-bind:src="currentIPFSURL"
          aspect-ratio="1"
          class="grey lighten-2"
          max-height="250px"
          max-width="250px"
          >
          </v-img>
        </v-layout>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
            color="purple darken-1"
            flat="flat"
            @click="dialog = false"
          >
            Close
          </v-btn>

        </v-card-actions>
      </v-card>
    </v-dialog>

      
    </v-layout>
  </v-container>
</template>

<script>
import axios from "axios";
import Vue from "vue";
export default {
  data: () => ({
    headers: [
      {
        text: "DPI #",
        value: "dpi"
      },
      { text: "Volunteer Name", value: "vname" },
      { text: "IPFS Hash", value: "ipfs" }
    ],
    referendum: [],
    dialog: false,
    currentIPFSURL: ""
  }),

  methods: {
    getContractData() {
      axios
        .get("http://35.231.64.75/referendum")
        .then(response => {
          let contractResponse = response.data.rows;
          let array = [];
          for (let element in contractResponse) {
            let dict = {
              dpi: contractResponse[element]["citizen_uid"],
              vname: contractResponse[element]["volunteer_id"],
              ipfs: contractResponse[element]["image_hash"]
            };
            array.push(dict);
          }
          Vue.set(this.$data, "referendum", array);
        })
        .catch(error => {
          console.log(error);
        });
    },
    showIPFSImage(row) {
      Vue.set(this.$data, "currentIPFSURL", "https://ipfs.io/ipfs/" + row.ipfs);
      //console.log(this.currentIPFSURL)
      Vue.set(this.$data, "dialog", true);
    }
  },
  created() {
    console.log("created");
    setInterval(() => {
      this.getContractData();
    }, 600000);
  }
};
</script>

<style>
</style>
