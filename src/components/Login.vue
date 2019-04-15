<template>
  <div>
    <div class="section"></div>
    <main>
      <center>
        <img class="responsive-img" style="width: 250px;" src="https://i.imgur.com/ax0NCsK.gif">
        <div class="section"></div>

        <h5 class="indigo-text">Please, login into your account</h5>
        <div class="section"></div>

        <div class="container">
          <div
            class="z-depth-1 grey lighten-4 row"
            style="display: inline-block; padding: 32px 48px 0px 48px; border: 1px solid #EEE;"
          >
            <form class="col s12" @submit.prevent="login">
              <div class="row">
                <div class="col s12"></div>
              </div>

              <div class="row">
                <div class="input-field col s12">
                  <i class="material-icons prefix">email</i>

                  <input class="validate" type="email" name="email" id="email" v-model="email">
                  <label for="email">Enter your email</label>
                </div>
              </div>

              <div class="row">
                <div class="input-field col s12">
                  <i class="material-icons prefix">lock</i>

                  <input
                    class="validate"
                    type="password"
                    name="password"
                    id="password"
                    v-model="password"
                  >
                  <label for="password">Enter your password</label>
                </div>
                <label style="float: right;">
                  <a class="pink-text" href="#!">
                    <b>Forgot Password?</b>
                  </a>
                </label>
              </div>

              <br>
              <center>
                <div class="row">
                  <button
                    type="submit"
                    name="btn_login"
                    class="col s12 btn btn-large waves-effect green"
                  >Login</button>
                </div>
              </center>
            </form>
          </div>
        </div>
        <router-link to="/register">Create account</router-link>
      </center>

      <div class="section"></div>
      <div class="section"></div>
    </main>
  </div>
</template>
<script>
import firebase from "firebase";
export default {
  name: "login",
  data() {
    return {
      email: null,
      password: null
    };
  },
  methods: {
    login: function() {
      console.log(this.email);
      firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then(data => {
          alert(`You are logged in as ${data.user.email}`);
          this.$router.push("/");
          return;
        })
        .catch(err => {
          console.log(err);
          alert(err.message);
          return;
        });
    }
  }
};
</script>
<style scoped>
body {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}

main {
  flex: 1 0 auto;
}

body {
  background: #fff;
}

.input-field input[type="date"]:focus + label,
.input-field input[type="text"]:focus + label,
.input-field input[type="email"]:focus + label,
.input-field input[type="password"]:focus + label {
  color: #e91e63;
}

.input-field input[type="date"]:focus,
.input-field input[type="text"]:focus,
.input-field input[type="email"]:focus,
.input-field input[type="password"]:focus {
  border-bottom: 2px solid #e91e63;
  box-shadow: none;
}
</style>
