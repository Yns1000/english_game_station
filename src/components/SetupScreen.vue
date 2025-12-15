<script setup>
import { Users, Swords, ArrowLeft, Play, Gamepad2 } from 'lucide-vue-next';

const props = defineProps(['teams']);
const emit = defineEmits(['start-game', 'back']);
</script>

<template>
  <div class="setup-screen">

    <div class="setup-card">

      <div class="header-section">
        <Gamepad2 :size="48" class="header-icon" />
        <h2 class="section-title">WHO IS PLAYING?</h2>
        <div class="section-subtitle">ENTER TEAM NAMES</div>
      </div>

      <div class="teams-input-wrapper">

        <div class="team-input-card blue">
          <div class="card-icon">
            <Users :size="32" />
          </div>
          <label>TEAM 1</label>
          <input
              v-model="teams.team1.name"
              type="text"
              placeholder="Blue Team..."
              maxlength="12"
          >
        </div>

        <div class="vs-badge">
          <Swords :size="64" stroke-width="1.5" />
        </div>

        <div class="team-input-card red">
          <div class="card-icon">
            <Users :size="32" />
          </div>
          <label>TEAM 2</label>
          <input
              v-model="teams.team2.name"
              type="text"
              placeholder="Red Team..."
              maxlength="12"
          >
        </div>

      </div>

      <div class="actions">
        <button @click="emit('back')" class="btn secondary">
          <ArrowLeft :size="20" />
          <span>BACK</span>
        </button>
        <button @click="emit('start-game')" class="btn primary">
          <span>LET'S PLAY</span>
          <Play :size="24" fill="currentColor" />
        </button>
      </div>

    </div>

  </div>
</template>

<style scoped>
.setup-screen {
  width: 100vw; height: 100vh;
  display: flex; align-items: center; justify-content: center;
  background-image: linear-gradient(rgba(0,0,0,0.1) 1px, transparent 1px), linear-gradient(90deg, rgba(0,0,0,0.1) 1px, transparent 1px);
  background-size: 50px 50px;
  overflow: hidden;
}

.setup-card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 50px 80px;
  border-radius: 40px;
  width: 90%; max-width: 1000px;
  box-shadow: 0 30px 60px rgba(0,0,0,0.6);
  display: flex; flex-direction: column; align-items: center;
  animation: slideUp 0.6s cubic-bezier(0.19, 1, 0.22, 1);
}

.header-section {
  text-align: center; margin-bottom: 50px;
  display: flex; flex-direction: column; align-items: center;
}

.header-icon { color: #f1c40f; margin-bottom: 15px; filter: drop-shadow(0 0 10px rgba(241, 196, 15, 0.5)); }

.section-title {
  font-family: 'Anton', sans-serif; font-size: 3.5rem; color: white;
  margin: 0; line-height: 1; letter-spacing: 2px;
  text-shadow: 4px 4px 0 #000;
}

.section-subtitle {
  font-family: Arial, sans-serif; letter-spacing: 4px; color: rgba(255,255,255,0.5);
  margin-top: 10px; font-weight: bold; font-size: 0.9rem;
}

.teams-input-wrapper {
  display: flex; align-items: center; gap: 30px; margin-bottom: 50px; width: 100%; justify-content: center;
}

.team-input-card {
  background: rgba(0, 0, 0, 0.3);
  border: 2px solid rgba(255,255,255,0.1);
  padding: 30px; border-radius: 20px;
  text-align: center; width: 320px;
  transition: all 0.3s;
  display: flex; flex-direction: column; align-items: center;
}

.team-input-card:focus-within { transform: translateY(-5px); }

.team-input-card.blue:focus-within { border-color: #3498db; box-shadow: 0 0 30px rgba(52, 152, 219, 0.3); }
.team-input-card.red:focus-within { border-color: #e74c3c; box-shadow: 0 0 30px rgba(231, 76, 60, 0.3); }

.card-icon {
  background: rgba(255,255,255,0.1); width: 60px; height: 60px;
  border-radius: 50%; display: flex; align-items: center; justify-content: center;
  margin-bottom: 15px;
}
.blue .card-icon { color: #3498db; }
.red .card-icon { color: #e74c3c; }

.team-input-card label {
  display: block; font-family: 'Anton', sans-serif; font-size: 1.5rem;
  margin-bottom: 15px; letter-spacing: 2px; color: rgba(255,255,255,0.8);
}

.team-input-card input {
  width: 100%; font-size: 2rem; text-align: center;
  padding: 10px; background: rgba(255,255,255,0.1);
  border: none; border-radius: 10px;
  font-family: 'Anton', sans-serif; color: white; outline: none;
  transition: background 0.2s;
}
.team-input-card input::placeholder { color: rgba(255,255,255,0.3); }
.team-input-card input:focus { background: rgba(255,255,255,0.2); }

.vs-badge {
  color: #f1c40f; display: flex; align-items: center; justify-content: center;
  filter: drop-shadow(0 0 15px rgba(241, 196, 15, 0.4));
  animation: pulse 2s infinite;
}

.actions { display: flex; justify-content: center; gap: 20px; width: 100%; }

.btn {
  font-family: 'Anton', sans-serif; font-size: 1.5rem; padding: 15px 40px;
  border-radius: 50px; cursor: pointer; border: none; transition: all 0.2s;
  display: flex; align-items: center; gap: 10px; letter-spacing: 1px;
}

.primary {
  background: #2ecc71; color: white;
  box-shadow: 0 10px 20px rgba(46, 204, 113, 0.3);
}
.primary:hover { transform: translateY(-3px); background: #27ae60; box-shadow: 0 15px 30px rgba(46, 204, 113, 0.5); }

.secondary {
  background: transparent; border: 2px solid rgba(255,255,255,0.2); color: #aaa;
}
.secondary:hover { border-color: white; color: white; background: rgba(255,255,255,0.05); }

@keyframes slideUp { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }
@keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.1); } 100% { transform: scale(1); } }
</style>