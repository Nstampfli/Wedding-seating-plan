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
      let numGuests = this.guests.length;
      let numTables = Math.ceil(numGuests / 6); // Chaque table peut contenir jusqu'à 6 invités

      for (let i = 0; i < numTables; i++) {
        let startIdx = i * 6;
        let endIdx = Math.min(startIdx + 6, numGuests);
        let seats = this.guests.slice(startIdx, endIdx);
        tables.push({ id: i + 1, seats });
      }

      tables.splice(-1, 0, { id: 100000, seats: [] });

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
  display: grid;

  grid-template-columns: repeat(3, 1fr);
  /* Trois colonnes de largeur égale */
  grid-template-rows: repeat(3, 1fr);
  /* Trois lignes de hauteur égale */
  max-width: 1000px;
  margin: 40px auto;
  gap: 40px 30px;
  border: 1px solid;
  border-radius: 10px;
  padding: 50px 8px;

}

.table {
  border: 2px solid #666;
  border-radius: 50%;
  position: relative;
  width: 100%;
  height: 0;
  padding-bottom: 100%;
  /* La hauteur est égale à la largeur */
  display: flex;
  justify-content: center;
  align-items: center;
}

.table:nth-last-child(2) {
  border: none;
}

.table-number {
  position: absolute;
  bottom: 5px;
  right: 5px;
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
  background-color: red;
  /* Fond rouge pour le siège sélectionné */
  color: white;
  /* Texte blanc pour une meilleure lisibilité */
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
