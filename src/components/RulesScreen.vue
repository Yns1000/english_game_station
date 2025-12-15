<script setup>
import { Users, Brain, Coins, Ban, ArrowLeft, Check } from 'lucide-vue-next';

const emit = defineEmits(['next', 'back']);
</script>

<template>
  <div class="rules-screen">

    <div class="rules-card">
      <div class="header-section">
        <h2 class="rules-title">HOW TO PLAY</h2>
        <div class="rules-subtitle">MASTER THE GAME IN 4 STEPS</div>
      </div>

      <div class="rules-grid">
        <div class="rule-item blue">
          <div class="icon-box">
            <Users :size="32" stroke-width="2.5" />
          </div>
          <div class="text-content">
            <strong>2 TEAMS</strong>
            <p>Face off to guess the top answers hidden on the board.</p>
          </div>
        </div>

        <div class="rule-item purple">
          <div class="icon-box">
            <Brain :size="32" stroke-width="2.5" />
          </div>
          <div class="text-content">
            <strong>GUESS ANSWERS</strong>
            <p>Find the most popular words. Use your common sense!</p>
          </div>
        </div>

        <div class="rule-item gold">
          <div class="icon-box">
            <Coins :size="32" stroke-width="2.5" />
          </div>
          <div class="text-content">
            <strong>BANK POINTS</strong>
            <p>Points go to the bank. Winner of the round takes all.</p>
          </div>
        </div>

        <div class="rule-item red">
          <div class="icon-box">
            <Ban :size="32" stroke-width="2.5" />
          </div>
          <div class="text-content">
            <strong>AVOID STRIKES</strong>
            <p>3 wrong answers? You lose control of the board!</p>
          </div>
        </div>
      </div>

      <div class="actions">
        <button @click="emit('back')" class="btn secondary">
          <ArrowLeft :size="20" />
          <span>BACK</span>
        </button>
        <button @click="emit('next')" class="btn primary">
          <span>UNDERSTOOD!</span>
          <Check :size="24" stroke-width="3" />
        </button>
      </div>
    </div>

  </div>
</template>

<style scoped>
.rules-screen {
  width: 100vw; height: 100vh;
  display: flex; align-items: center; justify-content: center;
  background-image: linear-gradient(rgba(0,0,0,0.1) 1px, transparent 1px), linear-gradient(90deg, rgba(0,0,0,0.1) 1px, transparent 1px);
  background-size: 50px 50px;
  overflow: hidden;
}

.rules-card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 50px;
  border-radius: 40px;
  max-width: 900px;
  width: 90%;
  box-shadow: 0 30px 60px rgba(0,0,0,0.6);
  animation: zoomIn 0.6s cubic-bezier(0.19, 1, 0.22, 1);
  display: flex; flex-direction: column; gap: 40px;
}

.header-section { text-align: center; }

.rules-title {
  font-family: 'Anton', sans-serif; font-size: 4rem;
  color: #f1c40f; margin: 0; line-height: 1;
  text-transform: uppercase; letter-spacing: 2px;
  text-shadow: 4px 4px 0 #000;
}

.rules-subtitle {
  font-family: Arial, sans-serif; letter-spacing: 4px; color: rgba(255,255,255,0.6);
  margin-top: 10px; font-weight: bold; font-size: 0.9rem;
}

.rules-grid {
  display: grid; grid-template-columns: 1fr 1fr; gap: 20px;
}

.rule-item {
  display: flex; align-items: center; gap: 20px;
  background: rgba(0, 0, 0, 0.3);
  padding: 20px 25px; border-radius: 20px;
  border: 1px solid transparent;
  transition: all 0.3s;
}

.rule-item:hover {
  transform: translateY(-5px);
  background: rgba(255,255,255,0.1);
}

.rule-item.blue { border-left: 4px solid #3498db; }
.rule-item.blue .icon-box { color: #3498db; background: rgba(52, 152, 219, 0.1); }

.rule-item.purple { border-left: 4px solid #9b59b6; }
.rule-item.purple .icon-box { color: #9b59b6; background: rgba(155, 89, 182, 0.1); }

.rule-item.gold { border-left: 4px solid #f1c40f; }
.rule-item.gold .icon-box { color: #f1c40f; background: rgba(241, 196, 15, 0.1); }

.rule-item.red { border-left: 4px solid #e74c3c; }
.rule-item.red .icon-box { color: #e74c3c; background: rgba(231, 76, 60, 0.1); }

.icon-box {
  width: 60px; height: 60px;
  display: flex; align-items: center; justify-content: center;
  border-radius: 50%; flex-shrink: 0;
}

.text-content strong {
  display: block; font-family: 'Anton', sans-serif; font-size: 1.4rem; letter-spacing: 1px; color: white;
}
.text-content p {
  margin: 5px 0 0; color: #ccc; font-size: 0.95rem; line-height: 1.4; font-family: Arial, sans-serif;
}

.actions { display: flex; justify-content: center; gap: 20px; margin-top: 10px; }

.btn {
  font-family: 'Anton', sans-serif; font-size: 1.4rem; padding: 15px 40px;
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

@keyframes zoomIn { from { opacity: 0; transform: scale(0.9); } to { opacity: 1; transform: scale(1); } }
</style>