<template>
  <div>
    <v-card class="mx-auto my-12" width="374">
      <v-card-title class="mt-4">
        <v-row class="mt-4" align="center" justify="center">
          <h1>Sign Up</h1>
        </v-row>
      </v-card-title>
      <v-form>
        <v-container>
          <v-row>
            <v-col cols="12">
              <v-text-field
                label="Enter email"
                placeholder="Email"
                outlined
                clearable
                hide-details="auto"
                v-model="email"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <v-text-field
                type="password"
                label="Enter password"
                placeholder="Password"
                outlined
                clearable
                hide-details="auto"
                v-model="password"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <v-text-field
                type="password"
                label="Confirm password"
                placeholder="Confirm Password"
                outlined
                clearable
                hide-details="auto"
                v-model="password2"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col md="9" offset-md="4">
              <v-btn class="primary" @click="submitData" @keyup.enter="submitData">Submit</v-btn>
            </v-col>
          </v-row>
        </v-container>
      </v-form>
    </v-card>
    <v-row class="justify-center">
      <nuxt-link to="/sign-in">Sign-in</nuxt-link>
    </v-row>
  </div>
</template>
<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      password2: "",
    };
  },
  methods: {
    async submitData() {
      //console.log({email:this.email, passw: this.password, passw2:this.password2})

      if (
        !this.email ||
        !this.password ||
        !this.password2 ||
        this.password !== this.password2
      ) {
        alert("Field missing or passwords do not match");
        return;
      }

      const hostname = "https://desolate-sea-00474.herokuapp.com/api/users";
      const body = {
        email: this.email,
        password: this.password,
        password2: this.password2,
      };
      try {
        const res = await fetch(hostname + "/sign-up", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(body),
        });
        const data = await res.json();
       
        if (data.error) {
          alert(data.error);
        } else {
          
          this.$router.push("/sign-in");
        }
      } catch (_) {}
    },
  },
};
</script>

<style scoped>
form {
  padding: 10px 20px;
}
</style>