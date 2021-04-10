<template>
  <div
    v-bind:class="{ board_container_active: active }"
    class="board_container"
  >
    <h2 class="board_player_name">
      {{ shared_data.player_data[this.player_number].name }}
    </h2>
    <div class="cards_container">
      <div
        v-for="(card, index) in this.shared_data.player_data[this.player_number]
          .board"
        v-bind:key="card.idx"
      >
        <Card
          v-if="card.enable"
          v-bind:idx="card.idx"
          v-bind:position="index"
          v-bind:cardnb="card.cardnb"
          v-bind:visible="card.visible"
          v-bind:enable="card.enable"
          v-bind:board="this.player_number"
          v-on:click="verifyBoard()"
        />
      </div>
    </div>
    <h2
      v-if="shared_data.player_data[player_number].board.length > 0"
      class="board_total_cards"
    >
      Total :
      {{
        // Sum of visible cards
        this.shared_data.player_data[player_number].board.reduce(
          (accumulator, currentValue) => {
            return currentValue.visible && currentValue.enable
              ? accumulator + currentValue.cardnb
              : accumulator;
          },
          0
        )
      }}
    </h2>
  </div>
</template>

<script>
import store from "../store/store.js";
import Card from "./Card.vue";

export default {
  name: "PlayerBoard",
  props: {
    player_number: Number,
    active: Boolean,
  },
  data: function () {
    return {
      shared_data: store.data,
    };
  },
  methods: {
    verifyBoard: function (x) {
      for (let i = 0; i <= 3; i++) {
        if (
          // They should have all the same nb value
          this.shared_data.player_data[this.player_number].board[0 + i]
            .cardnb ==
            this.shared_data.player_data[this.player_number].board[4 + i]
              .cardnb &&
          this.shared_data.player_data[this.player_number].board[4 + i]
            .cardnb ==
            this.shared_data.player_data[this.player_number].board[8 + i]
              .cardnb &&
          // They should be all visible
          this.shared_data.player_data[this.player_number].board[0 + i]
            .visible &&
          this.shared_data.player_data[this.player_number].board[4 + i]
            .visible &&
          this.shared_data.player_data[this.player_number].board[8 + i].visible
        ) {
          this.shared_data.player_data[this.player_number].board[
            0 + i
          ].enable = false;
          this.shared_data.player_data[this.player_number].board[
            4 + i
          ].enable = false;
          this.shared_data.player_data[this.player_number].board[
            8 + i
          ].enable = false;
          this.shared_data.player_data[this.player_number].board[
            0 + i
          ].visible = false;
          this.shared_data.player_data[this.player_number].board[
            4 + i
          ].visible = false;
          this.shared_data.player_data[this.player_number].board[
            8 + i
          ].visible = false;

          this.shared_data.player_data[this.player_number].nb_enable -= 3;
          // Emit event to those cards to disable them
        }
      }
      this.$emit("update");
    },
  },
  components: {
    Card,
  },
};
</script>
