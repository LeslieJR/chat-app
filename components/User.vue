<template>
  <v-card class="shadow bg-body rounded mb-4">
    <v-row align="center">
      <v-col cols="9">
        <v-card-title>{{ user.email }}</v-card-title>
      </v-col>
      <v-col cols="3">
        <v-card-actions>
          <nuxt-link class="text-decoration-none" :to="`/users/${user._id}`">
            <v-btn class="cyan darken-1 white--text">CHAT</v-btn>
          </nuxt-link>
          <v-btn
            v-if="admin === 'true'"
            class="red white--text"
            @click="removeUser(user._id)"
            >DELETE</v-btn
          >
        </v-card-actions>
      </v-col>
    </v-row>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      admin: undefined,
      token: undefined
    };
  },
  beforeMount() {
    const admin = window.localStorage.getItem("admin");
    this.admin = admin;
    const token = window.localStorage.getItem("token");
    this.token = token;
    if (!token) {
      this.$router.push("/sign-in");
    }
  },
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  methods: {
    async removeUser(id) {
      console.log({ id, token: this.token });
      const hostname = `http://localhost:4400/api/users/delete/${id}`;
      await fetch(hostname, {
          method: "delete",
          headers: {
            token: this.token,
          },
        });
    },
  },
};
</script>
