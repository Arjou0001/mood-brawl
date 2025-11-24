<template>
  <div class="mood-selector">
    <h2>Choisis ton mood pour Brawl Stars :</h2>

    <div class="buttons">
      <button
        v-for="mood in moods"
        :key="mood"
        :class="{ active: mood === selectedMood }"
        @click="selectMood(mood)"
      >
        {{ mood }}
      </button>
    </div>

    <div v-if="selectedBrawler" class="brawler-display">
      <h3>Brawler suggéré : {{ selectedBrawler.name }}</h3>
      <img :src="selectedBrawler.img" :alt="selectedBrawler.name" class="brawler-img" />
      <audio :src="selectedBrawler.audio" autoplay></audio>
    </div>

    <p v-else-if="selectedMood">Mood sélectionné : <strong>{{ selectedMood }}</strong></p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const moods = ['Aggressif', 'Défensif', 'Stratégique', 'Fun', 'Random']

const brawlers = [
  { name: 'Shelly', mood: 'Aggressif', img: '/images/shelly.png', audio: '/audio/shelly.mp3' },
  { name: 'Nita', mood: 'Défensif', img: '/images/nita.png', audio: '/audio/nita.mp3' },
  { name: 'Colt', mood: 'Stratégique', img: '/images/colt.png', audio: '/audio/colt.mp3' },
  { name: 'Poco', mood: 'Fun', img: '/images/poco.png', audio: '/audio/poco.mp3' },
  { name: 'Jessie', mood: 'Random', img: '/images/jessie.png', audio: '/audio/jessie.mp3' }
]

const selectedMood = ref(null)
const selectedBrawler = ref(null)

function selectMood(mood) {
  selectedMood.value = mood

  const matchingBrawlers = brawlers.filter(b => b.mood === mood)
  if (matchingBrawlers.length > 0) {
    const randomIndex = Math.floor(Math.random() * matchingBrawlers.length)
    selectedBrawler.value = matchingBrawlers[randomIndex]
  } else {
    selectedBrawler.value = null
  }
}
</script>

<style scoped>
.mood-selector {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  padding: 20px;
  text-align: center;
  background: linear-gradient(135deg, #ffecd2, #fcb69f);
  border-radius: 16px;
  max-width: 450px;
  margin: 20px auto;
  box-shadow: 0 8px 20px rgba(0,0,0,0.2);
}

h2 {
  font-size: 1.6rem;
  margin-bottom: 15px;
  color: #333;
}

.buttons {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

.buttons button {
  flex: 1 1 40%;
  max-width: 150px;
  padding: 12px 20px;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  background: linear-gradient(135deg, #6a11cb, #2575fc);
  color: white;
  font-weight: bold;
  transition: transform 0.2s, box-shadow 0.2s;
}

.buttons button:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

.buttons button.active {
  background: linear-gradient(135deg, #11998e, #38ef7d);
}

.brawler-display {
  margin-top: 25px;
  text-align: center;
  animation: fadeIn 0.5s ease-in-out;
}

.brawler-img {
  width: 120px;
  height: 120px;
  object-fit: contain;
  margin-top: 15px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.3);
}

p {
  font-size: 1.1rem;
  color: #444;
  margin-top: 15px;
}

/* Animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px);}
  to { opacity: 1; transform: translateY(0);}
}

/* Responsive */
@media (max-width: 480px) {
  .buttons button {
    flex: 1 1 100%;
  }

  .brawler-img {
    width: 100px;
    height: 100px;
  }
}
</style>
