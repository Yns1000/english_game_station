<script setup>
import { computed, onMounted } from 'vue';
import { Trophy, RotateCcw, Medal, Crown, PartyPopper } from 'lucide-vue-next';

const props = defineProps(['teams']);
const emit = defineEmits(['restart']);

const winner = computed(() => {
  if (props.teams.team1.score > props.teams.team2.score) {
    return { ...props.teams.team1, key: 'team1', color: 'blue', label: 'TEAM 1 WINS' };
  } else if (props.teams.team2.score > props.teams.team1.score) {
    return { ...props.teams.team2, key: 'team2', color: 'red', label: 'TEAM 2 WINS' };
  } else {
    return { name: "DRAW", score: props.teams.team1.score, color: 'purple', label: "IT'S A TIE!" };
  }
});

const launchConfetti = () => {
  const colors = ['#f1c40f', '#e74c3c', '#3498db', '#2ecc71', '#9b59b6'];
  const confettiCount = 150;

  for (let i = 0; i < confettiCount; i++) {
    const confetti = document.createElement('div');
    confetti.classList.add('confetti');
    confetti.style.left = Math.random() * 100 + 'vw';
    confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
    confetti.style.animationDuration = (Math.random() * 2 + 2) + 's';
    confetti.style.opacity = Math.random();
    document.body.appendChild(confetti);
    setTimeout(() => confetti.remove(), 4000);
  }
};

onMounted(() => {
  launchConfetti();
});
</script>

<template>
  <div class="end-screen">

    <div class="winner-container" :class="winner.color">
      <div class="glow-bg"></div>

      <div class="winner-header">
        <Crown :size="48" class="crown-icon" />
        <span class="label-text">{{ winner.label }}</span>
        <Crown :size="48" class="crown-icon" />
      </div>

      <div class="trophy-wrapper">
        <Trophy :size="140" stroke-width="1.5" class="main-trophy" />
        <div class="particles"></div>
      </div>

      <h1 class="winner-name">{{ winner.name }}</h1>

      <div class="total-score-display">
        <div class="score-pill">
          <span class="score-label">FINAL SCORE</span>
          <span class="score-number">{{ winner.score }}</span>
        </div>
      </div>
    </div>

    <div class="match-recap">
      <div class="recap-card blue" :class="{ 'loser': winner.color === 'red' }">
        <div class="team-initial">1</div>
        <div class="recap-info">
          <span class="r-name">{{ teams.team1.name }}</span>
          <span class="r-score">{{ teams.team1.score }} PTS</span>
        </div>
        <Medal v-if="winner.color === 'blue'" :size="32" class="medal" />
      </div>

      <div class="vs-badge">VS</div>

      <div class="recap-card red" :class="{ 'loser': winner.color === 'blue' }">
        <div class="team-initial">2</div>
        <div class="recap-info">
          <span class="r-name">{{ teams.team2.name }}</span>
          <span class="r-score">{{ teams.team2.score }} PTS</span>
        </div>
        <Medal v-if="winner.color === 'red'" :size="32" class="medal" />
      </div>
    </div>

    <button @click="emit('restart')" class="restart-btn">
      <RotateCcw :size="24" stroke-width="2.5" />
      <span>START NEW GAME</span>
    </button>

  </div>
</template>

<style>
.confetti {
  position: fixed; top: -10px; width: 12px; height: 12px;
  z-index: 9999; pointer-events: none;
  animation: fall linear forwards;
  border-radius: 2px;
}
@keyframes fall {
  to { transform: translateY(110vh) rotate(720deg); }
}
</style>

<style scoped>
.end-screen {
  width: 100vw; height: 100vh;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  background-image: linear-gradient(rgba(0,0,0,0.1) 1px, transparent 1px), linear-gradient(90deg, rgba(0,0,0,0.1) 1px, transparent 1px);
  background-size: 50px 50px;
  overflow: hidden;
  position: relative;
}

.winner-container {
  position: relative;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px);
  padding: 40px 80px;
  border-radius: 40px;
  border: 1px solid rgba(255,255,255,0.2);
  text-align: center;
  margin-bottom: 50px;
  animation: popIn 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  display: flex; flex-direction: column; align-items: center;
  box-shadow: 0 20px 50px rgba(0,0,0,0.5);
}

