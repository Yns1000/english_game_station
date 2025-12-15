<script setup>
import { ref, reactive } from 'vue';
import HomeScreen from './components/HomeScreen.vue';
import RulesScreen from "@/components/RulesScreen.vue";
import SetupScreen from './components/SetupScreen.vue';
import GameScreen from './components/GameScreen.vue';
import EndScreen from './components/EndScreen.vue';

const gameState = ref('home');

const teams = reactive({
  team1: { name: 'Team 1', score: 0 },
  team2: { name: 'Team 2', score: 0 }
});

const startGame = () => {
  if (!teams.team1.name) teams.team1.name = "Team 1";
  if (!teams.team2.name) teams.team2.name = "Team 2";
  gameState.value = 'game';
};

const resetGame = () => {
  teams.team1.score = 0;
  teams.team2.score = 0;
  gameState.value = 'home';
};
</script>

<template>
  <div class="app-container">
    <HomeScreen
        v-if="gameState === 'home'"
        @go-to-setup="gameState = 'rules'"
    />

    <RulesScreen
        v-else-if="gameState === 'rules'"
        @back="gameState = 'home'"
        @next="gameState = 'setup'"
    />

    <SetupScreen
        v-else-if="gameState === 'setup'"
        :teams="teams"
        @back="gameState = 'rules'"
        @start-game="startGame"
    />

    <GameScreen
        v-else-if="gameState === 'game'"
        :teams="teams"
        @quit="gameState = 'end'"
    />

    <EndScreen
        v-else-if="gameState === 'end'"
        :teams="teams"
        @restart="resetGame"
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
</style>