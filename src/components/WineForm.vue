<template>
  <div class="wine-form">
    <h2>Wine Prediction</h2>
    <form @submit.prevent="submitForm">
      <div class="form-field">
        <label for="alcohol">Alcohol:</label>
        <input type="number" id="alcohol" v-model="formData.alcohol" />
      </div>

      <div class="form-field">
        <label for="malic_acid">Malic Acid:</label>
        <input type="number" id="malic_acid" v-model="formData.malic_acid" />
      </div>

      <div class="form-field">
        <label for="ash">Ash:</label>
        <input type="number" id="ash" v-model="formData.ash" />
      </div>

      <div class="form-field">
        <label for="alcalinity_of_ash">Alcalinity of Ash:</label>
        <input type="number" id="alcalinity_of_ash" v-model="formData.alcalinity_of_ash" />
      </div>

      <div class="form-field">
        <label for="magnesium">Magnesium:</label>
        <input type="number" id="magnesium" v-model="formData.magnesium" />
      </div>

      <div class="form-field">
        <label for="total_phenols">Total Phenols:</label>
        <input type="number" id="total_phenols" v-model="formData.total_phenols" />
      </div>

      <div class="form-field">
        <label for="flavanoids">Flavanoids:</label>
        <input type="number" id="flavanoids" v-model="formData.flavanoids" />
      </div>

      <div class="form-field">
        <label for="nonflavanoid_phenols">Nonflavanoid Phenols:</label>
        <input type="number" id="nonflavanoid_phenols" v-model="formData.nonflavanoid_phenols" />
      </div>

      <div class="form-field">
        <label for="proanthocyanins">Proanthocyanins:</label>
        <input type="number" id="proanthocyanins" v-model="formData.proanthocyanins" />
      </div>

      <div class="form-field">
        <label for="color_intensity">Color Intensity:</label>
        <input type="number" id="color_intensity" v-model="formData.color_intensity" />
      </div>

      <div class="form-field">
        <label for="hue">Hue:</label>
        <input type="number" id="hue" v-model="formData.hue" />
      </div>

      <div class="form-field">
        <label for="proline">Proline:</label>
        <input type="number" id="proline" v-model="formData.proline" />
      </div>

      <button type="submit">Predict</button>
    </form>
    <div v-if="predictedClass">
      Predicted Class: {{ predictedClass }}
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

const formData = ref({
  alcohol: 0,
  malic_acid: 0,
  ash: 0,
  alcalinity_of_ash: 0,
  magnesium: 0,
  total_phenols: 0,
  flavanoids: 0,
  nonflavanoid_phenols: 0,
  proanthocyanins: 0,
  color_intensity: 0,
  hue: 0,
  proline: 0
})

const predictedClass = ref(null)

const submitForm = async () => {
  try {
    const response = await axios.post('http://127.0.0.1:5000/prediction', formData.value)
    predictedClass.value = response.data.predicted_class
  } catch (error) {
    console.error('Error fetching prediction:', error)
  }
}
</script>

<style scoped>
.wine-form {
  background-color: #4a1a2c; /* Cor de vinho tinto */
  color: #fff;
  padding: 20px;
  border-radius: 8px;
  font-family: 'Merriweather', serif; /* Fonte elegante */
}

.wine-form h2 {
  color: #dab3c8; /* Cor suave de vinho */
}

.wine-form button {
  background-color: #752436; /* Cor de vinho escuro */
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.wine-form button:hover {
  background-color: #9a2d50;
}

.form-field {
  margin-bottom: 10px;
}

.form-field label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.form-field input {
  width: 100%;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}
</style>
