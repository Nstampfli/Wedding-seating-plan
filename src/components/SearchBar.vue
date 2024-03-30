<template>
  <div class="search-bar">
    <h2 class="title">Trouvez votre place : Entrez votre nom parmi les {{ totalInvites }} invités</h2>
    <input type="text" v-model="searchQuery" @input="onInput" placeholder="Recherchez un invité..." />
    <ul v-if="suggestions.length" class="suggestions-list">
      <li v-for="suggestion in suggestions" :key="suggestion.seatNumber" @click="onSelect(suggestion)">
        {{ suggestion.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import invites from '../assets/invites_aleatoires.json';

export default {
  data() {
    return {
      searchQuery: '',
      suggestions: []
    };
  },
  computed: {
    totalInvites() {
      return invites.length;
    }
  },
  methods: {
    onInput() {
      if (this.searchQuery) {
        this.suggestions = invites.filter(guest =>
          guest.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      } else {
        this.suggestions = [];
      }
    },
    onSelect(suggestion) {
      this.$emit('select-guest', suggestion);
      this.searchQuery = suggestion.name; // Conservez le nom dans l'input
      this.suggestions = []; // Effacez les suggestions
    }
  }
};
</script>

<style>
.title {
  text-align: center;
  margin-bottom: 20px;
}

.search-bar {
  position: relative;
  padding: 10px;
  display: flex;
  padding-bottom: 0;
  flex-direction: column;
  align-items: center;
  /* Centrage vertical */
}

.search-bar input {
  width: 100%;
  max-width: 500px;
  padding: 8px;
  margin-bottom: 0;
  /* Retirez la marge du bas de l'input */
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.suggestions-list {
  position: absolute;
  list-style-type: none;
  padding: 0;
  margin: 0;
  /* Supprimez toute marge par défaut */
  border: 1px solid #ccc;
  border-top: none;
  border-radius: 0 0 4px 4px;
  max-height: 200px;
  overflow-y: auto;
  background-color: white;
  width: calc(100% - 20px);
  max-width: 500px;
  z-index: 10;
  top: calc(100% - 1px);
  /* Positionné juste en dessous de l'input */
}

.suggestions-list li {
  padding: 8px;
  border-top: 1px solid #eee;
  cursor: pointer;
}

.suggestions-list li:hover {
  background-color: #f0f0f0;
}
</style>