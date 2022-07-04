<template>
  <v-flex class="xs12 sm3 md2 xl1">
    <v-card>
      <v-card-title primary-title style="flex-direction: column;">
        <!-- <div class="headline">Create</div> -->
        <div class="w-100">
          <v-form
            v-if="!creatingList"
            v-model="validList"
            @submit.prevent="onCreateList()"
            @keydown.prevent.enter
          >
            <v-text-field
              v-model="list.name"
              :rules="notEmptyRules"
              label="Name"
              required
            ></v-text-field>
            <div class="d-flex justify-content-end mt-3">
              <v-btn type="submit" :disabled="!validList">Create</v-btn>
            </div>
          </v-form>
          <app-progress v-else></app-progress>
        </div>
      </v-card-title>
    </v-card>
  </v-flex>
</template>

<script>
import { notEmptyRules } from '@/validators';
import AppProgress from '@/components/AppProgress';

export default {
  name: 'BoardNewListForm',
  components: {
    AppProgress,
  },
  props: {
    creatingList: { type: Boolean, required: true },
    createList: { type: Function, required: true },
  },
  data: () => ({
    validList: false,
    list: { name: '' },
    notEmptyRules,
  }),
  methods: {
    async onCreateList() {
      if (this.validList) {
        await this.createList(this.list);
        this.list = { name: '' };
      }
    },
  },
};
</script>
