<template>
  <h1 class="title">Skyjo</h1>
  <div class="start_container">
    <h2>Bienvenue !</h2>
    <div class="start_form">
      <p>Nom du premier joueur :</p>
      <input
        class="start_input"
        id="first_player"
        autocomplete="off"
        v-model="first_player"
      />
      <p>Nom du deuxième joueur :</p>
      <input
        class="start_input"
        id="second_player"
        autocomplete="off"
        v-model="second_player"
      />
    </div>
    <div v-if="error_message.length > 0" class="error_message">
      {{ error_message }}
    </div>
    <button @click="StartGame()">Démarrer</button>
  </div>
</template>

<script>
import store from "../store/store.js";

export default {
  name: "StartScreen",
  data: function () {
    return {
      error_message: "",
      shared_data: store.data,
      first_player: "",
      second_player: "",
    };
  },
  methods: {
    StartGame: function () {
      if (this.first_player.length > 0 && this.second_player.length > 0) {
        this.shared_data.player_data.push({
          name: this.first_player,
          board: [],
          nb_visible: 0,
          nb_enable: 12,
        });
        this.shared_data.player_data.push({
          name: this.second_player,
          board: [],
          nb_visible: 0,
          nb_enable: 12,
        });
        this.shared_data.is_game_started = 1;
      } else {
        this.error_message = "Nom(s) incorrects..";
      }
    },
  },
  components: {},
};
</script>
