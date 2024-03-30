<template>
  <div class="table-plan">
    <div v-for="table in formattedTables" :key="table.id" class="table">
      <div v-for="seat in table.seats" :key="seat.seatNumber" class="seat"
        :class="{ 'highlighted': seat.seatNumber === highlightedSeat }">
        {{ seat.seatNumber }}
      </div>
    </div>
  </div>
</template>

<script>
import invites from '../assets/invites_aleatoires.json';

export default {
  props: ['highlightedSeat'],

  data() {
    return {
      guests: this.shuffleArray(invites)
    };
  },
  computed: {
    formattedTables() {
      let tables = [];
      for (let i = 0; i < this.guests.length; i += 6) { // Maintenant 6 invités par table
        tables.push({
          id: tables.length + 1,
          seats: this.guests.slice(i, i + 6)
        });
      }
      return tables;
    }
  },
  methods: {
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]]; // Echange des éléments
      }
      return array;
    }
  }
};
</script>

<style>
.table-plan {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  max-width: 1000px;
  margin: 40px auto;
  gap: 20px;
}

.table {
  border: 2px solid #666;
  border-radius: 50%;
  position: relative;
  width: 250px;
  height: 250px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.seat {
  position: absolute;
  min-width: 30px;
  padding: 5px;
  margin: 2px;
  border: 1px solid #444;
  border-radius: 50%;
  background-color: #eef;
  text-align: center;
  font-size: 0.8em;
}

.highlighted {
  background-color: red; /* Fond rouge pour le siège sélectionné */
  color: white; /* Texte blanc pour une meilleure lisibilité */
}

/* Positionnement des sièges autour de la table */
.seat:nth-child(1) {
  top: 0;
  left: 50%;
  transform: translate(-50%, -50%);
}

.seat:nth-child(2) {
  top: 25%;
  right: 0;
  transform: translateY(-50%);
}

.seat:nth-child(3) {
  bottom: 0;
  left: 50%;
  transform: translate(-50%, 50%);
}

.seat:nth-child(4) {
  top: 25%;
  left: 0;
  transform: translateY(-50%);
}

.seat:nth-child(5) {
  bottom: 25%;
  left: 0;
  transform: translateY(50%);
}

.seat:nth-child(6) {
  bottom: 25%;
  right: 0;
  transform: translateY(50%);
}
</style>
