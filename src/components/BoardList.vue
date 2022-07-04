<template>
  <v-card @dragover="setDroppingList($event, list)"
    :class="{ 'green lighten-5': droppingList === list }">
    <v-card-title primary-title>
      <v-layout column>
        <v-flex xs-12 class="justify-content-between d-flex align-items-center">
          <div class="headline">{{ list.name }}</div>
          <i class="list-ui-icon" v-on:click="removeList(list._id)">
            <b-icon icon="file-earmark-excel-fill"></b-icon>
          </i>
        </v-flex>
        <v-flex xs12 v-for="card in cardsByListId[list._id]" :key="card._id">
          <v-card class="my-1 px-2" draggable="true" @dragstart="startDraggingCard(card)" @dragend="dropCard()">
            <v-layout row>
              <v-flex>
                <div>{{ card.title }}</div>
              </v-flex>
            </v-layout>
          </v-card>
        </v-flex>
      </v-layout>
    </v-card-title>
    <v-card-actions>
      <board-new-card-form :listId="list._id" :boardId="$route.params.id" :user="user"></board-new-card-form>
    </v-card-actions>
  </v-card>
</template>

<script>
import { mapActions } from 'vuex';
import BoardNewCardForm from '@/components/BoardNewCardForm';

export default {
  name: 'BoardList',
  components: {
    BoardNewCardForm,
  },
  props: {
    list: { type: Object, required: true },
    setDroppingList: { type: Function, required: true },
    droppingList: Object,
    cardsByListId: { type: Object, required: true },
    startDraggingCard: { type: Function, required: true },
    dropCard: { type: Function, required: true },
    user: { type: Object, required: true },
  },
  methods: {
    ...mapActions('lists', { findLists: 'remove' }),
    async removeList(listId) {
      console.log('listId ', listId, this.$route.params.id);
      const { List } = this.$FeathersVuex.api;
      const list = new List({ _id: listId, boardId: this.$route.params.id });
      console.log('will remove this list ', list);
      await list.remove()
        .then((res) => {
          console.log('removed   res ', res);
        }).catch((e) => {
          console.log('remoive error ', e);
        });
    },
  },
};
</script>

<style>
.list-ui-icon {
  cursor: pointer;
}

.btn-remove-card .v-btn__content {
  background-color: transparent;
  border-style: none;
}

.absolute {
  position: absolute;
}

.modal-backdrop
{
    opacity:0 !important;
}
</style>
