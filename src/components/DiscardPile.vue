<template>
  <div @click="putCardInHand()">
    <Card   
        :key="shared_data.card_in_discard.cardnb"
        v-if="shared_data.is_discard"         
        v-bind:cardnb="shared_data.card_in_discard.cardnb"
        v-bind:visible=true
        v-bind:enable=true
        v-bind:position=-2
        />
  </div>
</template>

<script>

import Card from "./Card.vue";
import store from "../store/store.js";
export default {
  name: 'DiscardPile',
  props: {
   
  },
  data() {
   return {
       shared_data: store.data,
   }
  },
  methods: {
      putCardInHand: function () {
        if (this.shared_data.is_discard && !this.shared_data.is_card_in_hand)
        {
            this.shared_data.card_in_hand = this.shared_data.card_in_discard;
            this.shared_data.is_card_in_hand = true;
            this.shared_data.is_discard = false;
        }

    }
  },
  components: {
    Card,
  },
}
</script>
