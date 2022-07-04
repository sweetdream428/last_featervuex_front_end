<template>
  <v-flex class="col-xl-2 col-md-4 col-sm-6 col-12 my-2">
    <v-card height="260px">
      <v-card-title class="d-block">
        <div class="headline"></div>
        <div class="mt-5">
          <v-form
            v-if="!creating"
            v-model="valid"
            @submit.prevent="onCreateBoard()"
            @keydown.prevent.enter
          >
            <v-text-field
              v-model="board.name"
              :rules="notEmptyRules"
              label="Name"
              required
            ></v-text-field>
            <v-text-field
              v-model="board.background"
              :rules="notEmptyRules"
              label="Background"
              required
            ></v-text-field>
            <v-btn type="submit" :disabled="!valid" style="position:relative; margin-top: 40px;">Create</v-btn>
          </v-form>
          <app-progress v-else></app-progress>
        </div>
      </v-card-title>
    </v-card>
  </v-flex>
</template>

<script>
import AppProgress from '@/components/AppProgress';
import { notEmptyRules } from '@/validators';

export default {
  name: 'BoardNewBoardForm',
  components: {
    AppProgress,
  },
  props: {
    creating: { type: Boolean, required: true },
    createBoard: { type: Function, required: true },
  },
  data: () => ({
    valid: false,
    board: {
      name: '',
      background: '',
    },
    notEmptyRules,
  }),
  methods: {
    async onCreateBoard() {
      if (this.valid) {
        await this.createBoard(this.board);
        this.board = {
          name: '',
          background: '',
        };
      }
    },
  },
};
</script>
