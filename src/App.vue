<script setup>
import { ref, reactive, watch } from 'vue';
import { Volume2, VolumeX } from 'lucide-vue-next';
import HomeScreen from './components/HomeScreen.vue';
import RulesScreen from "@/components/RulesScreen.vue";
import SetupScreen from './components/SetupScreen.vue';
import GameScreen from './components/GameScreen.vue';
import EndScreen from './components/EndScreen.vue';
import HotSeatGame from './components/HotSeatGame.vue';
import MotusGame from './components/MotusGame.vue';
import MillionaireGame from "@/components/MillionaireGame.vue";
import FinalResultScreen from "@/components/FinalResultScreen.vue";


const gameState = ref('home');
const targetGame = ref('feud');

const savedData = sessionStorage.getItem('ff_teams');
const initialTeams = savedData ? JSON.parse(savedData) : {
  team1: { name: '', score: 0 },
  team2: { name: '', score: 0 }
};

const teams = reactive(initialTeams);

watch(teams, (newVal) => {
  sessionStorage.setItem('ff_teams', JSON.stringify(newVal));
}, { deep: true });

const currentAudio = new Audio();
const isMuted = ref(false);
const currentTrack = ref('');

const musicMap = {
  home: '/sounds/millionaire.mp3',
  result: '/sounds/millionaire.mp3',
  feud: '/sounds/millionaire.mp3',
  hotseat: '/sounds/millionaire.mp3',
  motus: '/sounds/millionaire.mp3',
  millionaire: '/sounds/millionaire.mp3'
};

const playMusic = (key) => {
  const file = musicMap[key];
  if (!file) return;

  if (currentTrack.value === key) return;

  currentTrack.value = key;
  currentAudio.src = file;
  currentAudio.loop = true;
  currentAudio.volume = 0.5;

  if (!isMuted.value) {
    const playPromise = currentAudio.play();
    if (playPromise !== undefined) {
      playPromise.catch(error => {
        console.log("Audio autoplay prevented by browser.");
      });
    }
  }
};

const toggleMute = () => {
  isMuted.value = !isMuted.value;
  if (isMuted.value) currentAudio.pause();
  else currentAudio.play();
};

watch([gameState, targetGame], ([newState, newTarget]) => {
  if (newState === 'home' || newState === 'result') {
    playMusic('home');
  } else {
    playMusic(newTarget);
  }
}, { immediate: true });

const prepareFeud = () => {
  targetGame.value = 'feud';
  if (teams.team1.name && teams.team2.name) {
    gameState.value = 'rules';
  } else {
    gameState.value = 'setup';
  }
};

const prepareHotSeat = () => {
  targetGame.value = 'hotseat';
  if (teams.team1.name) gameState.value = 'rules';
  else gameState.value = 'setup';
};

const prepareMotus = () => {
  targetGame.value = 'motus';
  if (teams.team1.name) gameState.value = 'rules';
  else gameState.value = 'setup';
};

const prepareMillionaire = () => {
  targetGame.value = 'millionaire';
  if (teams.team1.name) gameState.value = 'rules';
  else gameState.value = 'setup';
};

const handleSetupBack = () => {
  gameState.value = 'home';
};

const launchGame = () => {
  if (!teams.team1.name) teams.team1.name = "Team 1";
  if (!teams.team2.name) teams.team2.name = "Team 2";

  if (targetGame.value === 'feud') {
    gameState.value = 'game';
  } else if (targetGame.value === 'hotseat') {
    gameState.value = 'hotseat';
  } else if (targetGame.value === 'motus') gameState.value = 'motus';
  else gameState.value = 'millionaire';

};

const backToMenu = () => {
  gameState.value = 'home';
};

const showFinalResults = () => {
  gameState.value = 'result';
};

const hardReset = () => {
  if(confirm("Are you sure? This will delete team names, scores, and used words.")) {
    teams.team1.name = '';
    teams.team1.score = 0;
    teams.team2.name = '';
    teams.team2.score = 0;

    sessionStorage.removeItem('ff_teams');
    sessionStorage.removeItem('motus_history');
    sessionStorage.removeItem('hotseat_history');

    gameState.value = 'home';
  }
};
</script>

<template>
  <div class="app-container">

    <button class="volume-btn" @click="toggleMute" :title="isMuted ? 'Unmute' : 'Mute'">
      <VolumeX v-if="isMuted" :size="24" />
      <Volume2 v-else :size="24" />
    </button>

    <HomeScreen
        v-if="gameState === 'home'"
        :teams="teams"
        @go-to-setup="prepareFeud"
        @go-to-hotseat="prepareHotSeat"
        @go-to-motus="prepareMotus"
        @go-to-millionaire="prepareMillionaire"
        @finish-session="showFinalResults"
        @reset-data="hardReset"
    />

    <RulesScreen
        v-else-if="gameState === 'rules'"
        :activeGame="targetGame"
        @back="gameState = 'home'"
        @next="launchGame"
    />

    <SetupScreen
        v-else-if="gameState === 'setup'"
        :teams="teams"
        @back="handleSetupBack"
        @start-game="launchGame"
    />

    <GameScreen
        v-else-if="gameState === 'game'"
        :teams="teams"
        @quit="gameState = 'end'"
    />

    <HotSeatGame
        v-else-if="gameState === 'hotseat'"
        :teams="teams"
        @quit="backToMenu"
    />

    <MotusGame
        v-else-if="gameState === 'motus'"
        :teams="teams"
        @quit="backToMenu"
    />

    <MillionaireGame
        v-else-if="gameState === 'millionaire'"
        :teams="teams"
        @quit="backToMenu"
    />

    <EndScreen
        v-else-if="gameState === 'end'"
        :teams="teams"
        @restart="backToMenu"
    />

    <FinalResultScreen
        v-else-if="gameState === 'result'"
        :teams="teams"
        @back-home="backToMenu"
    />

  </div>
</template>

<style>
body { margin: 0; overflow: hidden; }
.app-container {
  width: 100vw; height: 100vh;
  background: radial-gradient(circle at center, #005f9e 0%, #000046 100%);
  display: flex; justify-content: center; align-items: center;
}

.volume-btn {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: rgba(0, 0, 0, 0.6);
  border: 2px solid rgba(255, 255, 255, 0.2);
  color: white;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 9999;
  transition: all 0.3s;
}

.volume-btn:hover {
  background: white;
  color: black;
  transform: scale(1.1);
}
</style>