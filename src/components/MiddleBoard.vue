<template>
  <div>
    <p class="board_total_cards">Pioche</p>
    <div class="drawer_container">
      <Drawer />
      <DiscardPile />
    </div>
    <div class="game_state_container">
      <p>{{ this.GetTextForState(shared_data.game_state) }}</p>
    </div>
    <div>
      <div v-if="shared_data.is_card_in_hand" class="card_in_hand">
        <Card
          v-bind:idx="shared_data.card_in_hand.idx"
          v-bind:position="-1"
          v-bind:cardnb="shared_data.card_in_hand.cardnb"
          v-bind:visible="true"
          v-bind:enable="true"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Card from "./Card.vue";
import Drawer from "./Drawer.vue";
import DiscardPile from "./DiscardPile.vue";

import store from "../store/store.js";
export default {
  name: "MiddleBoard",
  props: {},
  data() {
    return {
      shared_data: store.data,
    };
  },
  created: function () {
    // Shuffle cards

    for (let j = 0; j < 6; j++) {
      for (let i = -2; i <= 12; i++) {
        this.shared_data.deck.push(i);
      }
    }
    this.shared_data.deck = this.shared_data.deck.sort(
      () => Math.random() - 0.5
    );
    this.shared_data.cards = this.shared_data.deck.map((x, idx) => {
      return {
        idx: idx,
        position: -2,
        cardnb: x,
        visible: false,
        enable: true,
      };
    });
    // populate board 1
    for (let step = 0; step < 12; step++) {
      let card = this.shared_data.cards.pop();
      card.position = step;
      this.shared_data.player_data[0].board.push(card);
    }
    // populate board 2
    for (let step = 0; step < 12; step++) {
      let card = this.shared_data.cards.pop();
      card.position = step;
      this.shared_data.player_data[1].board.push(card);
    }
  },
  methods: {
    GetTextForState: function (x) {
      switch (x) {
        case 0:
          return "Joueurs, retournez 2 cartes de vos decks";
        case 1:
          return "C'est au tour de " + this.shared_data.player_data[this.shared_data.turn].name;
        case 2:
          return "Fin !"

      }
    },
  },
  components: {
    Card,
    Drawer,
    DiscardPile,
  },
};
</script>
