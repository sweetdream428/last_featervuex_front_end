<template>
  <div>
    <b-card
      overlay
      footer=""
      footer-tag="footer"
      :img-src="board.background"
      img-alt="Card Image"
      text-variant="white"
      :title="board.name"
    >
      <b-link v-if="board.background" :to="{ name: 'board', params: { id: board._id } }">
        <div style="height: 260px; width: 100%; position: absolute; left: 0px; top: 0px;"></div>
      </b-link>
    </b-card>
    <div class="board-card-footer d-flex justify-content-between px-1 align-items-center">
      <div>
        <b-img :src="user.imageUrl" class="rounded-circle card-avatar"></b-img> &nbsp;
        <VueMomentsAgo prefix="You - " suffix="ago" :date="board.updatedAt" lang="en"></VueMomentsAgo>
      </div>
      <b-icon icon="file-earmark-excel-fill" class="remove-board" v-on:click="removeBoard(board._id)" style="z-index: 99999;"></b-icon>
    </div>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
import VueMomentsAgo from 'vue-moments-ago';

export default {
  name: 'BoardListCard',
  components: {
    VueMomentsAgo,
  },
  props: {
    board: { type: Object, required: true },
  },
  computed: {
    ...mapGetters('auth', ['user']),
  },
  methods: {
    ...mapActions('boards', { findBoards: 'remove' }),
    async removeBoard(boardId) {
      const { Board } = this.$FeathersVuex.api;
      const board = new Board({ _id: boardId });
      await board.remove()
        .then(() => {
        });
    },
  },
};
</script>
<style scoped>
  .card-footer {
  background-color: #fff;
  color: #000;
  height: 60px;
  display: flex;
  align-items: center;
}
.card {
  box-shadow: 0 3px 1px -2px rgba(0,0,0,.2),0 2px 2px 0 rgba(0,0,0,.14),0 1px 5px 0 rgba(0,0,0,.12)!important;
  cursor: pointer;
  height: 260px;
}

.card-img {
  height: 200px;
}

.card-avatar {
  height: 20px;
}
.board-card-footer {
  position: relative;
  bottom: 40px;
}
.remove-board {
  cursor: pointer;
}
.remove-card:hover {
  transform: scale(1.5);
}

.vue-moments-ago {
  font-size: 14px;
}
</style>
