<template>
<div  @click="onClick()">
  <div v-if="show" class="card">
    <h1 class="card_number">{{ card.cardnb }}</h1>
  </div>
  <div v-else class="card card_hidden"></div>
  </div>
</template>

<script>
import store from "../store/store.js";
export default {
  name: "Card",
  props: {
    visible: String,
    cardnb: Number,
  },
  data() {
    return {
      shared_data: store.data,
      card: {cardnb: this.cardnb},
      show: this.visible == "true",
    };
  },
  methods: {
    onClick: function () {
      if (this.shared_data.is_card_in_hand) {
        
        this.shared_data.is_discard = true;
        this.shared_data.card_in_discard = this.card;
        this.card = this.shared_data.card_in_hand
        this.shared_data.is_card_in_hand = 0;
        console.log(this.shared_data.card_in_discard)
      } else {
        // S'il n'y a pas de carte dans la main, voir la carte
        
      }
      this.show = true;
    },
  },
};
</script>
