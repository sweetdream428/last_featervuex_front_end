<template>
  <div>
      <div class="col-12 row m-0 p-0" v-for="board in boards" :key="board._id">
        <div :style="{ backgroundImage: 'url(' + board.background + ')' }" style="width: 100%; height: 100%; background-size: cover; background-repeat: no-repeat; position: absolute;"></div>
        <div class="board-title-back m-0 pl-3" style="height: 40px;background: hsla(0,0%,100%,.16);box-shadow: 0 3px 1px -2px rgb(0 0 0 / 20%), 0 2px 2px 0 rgb(0 0 0 / 14%), 0 1px 5px 0 rgb(0 0 0 / 12%); z-index: 9;">
          <v-form
            @submit.prevent="updateBoard(board._id)"
            @keydown.prevent.enter
          >
            <div id="boardName">
              <v-text-field
                  v-model="board.name"
                  :rules="notEmptyRules"
                  class="mt-0 pt-0 board-name-change"
                  id="boardNameChange"
                ></v-text-field>
            </div>
          </v-form>
          <!-- <h3 v-if="board" style="line-height: 1.5">{{board.name}}</h3> -->
        </div>

        <v-row class="d-flex mt-4">
          <app-progress v-if="loadingBoard || loadingLists"></app-progress>

          <v-flex xs12 mx-3 v-if="listsError">
            <v-alert type="error">{{listsError.message}}</v-alert>
          </v-flex>

          <v-flex xs12 v-else>
            <app-progress v-if="loadingBoard || loadingLists"></app-progress>

            <v-row v-else>
              <v-flex
                class="xs12 sm3 md2 xl1 mt-4"
                v-for="list in lists"
                :key="list._id"
              >
                <board-list
                  :list="list"
                  :setDroppingList="onSetDroppingList"
                  :droppingList="droppingList"
                  :cardsByListId="cardsByListId"
                  :startDraggingCard="startDraggingCard"
                  :dropCard="dropCard"
                  :user="user"
                ></board-list>
              </v-flex>

              <board-new-list-form
                :creatingList="creatingList"
                :createList="createList"
                class="mt-4"
              ></board-new-list-form>
            </v-row>
          </v-flex>
        </v-row>
      </div>
  </div>
</template>

<script>
import {
  mapActions,
  mapState,
  mapGetters,
  mapMutations,
} from 'vuex';
import BoardNewListForm from '@/components/BoardNewListForm';
import AppProgress from '@/components/AppProgress';
import BoardList from '@/components/BoardList';
import { notEmptyRules } from '@/validators';

export default {
  name: 'board',
  components: {
    BoardNewListForm,
    AppProgress,
    BoardList,
  },
  data: () => ({
    notEmptyRules,
  }),
  computed: {
    ...mapState('board', ['droppingList', 'draggingCard']),
    ...mapState('boards', { loadingBoard: 'isGetPending' }),
    ...mapState('lists', { loadingLists: 'isFindPending', creatingList: 'isCreatePending', listsError: 'errorOnFind' }),
    ...mapGetters('auth', ['user']),
    ...mapGetters('lists', { findListsInStore: 'find' }),
    ...mapGetters('cards', { findCardsInStore: 'find' }),
    ...mapGetters('boards', { findBoardsInStore: 'find' }),
    boards() {
      return this.findBoardsInStore({ query: { _id: this.$route.params.id } }).data;
    },
    lists() {
      return this.findListsInStore({ query: { boardId: this.$route.params.id } }).data;
    },
    cards() {
      return this.findCardsInStore({ query: { boardId: this.$route.params.id } }).data;
    },
    cardsByListId() {
      return this.cards.reduce((byId, card) => {
        byId[card.listId] = byId[card.listId] || [];
        byId[card.listId].push(card);
        return byId;
      }, {});
    },
  },
  async mounted() {
    const boardId = this.$route.params.id;
    // const boardResponse = await this.getBoard(boardId);
    // this.board = boardResponse.data || boardResponse;

    await this.findLists({ query: { boardId } });
    await this.findCards({ query: { boardId } });
    await this.findBoards({});
  },
  methods: {
    ...mapMutations('board', ['setDroppingList', 'setDraggingCard']),
    // ...mapActions('boards', { getBoard: 'get' }),
    ...mapActions('lists', { findLists: 'find' }),
    ...mapActions('cards', { findCards: 'find' }),
    ...mapActions('boards', { findBoards: 'find' }),
    async createList(list) {
      const { List } = this.$FeathersVuex.api;
      list.boardId = this.$route.params.id;
      const newList = new List(list);
      await newList.save();
    },
    startDraggingCard(card) {
      this.setDraggingCard(card);
    },
    async dropCard() {
      if (this.droppingList && this.draggingCard.listId !== this.droppingList._id) {
        this.lists.find((list) => list._id === this.draggingCard.listId);
        this.draggingCard.listId = this.droppingList._id;
        await this.draggingCard.save();
      }
      this.setDraggingCard(null);
      this.setDroppingList(null);
    },
    onSetDroppingList(event, list) {
      event.preventDefault();
      this.setDroppingList(list);
    },
    async updateBoard(updateId) {
      const { Board } = this.$FeathersVuex.api;
      const board = new Board({ _id: updateId });
      await board.update();
    },
  },
};
</script>
<style>
  #boardName {
    margin-top: 5px;
  }
  #boardName .theme--light.v-text-field>.v-input__control>.v-input__slot:before {
    border: none;
  }
  #boardNameChange {
    font-size: 34px;
  }
</style>
