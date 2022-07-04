<template>
  <v-container fluid>
    <div id="signuppage" class="row col-12">
      <div class="col-md-4 col-sm-6 col-12">
        <v-form
          v-if="!loading"
          v-model="valid"
          @submit.prevent="signUp({ valid, user, router: $router })"
          @keydown.prevent.enter
        >
          <div class="d-flex justify-content-center">
            <b-icon icon="emoji-laughing" style="zoom:2;"></b-icon>
          </div>
          <v-text-field
            v-model="user.username"
            :rules="notEmptyRules"
            label="Username"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.displayName"
            :rules="notEmptyRules"
            label="Display Name"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.password"
            :rules="notEmptyRules"
            label="Password"
            type="password"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.confirmPassword"
            :rules="confirmPasswordRules"
            label="Confirm Password"
            type="password"
            required
          ></v-text-field>
          <v-text-field
            v-model="user.imageUrl"
            :rules="notEmptyRules"
            label="Avatar URL"
            required
          ></v-text-field>
          <div class="d-flex justify-content-center">
            <v-btn type="submit" :disabled="!valid">Sign Up</v-btn>
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
  name: 'SignUp',
  components: {
    AppProgress,
  },
  data: (vm) => ({
    valid: false,
    user: {
      username: '',
      password: '',
      confirmPassword: '',
      displayName: '',
      imageUrl: '',
    },
    notEmptyRules,
    confirmPasswordRules: [(confirmPwd) => confirmPwd === vm.user.password || 'Passwords must match.'],
  }),
  computed: {
    ...mapState('users', { loading: 'isCreatePending' }),
  },
  methods: {
    ...mapActions('localAuth', ['signUp']),
  },
};
</script>
<style scoped>
  #signuppage {
    align-items: center;
    display: flex;
    justify-content: center;
    height: calc(100vh - 100px);
  }
</style>
