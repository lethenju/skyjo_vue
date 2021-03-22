<template>
  <div class="card card_hidden" @click="putCardInHand()"></div>
</template>

<script>
import store from "../store/store.js";
export default {
  name: "Drawer",
  props: {},
  data() {
    return {
      cards: [],
      shared_data: store.data,
    };
  },
  methods: {
    buildDeck: function () {
      this.cards = this.shared_data.deck.map((x, idx) => {
        return {
          id: idx,
          cardnb: x
        }
      });
    },
    putCardInHand: function () {
      if (this.cards.length == 0) {
        this.buildDeck();
      }
      this.shared_data.card_in_hand = this.cards.pop();
      this.shared_data.is_card_in_hand = true;

    },
  },
};
</script>
