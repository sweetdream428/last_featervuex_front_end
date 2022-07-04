<template>
  <v-container fluid>
    <div id="loginpage" class="row col-12">
      <div class="col-md-4 col-sm-6 col-12">
        <v-form
          v-if="!loading"
          v-model="valid"
          @submit.prevent="login({ valid, user, router: $router })"
          @keydown.prevent.enter
        >
          <div class="d-flex justify-content-center">
            <b-icon icon="person-badge-fill" style="zoom:2;"></b-icon>
          </div>
          <v-text-field
            v-model="user.username"
            :rules="notEmptyRules"
            label="Username"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.password"
            :rules="notEmptyRules"
            label="Password"
            type="password"
            required
          ></v-text-field>
          <div class="d-flex justify-content-center">
            <v-btn type="submit" :disabled="!valid">Login</v-btn>
          </div>
        </v-form>
        <app-progress v-else></app-progress>
      </div>
    </div>
  </v-container>
</template>

<script>
import { mapState, mapActions } from 'vuex';
import AppProgress from '@/components/AppProgress';
import { notEmptyRules } from '@/validators';

export default {
  name: 'Login',
  components: {
    AppProgress,
  },
  data: () => ({
    valid: false,
    user: {
      username: '',
      password: '',
    },
    notEmptyRules,
  }),
  computed: {
    ...mapState('auth', { loading: 'isAuthenticatePending' }),
  },
  methods: {
    ...mapActions('auth', ['authenticate']),
    ...mapActions('localAuth', ['login']),
  },
};
</script>
<style>
  #loginpage {
    align-items: center;
    display: flex;
    justify-content: center;
    height: calc(100vh - 100px);
  }
</style>
