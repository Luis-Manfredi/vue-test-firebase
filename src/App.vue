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

      <v-row align-content="space-between">
        <!-- Form card -->
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

        <!-- Show data card -->

        <v-card
          shaped
          elevation="2"
          height="150px"
          style="padding: 20px; text-align: center;"
        >

          <v-col>
            <v-btn
              elevation="2"
              @click="loadData()"
              style="margin-bottom: 20px;"
            >Load Data</v-btn>

            <p v-if="isPressed">Check the developer console</p>
            
          </v-col>
        </v-card>                 
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
// Imports
import { initializeApp } from 'firebase/app';
import { getDatabase, push, ref, onValue} from 'firebase/database';
import config from './config';

// Initialization
const app = initializeApp(config);
const db = getDatabase(app);
const usersRef = ref(db, 'users');

export default {
  name: 'App',

  firebase: {
    users: usersRef
  },

  components: { },

  data: () => ({
    isPressed: false, 
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

      // Clear inputs
      this.user.firstname = '';
      this.user.lastname = '';
      this.user.email = '';
    },

    loadData () {
      onValue(usersRef, function(snapshot) {
        var data = snapshot.val();
        for (let index in data) {
          console.log(data[index])
        }
      });

      this.isPressed = true;
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