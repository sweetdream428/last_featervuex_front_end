<template>
  <v-app-bar app>
    <div class="v-toolbar__title ml-0 pl-3">
      <v-btn v-if="!user" :to="{ name: 'home' }" class="v-btn--flat">
        Demo
      </v-btn>
      <v-btn v-if="user" :to="{ name: 'boards' }" class="v-btn--flat">
        Demo
      </v-btn>
    </div>
    <v-spacer></v-spacer>

    <div v-if="!user" class="v-toolbar__items">
      <v-btn :to="{ name: 'login' }" class="v-btn--flat">Login</v-btn>
      <v-btn :to="{ name: 'signup' }" class="v-btn--flat">Sign up</v-btn>
    </div>
    <v-toolbar-items v-else>
      <v-layout justify-content align-center class="mr-4">
        <b-nav-item-dropdown class="avatar-drop">
          <!-- Using 'button-content' slot -->
          <template #button-content>
            <b-img :src="user.imageUrl" alt="avatar" class="rounded-circle avatar-img"></b-img>
          </template>
          <b-dropdown-item @click="logout()" class="logout-btn">LOGOUT</b-dropdown-item>
        </b-nav-item-dropdown>
      </v-layout>
    </v-toolbar-items>
  </v-app-bar>
</template>

<script>
export default {
  name: 'AppNavbar',
  props: {
    user: Object,
    logout: { type: Function, required: true },
  },
};
</script>

<style>
  .v-toolbar__content {
    background-color: #1565c0;
  }
  .v-toolbar__title {
    font-size: 20px;
    font-weight: 500;
    letter-spacing: .02em;
    white-space: nowrap;
    color: #fff;
    cursor: pointer;
  }
  .v-toolbar__items .v-btn:not(.v-btn--floating):not(.v-btn--icon) {
    height: inherit;
    margin: 0;
    color: #fff;
    box-shadow: none;
  }

  .v-toolbar__title .v-btn--flat {
    box-shadow: none;
  }
  .v-toolbar__title .v-btn--flat .v-btn__content {
    color: #fff;
    font-size: 20px;
  }
  .v-toolbar__title .theme--light.v-btn.v-btn--has-bg {
    background-color: #1565c0;
  }
  .v-btn:not(.v-btn--text):not(.v-btn--outlined).v-btn--active:before {
    background-color: #1565c0;
  }
  .v-toolbar__items .theme--light.v-btn.v-btn--has-bg {
    background-color: #1565c0;
  }
  .avatar-drop {
    list-style-type: none;
  }
  .avatar-img {
    height: 30px;
  }

  .dropdown-toggle::after {
    display: none !important;
  }
  .logout-btn {
    width: 100%;
  }
  .v-application ul {
    padding-left: 0 !important;
  }
</style>
