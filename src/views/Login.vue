<template>
  <div id="login">
    <b-container>
      <b-row class="justify-content-md-center mt-4 mb-4">
        <b-col col md="8">
          <b-card
            header="WELCOME BACK"
            header-bg-variant="primary"
            header-text-variant="white"
          >
            <b-card-text>
              <b-form @submit="onSubmit">
                <b-form-group
                  class="mt-2"
                  description="Enter your email"
                  label="Email"
                >
                  <b-form-input v-model="form.email" required> </b-form-input>
                </b-form-group>
                <b-form-group
                  class="mt-2"
                  description="Enter your password"
                  label="Password"
                >
                  <b-form-input
                    v-model="form.password"
                    required
                    type="password"
                  >
                  </b-form-input>
                </b-form-group>
                <b-form-group class="mt-2">
                  <b-button
                    type="submit"
                    variant="primary"
                    :enabled="acceptableSubmittion"
                  >
                    Login
                  </b-button>
                </b-form-group>
              </b-form>
            </b-card-text>
          </b-card>
        </b-col>
      </b-row>
      <b-row class="justify-content-md-center mt-4 mb-4">
        <b-col col md="8">
          <b-alert :show="error" variant="danger">
            {{ errorMessage }}
          </b-alert>
        </b-col>
      </b-row>
    </b-container>
    <router-view />
  </div>
</template>

<script>
import dotenv from "dotenv";
dotenv.config();
import router from "../router";
export default {
  name: "Login",
  data() {
    return {
      form: {
        email: "",
        password: "",
      },
      error: false,
      errorMessage: "",
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      fetch(`${process.env.VUE_APP_API_URL}/api/users/login`, {
        method: "POST",
        body: JSON.stringify(this.form),
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then((res) => {
          if (res.status == 200) {
            return res.json();
          } else {
            return Promise.reject(new Error("Try again!!!"));
          }
        })
        .then((data) => {
          localStorage.setItem("userId", data.userId);
          localStorage.setItem("token", data.token);
          router.push({ name: "PostList" });
        })
        .catch((err) => {
          this.error = true;
          this.errorMessage = "Email or password incorrect!";
          // alert("Email or password incorrect");
          console.log("Try again please");
          console.log(err);
        });
    },
  },
  computed: {
    acceptableSubmittion() {
      return this.form.email.lenght > 0 && this.form.password.lenght > 0
        ? false
        : true;
    },
  },
};
</script>

<style lang="scss">
#login {
  .row {
    .col {
      width: 400px;
      margin: 20 auto;
      padding: 10px;
    }
  }
}
</style>
