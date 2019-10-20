<template>
  <div>
    <nav class="navbar navbar-light navbar-default navbar-inverse" role="navigation">
      <div class="container">
        <div class="navbar-header pull-left">
          <router-link class="navbar-brand" to="/">RealWorldVue</router-link>
        </div>

        <!-- <div class="collapse navbar-collapse" id="navbarSupportedContent"> -->
          <!-- <ul class="navbar-nav mr-auto"> -->
            
          <ul>
            <li class="nav-item"><router-link to="/" class="nav-link">Home</router-link></li>
            <li v-if="user" class="nav-item"><router-link to="/new" class="nav-link"><i class="fas fa-edit"></i>&nbsp;New&nbsp;Post</router-link></li>
            <div class="username-container" v-if="user">
              <li class="nav-item"><router-link class="username no-underline nav-link" :to="'/profile/'+user.username">{{user.username}}</router-link></li>
              <li class="nav-item"><router-link class="no-underline nav-link" to="logout">Logout</router-link></li>
            </div>
            <div v-else class="login-signup-container">
              <li class="nav-item"><router-link to="/login" class="nav-link">Login</router-link></li>
              <li class="nav-item"><router-link to="/register" class="nav-link">Register</router-link></li>
            </div>
          </ul>
        <!-- </div> -->
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  data: () => {
    return {};
  },
  computed: {
    user() {
      return this.$root.user;
    }
  },
  methods: {
    logout() {
      console.log('logout');
    }
  }
};
/* from
sandep01010
sandep01010@gmail.com
qwertyuiop
*/
</script>

<style scoped>
ul li{
  display: inline;
  padding: 5px;
}
ul {
  text-align: right;
  margin-top: 10px;
  word-wrap: none;
}
.logout{
  display: none;
  position: absolute;
  bottom: -30px;
  right: 0px;
}
.username-container {
  display: inline;
  position: relative;
}
.username-container:hover .logout {
  display: inline;
}
.no-underline {
  text-decoration: none;
}
.login-signup-container {
  display: inline;
}
</style>
