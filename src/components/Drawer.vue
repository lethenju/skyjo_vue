<template>
  <div class="card card_hidden" @click="putCardInHand()">
    <p v-if="!this.shared_data.is_card_in_hand"> Prendre une carte </p>
  </div>
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
    putCardInHand: function () {
      if (!this.shared_data.is_card_in_hand && this.shared_data.game_state==1) {
        this.shared_data.card_in_hand = this.shared_data.cards.pop();
        this.shared_data.is_card_in_hand = true;
      }
    },
  },
};
</script>
