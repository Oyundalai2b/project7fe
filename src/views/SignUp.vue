<template>
  <div id="signup">
    <b-container>
      <b-row class="justify-content-md-center mt-4 mb-4">
        <b-col col md="8">
          <b-card
            header="WELCOME TO GROUPOMANIA"
            header-bg-variant="primary"
            header-text-variant="white"
          >
            <b-form @submit="onSubmit" @reset="onReset">
              <b-form-group
                class="mt-2"
                id="input-group-1"
                label="Email address:"
                label-for="input-1"
                description="We'll never share your email with anyone else."
              >
                <b-form-input
                  id="input-1"
                  v-model="form.email"
                  type="email"
                  placeholder="Enter email"
                  required
                ></b-form-input>
              </b-form-group>

              <b-form-group
                class="mt-2"
                id="input-group-2"
                label="Your Password:"
                label-for="input-2"
              >
                <b-form-input
                  id="input-2"
                  v-model="form.password"
                  placeholder="Enter Password"
                  type="password"
                  required
                ></b-form-input>
              </b-form-group>

              <b-button class="signup mt-2" type="submit" variant="primary"
                >Sign Up</b-button
              >
              <b-button class="mt-2" type="reset" variant="danger"
                >Reset</b-button
              >
            </b-form>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
    <b-toast id="my-toast" variant="success" solid>
      <template #toast-title>
        <div class="d-flex flex-grow-1 align-items-baseline">
          <strong class="mr-auto">Profile created!</strong>
        </div>
      </template>
      Your profile has been created successfully.
      <br />Please click <a href="/login">here</a> to login and update your
      profile.
    </b-toast>
    <b-toast id="error-toast" variant="danger" solid>
      <template #toast-title>
        <div class="d-flex flex-grow-1 align-items-baseline">
          <strong class="mr-auto">Error!</strong>
        </div>
      </template>
      Email has already been used!
    </b-toast>
  </div>
</template>

<script>
export default {
  name: "SignUp",
  data() {
    return {
      form: {
        email: "",
        password: "",
      },
    };
  },
  mounted() {},
  methods: {
    onSubmit(event) {
      event.preventDefault();
      fetch(`${process.env.VUE_APP_API_URL}/api/users/signup`, {
        method: "POST",
        body: JSON.stringify(this.form),
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then((res) => {
          if (res.status == 201) {
            this.$bvToast.show("my-toast");

            this.form.email = "";
            this.form.password = "";
          } else {
            this.$bvToast.show("error-toast");
          }
        })
        .catch((err) => {
          // this.$bvToast.show("error-toast");
          console.log(err);
        });
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";

      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
};
</script>

<style lang="scss">
#signup {
  .row {
    .col {
      width: 400px;
      margin: 0 auto;
      margin-bottom: 20px !important;
      div.card-body {
        line-height: 40px;
      }
    }
  }
}
button.signup {
  margin-right: 20px;
}
</style>
