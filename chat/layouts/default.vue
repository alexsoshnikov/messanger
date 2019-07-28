<template>
  <v-app app>
    <v-navigation-drawer app v-model="drawer" mobile-break-point="850">
      <v-list subheader>
        <v-subheader>
          <h2>Recent chat</h2>
        </v-subheader>
        <v-list-tile v-for="item in users" :key="item.id" @click.prevent>
          <v-list-tile-content>
            <v-list-tile-title>{{item.name}}</v-list-tile-title>
          </v-list-tile-content>

          <v-list-tile-action>
            <v-icon :color="item.id === user.id   ? '#1976d2' : 'grey'">chat_bubble</v-icon>
          </v-list-tile-action>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar app>
      <v-toolbar-side-icon v-if="width <= 850" @click="drawer = !drawer"></v-toolbar-side-icon>
      <v-btn icon @click="exit">
        <v-icon>arrow_back</v-icon>
      </v-btn>
      <v-toolbar-title>Room #{{user.room}}</v-toolbar-title>
    </v-toolbar>
    <v-content>
      <div style="height: 100%">
        <nuxt />
      </div>
    </v-content>
  </v-app>
</template>

<script>
import { mapState, mapMutations } from "vuex";
export default {
  data: () => ({
    drawer: true,
    width: null
  }),
  computed: mapState(["user", "users"]),
  methods: {
    ...mapMutations(["clearData"]),
    exit() {
      this.$socket.emit("userLeft", this.user.id, () => {
        this.$router.push("/?message=leftChat");
        this.clearData();
      });
    },
    updateWidth() {
      this.width = window.innerWidth;
    }
  },
  mounted() {
    window.addEventListener("resize", this.updateWidth);
    this.updateWidth();
  }
};
</script>
