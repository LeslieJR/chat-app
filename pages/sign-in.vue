<template>
  <div>
    <v-card class="mx-auto my-12" width="374">
      <v-card-title class="mt-4">
        <v-row align="center" justify="center">
          <h1>Sign In</h1>
        </v-row>
      </v-card-title>
      <v-form>
        <v-container>
          <v-row>
            <v-col>
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
            <v-col>
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
            <v-col md="9" offset-md="4">
              <v-btn class="primary" @click="submitData" @keyup.enter="submitData">Submit</v-btn>
            </v-col>
          </v-row>
        </v-container>
      </v-form>
    </v-card>
    <v-row class="justify-center">
      <nuxt-link  to="/sign-up">Sign-up</nuxt-link>  
    </v-row>
  </div>
</template>
<script>

export default ({
  data() {
    return{
      email:"",
      password:""
    }
  },
  methods:{
    async submitData() {
      
      if (!this.email || !this.password) {
        alert("Required field missing");
        return
      }
      const hostname = "http://localhost:4400/api/users";
      const body = {
        email: this.email,
        password: this.password,
      };
      const res = await fetch(hostname + "/sign-in", {
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
        console.log(data)
        window.localStorage.setItem('token', data.token)
        window.localStorage.setItem('userId', data.userId)
        window.localStorage.setItem('admin', data.admin)
        this.$router.push("/users");
      }
    },

  }
})
</script>

<style scoped>
form {
  padding: 10px 20px;
}
</style>