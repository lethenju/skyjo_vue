<template>
  <div v-if="shared_data.is_game_started" class="game_container">
    <PlayerBoard
      v-bind:class="{ player_board_active: shared_data.turn == 0 }"
      v-bind:player_number="0"
      v-on:update="updateGame()"
    />

    <MiddleBoard />

    <PlayerBoard
      v-bind:class="{ player_board_active: shared_data.turn == 1 }"
      v-bind:player_number="1"
      v-on:update="updateGame()"
    />
  </div>
  <div v-else>
    <StartScreen />
  </div>
</template>

<script>
import StartScreen from "./components/StartScreen.vue";
import PlayerBoard from "./components/PlayerBoard.vue";
import MiddleBoard from "./components/MiddleBoard.vue";

import store from "./store/store.js";

export default {
  name: "App",
  data: function () {
    return {
      shared_data: store.data,
    };
  },
  methods: {
    updateGame: function () {
      // First part : discovering 2 cards
      console.log("UpdateGame ");
      console.log(this.shared_data);
      switch (this.shared_data.game_state) {
        case 0:
          for (const player of this.shared_data.player_data) {
            if (player.nb_visible != 2) {
              return;
            }
          }
          // All players have 2 visible cards
          this.shared_data.game_state = 1;
          break;
        case 1:
          for (const player of this.shared_data.player_data) {
            player.nb_visible = 0
            for (let card of player.board) {
              if (card.visible)
              {
                player.nb_visible++;
              }
            }
            
            if (player.nb_visible == player.nb_enable) {
              // All the cards of this player are visible !
              this.shared_data.game_state = 2;
              break;
            }
          }
          break;
        case 2:
          // We show the cards of everybody
          for (let player of this.shared_data.player_data) {
            for (let card of player.board) {
              card.visible = true;
            }
          }

          break;
      }
    },
  },
  components: {
    StartScreen,
    PlayerBoard,
    MiddleBoard,
  },
};
</script>
