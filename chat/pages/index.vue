<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8>
      <v-card min-width="500">
        <v-snackbar v-model="snackbar" :timeout="6000" top>
          {{ message }}
          <v-btn dark text @click="snackbar = false">Close</v-btn>
        </v-snackbar>
        <v-card-title>
          <h1>Chat</h1>
        </v-card-title>
        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-text-field v-model="name" :counter="16" :rules="nameRules" label="Name" required></v-text-field>
            <v-text-field v-model="room" :rules="roomRules" label="Room" required></v-text-field>
            <v-btn :disabled="!valid" color="primary" class="mr-4" @click="submit">Login</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import { mapMutations } from "vuex";
export default {
  layout: "start",
  head: {
    title: "Chat"
  },
  sockets: {
    connect: function() {
      console.log("socket connected");
    }
  },
  data: () => ({
    snackbar: false,
    message: "",
    valid: true,
    name: "",
    nameRules: [
      v => !!v || "Name is required",
      v => (v && v.length <= 16) || "Name must be less than 10 characters"
    ],
    room: "",
    roomRules: [
      v => !!v || "Enter the right room",
      v => /[0-9]/g.test(v) || "Invalid room"
    ]
  }),
  mounted() {
    const { message } = this.$route.query;
    if (message === "leftChat") {
      this.message = "You left the chat!";
    }

    this.snackbar = !!this.message;
  },
  methods: {
    ...mapMutations(["setUser"]),
    submit() {
      if (this.$refs.form.validate()) {
        const user = {
          name: this.name,
          room: this.room
        };

        this.$socket.emit("UserJoined", user, data => {
          if (typeof data === "string") {
            console.error(data);
          } else {
            user.id = data.userID;
            this.setUser(user);
            this.$router.push("/chat");
          }
        });
      }
    }
  }
};
</script>