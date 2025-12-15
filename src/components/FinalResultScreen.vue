<script setup>
import { computed, onMounted } from 'vue';
import { Trophy, Frown, Home, Crown } from 'lucide-vue-next';

const props = defineProps(['teams']);
const emit = defineEmits(['back-home']);

const result = computed(() => {
  const t1 = props.teams.team1;
  const t2 = props.teams.team2;

  if (t1.score > t2.score) {
    return {
      winner: { ...t1, color: 'blue' },
      loser: { ...t2, color: 'red' },
      isDraw: false
    };
  } else if (t2.score > t1.score) {
    return {
      winner: { ...t2, color: 'red' },
      loser: { ...t1, color: 'blue' },
      isDraw: false
    };
  } else {
    return { isDraw: true };
  }
});

const launchConfetti = () => {
  const colors = ['#f1c40f', '#e74c3c', '#3498db', '#2ecc71', '#ffffff'];
  for (let i = 0; i < 200; i++) {
    const el = document.createElement('div');
    el.classList.add('confetti');
    el.style.left = Math.random() * 100 + 'vw';
    el.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
    el.style.animationDuration = (Math.random() * 2 + 3) + 's';
    el.style.opacity = Math.random();
    document.body.appendChild(el);
    setTimeout(() => el.remove(), 5000);
  }
};

onMounted(() => {
  if (!result.value.isDraw) launchConfetti();
});
</script>

<template>
  <div class="final-screen">

    <h1 class="page-title">SESSION RESULTS</h1>

    <div v-if="result.isDraw" class="draw-card">
      <div class="icons">
        <Crown :size="80" color="#f1c40f" />
        <Crown :size="80" color="#f1c40f" />
      </div>
      <h2>IT'S A DRAW!</h2>
      <div class="draw-score">{{ teams.team1.score }} PTS</div>
    </div>

    <div v-else class="results-container">

      <div class="card winner" :class="result.winner.color">
        <div class="crown-box"><Crown :size="60" fill="currentColor" /></div>
        <div class="avatar-win">
          <Trophy :size="100" stroke-width="1.5" />
        </div>
        <div class="rank">WINNER</div>
        <div class="name">{{ result.winner.name }}</div>
        <div class="score">{{ result.winner.score }} PTS</div>
      </div>

      <div class="card loser" :class="result.loser.color">
        <div class="stamp">LOSER</div>
        <div class="avatar-lose">
          <Frown :size="80" stroke-width="1.5" />
        </div>
        <div class="rank">2ND PLACE</div>
        <div class="name">{{ result.loser.name }}</div>
        <div class="score">{{ result.loser.score }} PTS</div>
      </div>

    </div>

    <button @click="emit('back-home')" class="home-btn">
      <Home :size="24" /> BACK TO MENU
    </button>

  </div>
</template>

<style>
.confetti {
  position: fixed; top: -10px; width: 10px; height: 10px;
  z-index: 9999; pointer-events: none; animation: fall linear forwards;
}
@keyframes fall { to { transform: translateY(110vh) rotate(720deg); } }
</style>

<style scoped>
.final-screen {
  width: 100vw; height: 100vh;
  background: radial-gradient(circle at center, #1a1a1a 0%, #000000 100%);
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  color: white; font-family: 'Arial', sans-serif; overflow: hidden;
}

.page-title {
  font-family: 'Anton'; font-size: 4rem; margin-bottom: 40px; color: #f1c40f;
  text-shadow: 0 5px 15px rgba(0,0,0,0.5); letter-spacing: 5px;
}

.results-container {
  display: flex; gap: 60px; align-items: flex-end; margin-bottom: 50px;
}

.card {
  border-radius: 30px; padding: 40px; text-align: center;
  display: flex; flex-direction: column; align-items: center;
  position: relative; transition: all 0.5s;
}

.card.winner {
  background: linear-gradient(135deg, rgba(255,255,255,0.1), rgba(255,255,255,0.05));
  border: 3px solid #f1c40f; width: 400px; height: 550px;
  box-shadow: 0 0 50px rgba(241, 196, 15, 0.2);
  z-index: 10; transform: scale(1.05);
}
.crown-box { color: #f1c40f; margin-bottom: 20px; animation: bounce 2s infinite; }
.card.winner .rank { font-family: 'Anton'; font-size: 3.5rem; color: #f1c40f; margin: 20px 0 10px; letter-spacing: 3px; }
.card.winner .name { font-size: 2.5rem; font-weight: bold; margin-bottom: 10px; }
.card.winner .score { font-family: 'Anton'; font-size: 4.5rem; color: white; text-shadow: 0 0 20px rgba(255,255,255,0.5); }
.avatar-win { color: #f1c40f; filter: drop-shadow(0 0 20px rgba(241,196,15,0.5)); }

.card.loser {
  background: rgba(0,0,0,0.5); border: 2px solid #333;
  width: 320px; height: 450px; opacity: 0.7;
  filter: grayscale(0.8);
}
.card.loser .rank { font-family: 'Anton'; font-size: 2rem; color: #777; margin: 20px 0 10px; }
.card.loser .name { font-size: 1.8rem; font-weight: bold; color: #aaa; margin-bottom: 10px; }
.card.loser .score { font-family: 'Anton'; font-size: 2.5rem; color: #777; }
.avatar-lose { color: #777; margin-top: 20px; }

.stamp {
  position: absolute; top: 40%; left: 50%;
  transform: translate(-50%, -50%) rotate(-15deg);
  border: 8px solid #c0392b; color: #c0392b;
  font-family: 'Courier New', Courier, monospace;
  font-weight: 900; font-size: 5rem;
  padding: 10px 20px; letter-spacing: 10px;
  opacity: 0; animation: stampIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275) 1.5s forwards;
  z-index: 20; background: rgba(0,0,0,0.2);
  text-transform: uppercase;
}

.winner.blue { border-color: #3498db; box-shadow: 0 0 60px rgba(52, 152, 219, 0.4); }
.winner.red { border-color: #e74c3c; box-shadow: 0 0 60px rgba(231, 76, 60, 0.4); }

.draw-card { text-align: center; font-size: 2rem; color: #aaa; }
.draw-score { font-family: 'Anton'; font-size: 5rem; color: white; }

.home-btn {
  background: transparent; border: 2px solid rgba(255,255,255,0.2); color: white;
  padding: 15px 40px; border-radius: 50px; font-family: 'Anton'; font-size: 1.2rem;
  cursor: pointer; display: flex; align-items: center; gap: 10px; transition: all 0.2s;
}
.home-btn:hover { background: white; color: black; }

@keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
@keyframes stampIn {
  0% { opacity: 0; transform: translate(-50%, -50%) scale(3) rotate(-15deg); }
  100% { opacity: 0.9; transform: translate(-50%, -50%) scale(1) rotate(-15deg); }
}
</style>