<template>
  <v-app>
    <!-- INICIO-->
    <Header />
    <Articles />
    <Contact />
    <v-main>
      <v-container>
        <v-dialog v-model="loading.estado" hide-overlay persistent width="300">
          <v-card color="primary" dark>
            <v-card-text>
              {{ loading.titulo }}
              <v-progress-linear
                indeterminate
                color="white"
                class="mb-0"
              ></v-progress-linear>
            </v-card-text>
          </v-card>
        </v-dialog>

        <router-view />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { mapState } from "vuex";

import Header from "@/components/Header.vue";
import Articles from "@/components/Articles.vue";
import Contact from "@/components/Contact.vue";

export default {
  name: "App",
  components: {
    Header,
    Articles,
    Contact,
  },
  data: () => ({
    //
  }),
  computed: {
    ...mapState(["loading"]),
  },
};
</script>
