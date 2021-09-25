<template>
  <v-container>
    <nuxt-link to="/users">Go back</nuxt-link>
    <v-card class="d-flex flex-column fill-height">
      <v-card-title class="d-flex justify-center"> chat </v-card-title>
      <v-card-text class="flex-grow-1 overflow-y-auto">
        <div v-for="(msg, index) in messages" :key="index">
          <v-chip
            class="d-flex justify-end pa-4 mb-2"
            v-if="msg.user_owner == user_id"
            style="height: auto; white-space: normal"
          >
            {{ msg.message }}
          </v-chip>
          <v-chip
            class="d-flex justify-start pa-4 mb-2"
            v-else-if="msg.user_owner != user_id"
            dark
            style="height: auto; white-space: normal"
          >
            {{ msg.message }}
          </v-chip>
        </div>
        <!--<v-text-field v-model="message"> </v-text-field>-->
        <v-text-field
            v-model="message"
            :append-outer-icon="'mdi-send'"
            filled
            clear-icon="mdi-close-circle"
            clearable
            label="Message"
            type="text"
            @click:append-outer="sendMessage"
             
          ></v-text-field>
        
      </v-card-text>
    </v-card>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      messages: [],
      params_id: undefined,
      user_id: undefined,
      message: undefined,
    };
  },
  async beforeMount() {
    this.params_id = this.$route.params.id;
    this.user_id = window.localStorage.getItem("userId");
    const token = window.localStorage.getItem("token");
    if (!token) {
      this.$router.push("/sign-in");
    }
    await this.getMessages(token);
  },
  methods: {
    async getMessages(token) {
      try {
        const hostname = "http://localhost:4400/api/messages/chat";
        const user_one_id = window.localStorage.getItem("userId");
        const user_two_id = this.params_id;
        const body = {
          user_one_id,
          user_two_id,
        };
        console.log(body);
        const res = await fetch(hostname, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            token,
          },
          body: JSON.stringify(body),
        });

        const data = await res.json();
        console.log(data);
        if (data.error) {
          alert(data.error);
          return this.$router.push("/sign-in");
        }
        this.messages = data;
      } catch (err) {
        alert(err);
      }
    },
    async sendMessage() {
      try {
        const token = window.localStorage.getItem("token");
        const user_one_id = window.localStorage.getItem("userId");
        const user_two_id = this.params_id;
        const body = {
          user_one_id,
          user_two_id,
          user_owner_id: user_one_id,
          message: this.message,
        };
        const res = await fetch("http://localhost:4400/api/messages/create", {
          method: "POST",
          headers: {
            token,
            "Content-Type": "application/json",
          },
          body: JSON.stringify(body),
        });
        const data = await res.json();
        console.log(data);
        if (data.error) {
          alert({ error: data.error });
        } else {
          this.messages = [];
          await this.getMessages(token);
        }
      } catch (err) {
        alert({ error: err });
      }
    },
  },
};
</script>
