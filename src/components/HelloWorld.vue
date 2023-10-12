<template>
  <div>
    <h1>Interface Utilisateur</h1>
    <label for="day">Jour :</label>
    <input v-model="day" type="text" id="day" />
    <label for="month">Mois :</label>
    <input v-model="month" type="text" id="month" />
    <label for="year">Année :</label>
    <input v-model="year" type="text" id="year" />
    <button @click="generateUI">Générer UI</button>
    <div v-if="uiData">
      <h2>Résultats</h2>
      <p>Jour : {{ uiData.day }}</p>
      <p>Mois : {{ uiData.month }}</p>
      <p>Année : {{ uiData.year }}</p>
      <h2>Date sélectionnée : {{ uiData.selectedDate }}</h2>
      <!-- Affichez d'autres données ici -->
    </div>
    <div v-if="showCalendar">
      <label>Sélectionnez une date :</label>
      <DatePicker :value="selectedDate" format="yyyy-MM-dd" @input="updateSelectedDate"></DatePicker>
    </div>
  </div>
</template>

<script>
import DatePicker from 'vue3-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

export default {
  components: {
    DatePicker,
  },
  data() {
    return {
      day: '',
      month: '',
      year: '',
      uiData: null,
      selectedDate: '', // Initialisez selectedDate à vide
      showCalendar: false,
    };
  },
  methods: {
    async generateUI() {
      try {
        const response = await fetch('http://localhost:3000/api/generate-ui', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            day: this.day,
            month: this.month,
            year: this.year,
          }),
        });
        if (response.ok) {
          this.uiData = await response.json();
          this.selectedDate = `${this.uiData.year}-${this.uiData.month.padStart(2, '0')}-${this.uiData.day.padStart(2, '0')}`;
          this.showCalendar = true;
        }
      } catch (error) {
        console.error('Une erreur s\'est produite :', error);
      }
    },
    updateSelectedDate(newDate) {
      this.selectedDate = newDate;
    },
  },
};
</script>

<style scoped>
/* Ajoutez des styles CSS ici si nécessaire */
</style>