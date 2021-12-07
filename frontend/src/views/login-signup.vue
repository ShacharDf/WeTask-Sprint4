<template>
  <div class="login-container about">
    <lottie-player
      src="https://assets7.lottiefiles.com/packages/lf20_dyppatws.json"
      background="transparent"
      speed="1"
      style="width: 300px; height: 300px"
      loop
      autoplay
    ></lottie-player>
    <p>{{ msg }}</p>
    <div
      class="logout-user"
      v-if="loggedinUser && loggedinUser.fullname !== 'Guest'"
    >
      <h2>Loggedin User:</h2>
      <p>{{ loggedinUser.fullname }}</p>
      <button @click="logout" class="login-btn">Logout</button>
    </div>
    <div v-else class="login-form">
      <h2>Login</h2>
      <form @submit.prevent="signup">
        <input
          type="text"
          v-model="signupCred.fullname"
          placeholder="Full name"
        />
        <input
          type="password"
          v-model="signupCred.password"
          placeholder="Password"
        />
        <input
          type="text"
          v-model="signupCred.username"
          placeholder="Username"
        />
        <button class="login-btn">Login</button>
      </form>
    </div>
  </div>
</template>




<script>
import "@lottiefiles/lottie-player";

export default {
  name: "test",
  data() {
    return {
      msg: "",
      loginCred: { username: "user1", password: "123" },
      signupCred: { username: "", password: "", fullname: "" },
    };
  },
  computed: {
    users() {
      return this.$store.getters.users;
    },
    loggedinUser() {
      return this.$store.getters.loggedinUser;
    },
  },
  created() {
    this.loadUsers();
  },
  methods: {
    async login() {
      if (!this.loginCred.username) {
        this.msg = "Please enter username/password";
        return;
      }
      try {
        await this.$store.dispatch({ type: "login", userCred: this.loginCred });
        this.$router.push("/");
      } catch (err) {
        console.log(err);
        this.msg = "Failed to login";
      }
    },
    logout() {
      this.$store.dispatch({ type: "logout" });
    },
    async signup() {
      if (
        !this.signupCred.fullname ||
        !this.signupCred.password ||
        !this.signupCred.username
      ) {
        this.msg = "Please fill up the form";
        return;
      }
      await this.$store.dispatch({ type: "signup", userCred: this.signupCred });
      this.$router.push("/");
    },
    loadUsers() {
      this.$store.dispatch({ type: "loadUsers" });
    },
    async removeUser(userId) {
      try {
        await this.$store.dispatch({ type: "removeUser", userId });
        this.msg = "User removed";
      } catch (err) {
        this.msg = "Failed to remove user";
      }
    },
  },
};
</script>