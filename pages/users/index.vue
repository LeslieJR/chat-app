<template>
  <v-container>
    <v-row class="mt-4">
      <v-col>
        <nuxt-link class="text-decoration-none" to="/sign-in">
          <v-btn @click="logout">Logout</v-btn>
        </nuxt-link>
      </v-col>
    </v-row>
    <v-row align="center">
      <v-col>
        <h1 class="text-center mb-4">Users</h1>
        <v-row v-for="(user, index) in users" :key="index">
          <v-col cols="9">
            <h2>{{user.email}}</h2>
          </v-col>
          <v-col cols="3">
            <nuxt-link class="text-decoration-none" :to="`/users/${user._id}`">
              <v-btn class="cyan darken-1 white--text">CHAT</v-btn>
            </nuxt-link>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      users: [],
    };
  },
  beforeMount() {
    const token = window.localStorage.getItem("token");
    if (!token) {
      this.$router.push("/sign-in");
    }

    this.getUsers(token);
  },
  methods: {
    async getUsers(token) {
      try {
        const hostname = "http://localhost:4400/api/users/all";
        const res = await fetch(hostname, {
          headers: {
            token,
          },
        });
        const data = await res.json();

        if(data.error){
          alert(data.error)
          return this.$router.push('/sign-in')
        }

        this.users = data;
      } catch (err) {
        alert(err)
      }
    },
    logout(){
      window.localStorage.clear();
      this.$router.push('/sign-in')
    }
  },
};
</script>
