<template>
  <nav>
    <div class='nav-wrapper blue'>
      <div class='container'>
		<img src="http://localhost:8080/src/assets/IT SOURCECODE_ICON-21.png" alt="IT Sourcecode.com" width="68" height="68" style="padding: 0px 10px 0px 00px;">
        <router-link
          to="/"
          class="brand-logo">Employee Manager</router-link>
		  
          <ul class='right'>
            <li v-if='isLoggedIn'><span class='email white-text'>{{currentUser}}</span></li>
            <li v-if='isLoggedIn'><router-link to='/'>Dashboard</router-link></li>
            <li v-if='!isLoggedIn'><router-link to='/login'>Login</router-link></li>
            <li v-if='!isLoggedIn'><router-link to='/register'>Register</router-link></li>
            <li v-if='isLoggedIn'><button v-on:click='logout' class='btn white black-text'>Logout</button></li>
          </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import firebase from 'firebase';
export default {
	name: 'navbar',
	data() {
		return {
			isLoggedIn: false,
			currentUser: false
		};
	},
	created() {
		if (firebase.auth().currentUser) {
			this.isLoggedIn = true;
			this.currentUser = firebase.auth().currentUser.email;
		}
	},
	methods: {
		logout: function() {
			firebase
				.auth()
				.signOut()
				.then(() => {
					this.$router.go({ path: this.$router.path });
				});
		}
	}
};
</script>

<style scoped>
.email {
	padding-right: 10px;
}
</style>