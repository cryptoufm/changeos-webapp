<template>
  <v-app>
    <v-toolbar app dark>
      <v-toolbar-title class="headline text-uppercase">
        <v-avatar
          slot="activator"
          size="40px"
        >
          <img
            src="./assets/eos-logo.svg"
            alt="Avatar"
          >
        </v-avatar>
        <span> Changeos</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        href="https://github.com/cryptoufm"
        target="_blank"
      >
        <span class="mr-2">Github Repo</span>
      </v-btn>
    </v-toolbar>

    <v-content>
      <HelloWorld/>
    </v-content>
    <v-footer class="pa-3" dark>
      <v-spacer></v-spacer>
    <div>&copy; EEGB | JRBN | JRAS | JLLG | MFGD | LATT {{ new Date().getFullYear() }}</div>
  </v-footer>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  components: {
    HelloWorld
  },
  data() {
    return {
      //
    };
  }
};
</script>
