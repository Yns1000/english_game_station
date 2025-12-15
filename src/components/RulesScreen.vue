<script setup>
import { computed } from 'vue';
import {
  Users, Brain, Coins, Ban, ArrowLeft, Check,
  Flame, Clock, MessageSquareOff, SkipForward,
  Keyboard, Type, Grid3X3, Eye,
  Divide, Phone, BarChart3, Lock
} from 'lucide-vue-next';

const props = defineProps(['activeGame']);
const emit = defineEmits(['next', 'back']);

const rulesContent = {
  feud: {
    title: "FAMILY FEUD",
    subtitle: "GUESS THE MOST POPULAR ANSWERS",
    accent: "#2ecc71",
    bg: "radial-gradient(circle at center, #27ae60 0%, #004d26 100%)",
    steps: [
      { icon: Users, color: 'blue', title: "2 TEAMS", text: "Face off to guess the top answers hidden on the board." },
      { icon: Brain, color: 'purple', title: "GUESS ANSWERS", text: "Find the most popular words based on surveys." },
      { icon: Coins, color: 'gold', title: "BANK POINTS", text: "Points go to the bank. Winner of the round takes all." },
      { icon: Ban, color: 'red', title: "AVOID STRIKES", text: "3 wrong answers? You lose control of the board!" }
    ]
  },
  hotseat: {
    title: "HOT SEAT",
    subtitle: "DESCRIBE WORDS UNDER PRESSURE",
    accent: "#e74c3c",
    bg: "radial-gradient(circle at center, #c0392b 0%, #500000 100%)",
    steps: [
      { icon: Flame, color: 'red', title: "THE HOT SEAT", text: "One player sits with their back to the board." },
      { icon: Clock, color: 'gold', title: "60 SECONDS", text: "Describe as many words as possible before time runs out." },
      { icon: MessageSquareOff, color: 'purple', title: "FORBIDDEN WORDS", text: "Don't say the word itself or the 6 forbidden words!" },
      { icon: SkipForward, color: 'blue', title: "SKIP OR PASS", text: "Stuck? Skip it, but time is ticking!" }
    ]
  },
  motus: {
    title: "MOTUS",
    subtitle: "CRACK THE HIDDEN WORD",
    accent: "#3498db",
    bg: "radial-gradient(circle at center, #3498db 0%, #001f3f 100%)",
    steps: [
      { icon: Type, color: 'blue', title: "GUESS THE WORD", text: "Propose a word starting with the given letter." },
      { icon: Grid3X3, color: 'red', title: "COLOR CODES", text: "Red = Correct place. Yellow = Wrong place." },
      { icon: Keyboard, color: 'purple', title: "KEYBOARD", text: "Use the virtual keyboard or type directly." },
      { icon: Coins, color: 'gold', title: "EARN POINTS", text: "Find the word to score 20 points for your team." }
    ]
  },
  millionaire: {
    title: "MILLIONAIRE",
    subtitle: "CLIMB THE LADDER TO GLORY",
    accent: "#9b59b6",
    bg: "radial-gradient(circle at center, #8e44ad 0%, #2c3e50 100%)",
    steps: [
      { icon: Brain, color: 'blue', title: "15 QUESTIONS", text: "Answer correctly to climb the money ladder." },
      { icon: Lock, color: 'gold', title: "SAFETY NETS", text: "Secure your earnings at Q5 and Q10." },
      { icon: BarChart3, color: 'purple', title: "USE JOKERS", text: "50:50, Phone a Friend, or Ask the Audience." },
      { icon: Coins, color: 'red', title: "WALK AWAY", text: "Not sure? Stop and keep your current earnings." }
    ]
  }
};

const currentRule = computed(() => {
  return rulesContent[props.activeGame] || rulesContent.feud;
});
</script>

<template>
  <div class="rules-screen" :style="{ background: currentRule.bg }">

    <div class="rules-card">
      <div class="header-section">
        <h2 class="rules-title" :style="{ color: currentRule.accent }">{{ currentRule.title }}</h2>
        <div class="rules-subtitle">{{ currentRule.subtitle }}</div>
      </div>

      <div class="rules-grid">
        <div
            v-for="(step, index) in currentRule.steps"
            :key="index"
            class="rule-item"
            :class="step.color"
        >
          <div class="icon-box">
            <component :is="step.icon" :size="32" stroke-width="2.5" />
          </div>
          <div class="text-content">
            <strong>{{ step.title }}</strong>
            <p>{{ step.text }}</p>
          </div>
        </div>
      </div>

      <div class="actions">
        <button @click="emit('back')" class="btn secondary">
          <ArrowLeft :size="20" />
          <span>BACK</span>
        </button>
        <button
            @click="emit('next')"
            class="btn primary"
            :style="{ background: currentRule.accent, boxShadow: `0 10px 20px ${currentRule.accent}40` }"
        >
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
  background-size: 50px 50px;
  overflow: hidden;
  transition: background 0.5s ease;
}

.rules-card {
  background: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.15);
  padding: 50px;
  border-radius: 40px;
  max-width: 900px;
  width: 90%;
  box-shadow: 0 30px 60px rgba(0,0,0,0.5);
  animation: zoomIn 0.6s cubic-bezier(0.19, 1, 0.22, 1);
  display: flex; flex-direction: column; gap: 40px;
}

.header-section { text-align: center; }

.rules-title {
  font-family: 'Anton', sans-serif; font-size: 4rem;
  margin: 0; line-height: 1;
  text-transform: uppercase; letter-spacing: 2px;
  text-shadow: 4px 4px 0 rgba(0,0,0,0.5);
}

.rules-subtitle {
  font-family: Arial, sans-serif; letter-spacing: 4px; color: rgba(255,255,255,0.8);
  margin-top: 10px; font-weight: bold; font-size: 0.9rem;
}

.rules-grid {
  display: grid; grid-template-columns: 1fr 1fr; gap: 20px;
}

.rule-item {
  display: flex; align-items: center; gap: 20px;
  background: rgba(255, 255, 255, 0.05);
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
  margin: 5px 0 0; color: #ddd; font-size: 0.95rem; line-height: 1.4; font-family: Arial, sans-serif;
}

.actions { display: flex; justify-content: center; gap: 20px; margin-top: 10px; }

.btn {
  font-family: 'Anton', sans-serif; font-size: 1.4rem; padding: 15px 40px;
  border-radius: 50px; cursor: pointer; border: none; transition: all 0.2s;
  display: flex; align-items: center; gap: 10px; letter-spacing: 1px;
}

.primary {
  color: white;
}
.primary:hover { transform: translateY(-3px); filter: brightness(1.1); }

.secondary {
  background: transparent; border: 2px solid rgba(255,255,255,0.2); color: #aaa;
}
.secondary:hover { border-color: white; color: white; background: rgba(255,255,255,0.05); }

@keyframes zoomIn { from { opacity: 0; transform: scale(0.9); } to { opacity: 1; transform: scale(1); } }
</style>