<template>
  <div class="game_container">
 
    <div class="cards_container">
      <Card
        v-for="card in cards"
        v-bind:key="card.id"
        v-bind:cardnb="card.cardnb"
        visible=false
      />
    </div>
    <div class="drawer_container">
      <Drawer />
      <DiscardPile />
    </div>
    
    <div v-if="shared_data.is_card_in_hand" class="card_in_hand">
      <Card
        v-bind:cardnb="shared_data.card_in_hand.cardnb"
        visible=true
      />
    </div>
  </div>
</template>

<script>
import Card from "./components/Card.vue";
import Drawer from "./components/Drawer.vue";
import DiscardPile from "./components/DiscardPile.vue";

import store from "./store/store.js";



export default {
  name: "App",
  data: function () {
    return {
      
      shared_data : store.data,
      cards: [
        { id: 0, cardnb: 0 },
        { id: 1, cardnb: 1 },
        { id: 2, cardnb: 2 },
        { id: 3, cardnb: 3 },
        { id: 4, cardnb: 4 },
        { id: 5, cardnb: 5 },
        { id: 6, cardnb: 10 },
        { id: 7, cardnb: -2 },
        { id: 8, cardnb: 0 },
        { id: 9, cardnb: 0 },
        { id: 10, cardnb: 0 },
        { id: 11, cardnb: 0 },
      ],
    };
  },
  methods: {},
  components: {
    Card,
    Drawer,
    DiscardPile,
  },
};
</script>
