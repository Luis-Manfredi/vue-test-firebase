<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
    >
      <!--  -->
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Test Vue-Firebase</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-card
        elevation="2"
        shaped
      >
      
      <v-form v-model="valid">
        <v-container>

          <v-col
            cols="12"
            md="4"
          >
            <v-text-field
              v-model="user.firstname"
              :rules="nameRules"
              :counter="15"
              label="First name"
              required
            ></v-text-field>
          </v-col>

          <v-col
            cols="12"
            md="4"
          >
            <v-text-field
              v-model="user.lastname"
              :rules="nameRules"
              :counter="15"
              label="Last name"
              required
            ></v-text-field>
          </v-col>

          <v-col
            cols="12"
            md="4"
          >
            <v-text-field
              v-model="user.email"
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
          </v-col>

          <v-btn
            type="submit"
            @click="submit"
          >
          submit
          </v-btn>
        </v-container>
      </v-form>
      </v-card>
    </v-main>
  </v-app>
</template>

<script>
// Initialization
import { initializeApp } from 'firebase/app';
import { getDatabase, push, ref} from 'firebase/database';
import config from './config';
const app = initializeApp(config);
const db = getDatabase(app);
let usersRef = ref(db, 'users');

export default {
  name: 'App',

  firebase: {
    users: usersRef
  },

  components: {

  },

  data: () => ({ 
    drawer: false,
    valid: false,
    user: {
      firstname: '',
      lastname: '',
      email: ''
    },
    nameRules: [
      v => !!v || 'Name is required',
      v => v.length <= 15 || 'Name must be less than 10 characters',
    ],
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /.+@.+/.test(v) || 'E-mail must be valid',
    ],
  }),

  methods: {
    submit () {
      // Send data to Firebase Realtime Database
      push(usersRef, {
        firstname: this.user.firstname,
        lastname: this.user.lastname,
        email: this.user.email
      });

      // console.log(this.user.firstname);
      // console.log(this.user.lastname);
      // console.log(this.user.email);

      // Clear inputs
      this.user.firstname = '';
      this.user.lastname = '';
      this.user.email = '';
    }
  }
};
</script>

<style>
  .v-main {
    background-color: #393939;
  }

  .v-card {
    width: 400px;
    margin: 20px;
  }

  .col-md-4 {
    max-width: none;
  }
</style>