<template>
  <div id="app">
    <welcome-screen v-if="showWelcomeScreen" @welcomeEnd="welcomeScreenEnd"></welcome-screen>
    <div v-else>
      <search-bar @select-guest="handleSelectGuest"></search-bar>
      <p v-if="selectedGuest" class="guest-message">
        Bonjour <strong>{{ selectedGuest.name }}</strong>, vous êtes assis à la place 
        <strong>{{ selectedGuest.seatNumber }}</strong>, votre place est indiquée en <strong>rouge</strong> 
        sur le plan ci-dessous.
      </p>
      <table-plan :highlightedSeat="highlightedSeat"></table-plan>
    </div>
  </div>
</template>

<script>
import WelcomeScreen from './components/WelcomeScreen.vue';
import SearchBar from './components/SearchBar.vue';
import TablePlan from './components/TablePlan.vue';

export default {
  components: {
    WelcomeScreen,
    SearchBar,
    TablePlan
  },
  data() {
    return {
      showWelcomeScreen: true, // Gère l'affichage de l'écran d'accueil
      highlightedSeat: null,
      selectedGuest: null
    };
  },
  methods: {
    welcomeScreenEnd() {
      this.showWelcomeScreen = false; // Masque l'écran d'accueil
    },
    handleSelectGuest(guest) {
      this.highlightedSeat = guest.seatNumber;
      this.selectedGuest = guest;
    }
  }
};
</script>

<style>
  /* Vos styles globaux, y compris pour body, etc. */
  body {
    font-family: sans-serif;
  }

  .guest-message {
    text-align: center;
    margin-top: 20px;
  }
</style>
