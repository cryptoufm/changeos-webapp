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

        <v-btn color="info" v-on:click="getContractData()">Refresh</v-btn>

        
      </v-flex>

    <v-dialog
      v-model="dialog"
      
    >
      <v-card>
        <v-card-title class="headline">Stored Image in IPFS:</v-card-title>

        <v-img
          :src="QmYFXBeBEZ7QHMtuxJKdSa1iU6sViRqEoRYFoeSJegGU82"
          aspect-ratio="1"
          class="grey lighten-2"
        >
        </v-img>

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
import Vue from 'vue';
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
    referendum: [
      {
        dpi: "3001973810101",
        vname: "eegod",
        ipfs: "QmYFXBeBEZ7QHMtuxJKdSa1iU6sViRqEoRYFoeSJegGU82"
      }
    ],
    dialog: false,
    currentIPFSHash: "QmYFXBeBEZ7QHMtuxJKdSa1iU6sViRqEoRYFoeSJegGU82",
  }),
  //QmYFXBeBEZ7QHMtuxJKdSa1iU6sViRqEoRYFoeSJegGU82
  methods: {
    getContractData() {
      axios
        .get("http://35.231.64.75/referendum")
        .then(response => {
          let contractResponse = response.data.rows;
          let array = []
          for (let element in contractResponse){
            console.log(contractResponse[element])
            array.push(contractResponse[element])
          }
          Vue.set(this.$data, "referendum", array)
          console.log(this.referendum)
        })
        .catch(error => {
          console.log(error);
        });
    },
    showIPFSImage(row){
      //Vue.set(this.$data, "currentIPFSHash", row.ipfs)
      Vue.set(this.$data, "dialog", true);
    }
  }
};
</script>

<style>
</style>
