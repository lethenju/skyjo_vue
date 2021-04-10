<template>
  <div v-if="card.enable" @click="onClick()">
    <div v-if="card.visible">
      <div v-if="card.cardnb > 9" class="card card_high">
        <h2 class="card_little_numb_1">{{ card.cardnb }}</h2>
        <h1 class="card_number">{{ card.cardnb }}</h1>
        <h2 class="card_little_numb_2">{{ card.cardnb }}</h2>
      </div>
      <div v-else-if="card.cardnb > 6" class="card card_middle">
        <h2 class="card_little_numb_1">{{ card.cardnb }}</h2>
        <h1 class="card_number">{{ card.cardnb }}</h1>
        <h2 class="card_little_numb_2">{{ card.cardnb }}</h2>
      </div>
      <div v-else-if="card.cardnb > 0" class="card card_low">
        <h2 class="card_little_numb_1">{{ card.cardnb }}</h2>
        <h1 class="card_number">{{ card.cardnb }}</h1>
        <h2 class="card_little_numb_2">{{ card.cardnb }}</h2>
      </div>
      <div v-else class="card card_negative">
        <h2 class="card_little_numb_1">{{ card.cardnb }}</h2>
        <h1 class="card_number">{{ card.cardnb }}</h1>
        <h2 class="card_little_numb_2">{{ card.cardnb }}</h2>
      </div>
    </div>
    <div v-else class="card card_hidden"></div>
  </div>
</template>

<script>
import store from "../store/store.js";
export default {
  name: "Card",
  props: {
    visible: Boolean, // if its visible or not
    cardnb: Number, // value of the card
    position: Number, // position in the 3*4 board (-1 if outside)
    board: Number, // what board(player) is it in
    idx: Number, // id in the deck
    enable: Boolean, // if enabled
  },
  data() {
    return {
      shared_data: store.data,
      card: {
        idx: this.idx,
        position: this.position,
        cardnb: this.cardnb,
        visible: this.visible,
        enable: this.enable,
      },
    };
  },
  methods: {
    putCard: function () {
      this.card.visible = true;
      this.shared_data.player_data[this.board].nb_visible++;
      this.shared_data.player_data[this.board].board[this.position] = this.card;
      this.shared_data.player_data[this.board].board[
        this.position
      ].position = this.position;
      
      // Next turn
      this.shared_data.turn =
        (this.shared_data.turn + 1) % this.shared_data.player_data.length;

    },
    onClick: function () {
      if (this.shared_data.turn !== this.board) {
        // Not allowed
        return;
      }

      // board card
      if (this.position >= 0) {
        if (this.shared_data.is_card_in_hand) {
          if (!this.card.visible) {
            this.shared_data.player_data[this.board].nb_visible++;
          }
          this.shared_data.is_discard = true;
          this.shared_data.card_in_discard = this.card;
          this.card = this.shared_data.card_in_hand;
          this.shared_data.is_card_in_hand = 0;
          this.putCard();
        } else if (this.shared_data.game_state == 0) {
          this.putCard();
        }

        // Emit event to Playerboard
        this.$emit("click");
      }
    },
  },
};
</script>