.winner-container.blue { border-color: #3498db; box-shadow: 0 0 60px rgba(52, 152, 219, 0.3), inset 0 0 20px rgba(52, 152, 219, 0.2); }
.winner-container.red { border-color: #e74c3c; box-shadow: 0 0 60px rgba(231, 76, 60, 0.3), inset 0 0 20px rgba(231, 76, 60, 0.2); }
.winner-container.purple { border-color: #9b59b6; box-shadow: 0 0 60px rgba(155, 89, 182, 0.3); }

.glow-bg {
  position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
  width: 300px; height: 300px;
  border-radius: 50%;
  filter: blur(80px);
  z-index: -1;
  opacity: 0.5;
}
.blue .glow-bg { background: #3498db; }
.red .glow-bg { background: #e74c3c; }

.winner-header {
  display: flex; align-items: center; gap: 15px;
  margin-bottom: 10px;
  color: #f1c40f;
}
.label-text {
  font-family: Arial, sans-serif; letter-spacing: 5px; font-weight: bold; font-size: 1.2rem;
  text-transform: uppercase; color: rgba(255,255,255,0.8);
}
.crown-icon { animation: float 2s infinite ease-in-out; }

.trophy-wrapper {
  margin: 10px 0;
  color: #f1c40f;
  filter: drop-shadow(0 0 15px rgba(241, 196, 15, 0.6));
  animation: shine 3s infinite;
}

.winner-name {
  font-family: 'Anton', sans-serif; font-size: 6rem; margin: 0; line-height: 1;
  text-transform: uppercase;
  background: linear-gradient(to bottom, #ffffff, #bdc3c7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  filter: drop-shadow(0 5px 0 rgba(0,0,0,0.5));
}

.score-pill {
  margin-top: 20px;
  background: rgba(0,0,0,0.6);
  padding: 10px 30px;
  border-radius: 50px;
  border: 2px solid rgba(255,255,255,0.1);
  display: flex; flex-direction: column;
}
.score-label { font-size: 0.8rem; color: #aaa; letter-spacing: 2px; }
.score-number { font-family: 'Anton'; font-size: 3rem; color: #f1c40f; line-height: 1; }

.match-recap {
  display: flex; align-items: center; gap: 20px; margin-bottom: 40px;
}

.recap-card {
  display: flex; align-items: center; gap: 15px;
  background: rgba(0,0,0,0.4); border: 2px solid rgba(255,255,255,0.1);
  padding: 10px 25px; border-radius: 15px;
  min-width: 200px;
  transition: all 0.3s;
}
.recap-card.blue { border-left: 5px solid #3498db; }
.recap-card.red { border-left: 5px solid #e74c3c; }

.recap-card.loser { opacity: 0.5; transform: scale(0.95); filter: grayscale(0.5); }

.team-initial {
  background: rgba(255,255,255,0.1); width: 40px; height: 40px;
  display: flex; align-items: center; justify-content: center;
  border-radius: 50%; font-family: 'Anton'; font-size: 1.5rem;
}

.recap-info { display: flex; flex-direction: column; }
.r-name { font-weight: bold; font-size: 0.9rem; color: #ccc; }
.r-score { font-family: 'Anton'; font-size: 1.8rem; line-height: 1; }
.medal { color: #f1c40f; }

.vs-badge {
  font-family: 'Anton'; font-size: 1.5rem; color: rgba(255,255,255,0.3); font-style: italic;
}

.restart-btn {
  background: #2ecc71; color: white;
  font-family: 'Anton', sans-serif; font-size: 1.5rem; letter-spacing: 1px;
  padding: 15px 50px; border-radius: 50px; border: none; cursor: pointer;
  transition: all 0.2s;
  display: flex; align-items: center; gap: 12px;
  box-shadow: 0 10px 20px rgba(46, 204, 113, 0.3);
}
.restart-btn:hover {
  background: #27ae60; transform: translateY(-3px);
  box-shadow: 0 15px 30px rgba(46, 204, 113, 0.5);
}
.restart-btn:active { transform: translateY(0); }

@keyframes popIn { from { transform: scale(0.5); opacity: 0; } to { transform: scale(1); opacity: 1; } }
@keyframes float { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-5px); } }
@keyframes shine { 0%, 100% { filter: drop-shadow(0 0 15px rgba(241, 196, 15, 0.6)); } 50% { filter: drop-shadow(0 0 30px rgba(241, 196, 15, 1)); } }
</style>