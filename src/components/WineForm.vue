<template>
  <section class="wine-info">
    <img src="../assets/vinho.png" alt="Vineyard" class="vineyard-image"/>
    <div class="units-description">
      <h3>Entendendo as Unidades</h3>
      <ul>
        <li><strong>Alcohol:</strong> Teor alcoólico (% vol).</li>
        <li><strong>Malic Acid:</strong> Ácido málico (g/L).</li>
        <li><strong>Ash:</strong> Cinzas (g/L).</li>
        <li><strong>Alcalinity of Ash:</strong> Alcalinidade das cinzas (g/L).</li>
        <li><strong>Magnesium:</strong> Magnésio (mg/L).</li>
        <li><strong>Total Phenols:</strong> Fenóis totais (mg/L).</li>
        <li><strong>Flavanoids:</strong> Flavonoides (mg/L).</li>
        <li><strong>Nonflavanoid Phenols:</strong> Fenóis não flavonoides (mg/L).</li>
        <li><strong>Proanthocyanins:</strong> Proantocianidinas (mg/L).</li>
        <li><strong>Color Intensity:</strong> Intensidade da cor (mg/L).</li>
        <li><strong>Hue:</strong> Matiz (mg/L).</li>
        <li><strong>Proline:</strong> Prolina (mg/L).</li>
      </ul>
    </div>
  </section>
  <div class="wine-form">
    <h2>Wine Prediction</h2>
    <form @submit.prevent="submitForm" class="form-grid">
      <div class="form-field">
        <label for="alcohol">Alcohol:</label>
        <input type="number" id="alcohol" v-model="formData.alcohol" step="0.01" />
      </div>

      <div class="form-field">
        <label for="malic_acid">Malic Acid:</label>
        <input type="number" id="malic_acid" v-model="formData.malic_acid" step="0.01"/>
      </div>

      <div class="form-field">
        <label for="ash">Ash:</label>
        <input type="number" id="ash" v-model="formData.ash" step="0.01" />
      </div>

      <div class="form-field">
        <label for="alcalinity_of_ash">Alcalinity of Ash:</label>
        <input type="number" id="alcalinity_of_ash" v-model="formData.alcalinity_of_ash" step="0.01" />
      </div>

      <div class="form-field">
        <label for="magnesium">Magnesium:</label>
        <input type="number" id="magnesium" v-model="formData.magnesium" step="0.01"/>
      </div>

      <div class="form-field">
        <label for="total_phenols">Total Phenols:</label>
        <input type="number" id="total_phenols" v-model="formData.total_phenols" step="0.01"/>
      </div>

      <div class="form-field">
        <label for="flavanoids">Flavanoids:</label>
        <input type="number" id="flavanoids" v-model="formData.flavanoids" step="0.01"/>
      </div>

      <div class="form-field">
        <label for="nonflavanoid_phenols">Nonflavanoid Phenols:</label>
        <input type="number" id="nonflavanoid_phenols" v-model="formData.nonflavanoid_phenols" step="0.01"/>
      </div>

      <div class="form-field">
        <label for="proanthocyanins">Proanthocyanins:</label>
        <input type="number" id="proanthocyanins" v-model="formData.proanthocyanins" step="0.01"/>
      </div>

      <div class="form-field">
        <label for="color_intensity">Color Intensity:</label>
        <input type="number" id="color_intensity" v-model="formData.color_intensity" step="0.01"/>
      </div>

      <div class="form-field">
        <label for="hue">Hue:</label>
        <input type="number" id="hue" v-model="formData.hue" step="0.01"/>
      </div>

      <div class="form-field">
        <label for="proline">Proline:</label>
        <input type="number" id="proline" v-model="formData.proline" step="0.01"/>
      </div>

      <button type="submit">Predict</button>
    </form>
    <div v-if="predictedClass" class="prediction-result">
      <h3>Resultado da Previsão:</h3>
      <div v-if="predictedClass === 'class_0'" class="class-info">
        <h4>Classe 0</h4>
        <ul>
          <li>Tipo de Uva: "Barbera".</li>
          <li>Vinhos com maior teor de álcool e flavonoides.</li>
          <li>Perfil de Sabor: Notas de frutas escuras.</li>
        </ul>
      </div>
      <div v-if="predictedClass === 'class_1'" class="class-info">
        <h4>Classe 1</h4>
        <ul>
          <li>Tipo de Uva: "Grignolino".</li>
          <li>Vinhos com teor mais baixo de álcool e menos flavonoides.</li>
          <li>Perfil de Sabor: Notas florais e frutas mais claras.</li>
        </ul>
      </div>
      <div v-if="predictedClass === 'class_2'" class="class-info">
        <h4>Classe 2</h4>
        <ul>
          <li>Tipo de Uva: "Barolo" ou "Nebbiolo".</li>
          <li>Alto teor de ácido málico e fenóis totais.</li>
          <li>Perfil de Sabor: Notas de frutas secas e especiarias.</li>
        </ul>
      </div>
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

.wine-info {
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: 20px;
}

.vineyard-image {
  max-width: 50%;
  height: auto;
  border-radius: 8px;
}

.units-description {
  max-width: 50%;
}

.units-description h3 {
  margin-bottom: 10px;
}

.units-description ul {
  list-style: none;
  padding: 0;
}

.units-description li {
  margin-bottom: 5px;
}
.wine-form {
  background-color: #4a1a2c;
  color: #fff;
  padding: 20px;
  border-radius: 8px;
  font-family: 'Merriweather', serif;
}

.wine-form h2 {
  color: #dab3c8;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  margin-bottom: 20px;
}

.form-field {
  margin-bottom: 0;
  display: flex;
  flex-direction: column;
}


.predict-button {
  grid-column: 1 / -1;
  background-color: #752436;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
  justify-self: center; /* Centralizar o botão */
}

.predict-button:hover {
  background-color: #9a2d50;
}

.prediction-result {
  margin-top: 20px;
}

@media (max-width: 768px) {
  .form-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .predict-button {
    grid-column: 1 / -1;
  }
}

.prediction-result h3 {
  margin-bottom: 15px;
}

.class-info {
  border-radius: 8px;
  padding: 15px;
  margin-top: 10px;
}

.class-info h4 {
  margin-bottom: 10px;
}

.class-info ul {
  list-style-type: none;
  padding: 0;
}

.class-info li {
  margin-bottom: 5px;
}
</style>
