<template>
  <div>
    <Login :isreg="isreg" @change-prop="changeProp" />
    <b-navbar class="bar">
      <b-navbar-nav>
        <b-nav-item to="/">Home</b-nav-item>
        <!-- <b-nav-item to="/about">About</b-nav-item> -->
      </b-navbar-nav>

      <b-navbar-nav class="ml-auto">
        <!-- Navbar dropdowns -->
        <!-- <b-nav-item-dropdown text="User" right v-if="access_token">
          <b-dropdown-item href="#">Account</b-dropdown-item>
          <b-dropdown-item href="#">Settings</b-dropdown-item>
        </b-nav-item-dropdown> -->
        <!-- End of navbar dropdowns -->

        <b-button
          class="myButton"
          v-b-modal.LoginModal
          @click="changeProp(false)"
          v-if="!access_token"
          >Login</b-button
        >
        <b-button class="myButton" @click="logoutUser" v-if="access_token"
          >Logout</b-button
        >
      </b-navbar-nav>
    </b-navbar>
  </div>
</template>

<script>
import { mapState } from "vuex";
import Login from "@/components/LoginModal";

export default {
  components: {
    Login
  },
  data() {
    return {
      isreg: false
    };
  },
  methods: {
    changeProp(value) {
      this.isreg = value;
    },
    logoutUser() {
      localStorage.removeItem("access_token");
      localStorage.removeItem("activeUserId");
      this.$store.commit("SET_USER_CREDENTIALS", {
        token: "",
        activeUserId: ""
      });
      this.$swal({
        icon: "info",
        title: "you've been logged out successfully."
      });
      if (this.$router.currentRoute.fullPath !== "/home") {
        this.$router.push("/home");
      }
    }
  },
  computed: mapState(["access_token"])
};
</script>

<style scoped>
.bar {
  background-color: white;
  border-bottom: 3px solid coral;
  font-weight: 700;
  height: 8vh;
}
</style>
