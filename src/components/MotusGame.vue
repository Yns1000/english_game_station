<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { Trophy, X, ArrowRight, Delete, LogOut } from 'lucide-vue-next';
import motusData from '../assets/motus.json';

const props = defineProps(['teams']);
const emit = defineEmits(['quit']);

const MAX_ATTEMPTS = 6;

const step = ref('intro');
const activeTeamKey = ref('team1');
const targetWord = ref('');
const grid = ref([]);
const currentRow = ref(0);
const currentCol = ref(0);
const message = ref('');
const shakeRow = ref(false);
const keyboardState = ref({});

const keyboardRows = [
  ['A','Z','E','R','T','Y','U','I','O','P'],
  ['Q','S','D','F','G','H','J','K','L','M'],
  ['W','X','C','V','B','N']
];

const getUniqueWord = () => {
  const history = JSON.parse(sessionStorage.getItem('motus_history') || '[]');

  let available = motusData.filter(word => !history.includes(word));

  if (available.length === 0) {
    available = motusData;
    sessionStorage.removeItem('motus_history');
  }

  const word = available[Math.floor(Math.random() * available.length)];

  history.push(word);
  sessionStorage.setItem('motus_history', JSON.stringify(history));

  return word.toUpperCase();
};

const startRound = (teamKey) => {
  activeTeamKey.value = teamKey;

  targetWord.value = getUniqueWord();

  keyboardState.value = {};

  const len = targetWord.value.length;
  grid.value = Array.from({ length: MAX_ATTEMPTS }, () =>
      Array.from({ length: len }, () => ({ char: '.', status: 'empty' }))
  );

  grid.value[0][0] = { char: targetWord.value[0], status: 'fixed' };

  currentRow.value = 0;
  currentCol.value = 1;
  step.value = 'playing';
  message.value = '';
};

const handleKeydown = (e) => {
  if (step.value !== 'playing') return;
  const key = e.key.toUpperCase();
  if (key === 'ENTER') submitRow();
  else if (key === 'BACKSPACE') deleteChar();
  else if (/^[A-Z]$/.test(key)) addChar(key);
};

const addChar = (char) => {
  if (currentCol.value < targetWord.value.length) {
    grid.value[currentRow.value][currentCol.value].char = char;
    grid.value[currentRow.value][currentCol.value].status = 'filled';
    currentCol.value++;
  }
};

const deleteChar = () => {
  if (currentCol.value > 0) {
    currentCol.value--;
    grid.value[currentRow.value][currentCol.value].char = '.';
    grid.value[currentRow.value][currentCol.value].status = 'empty';
  }
};

const submitRow = () => {
  const currentLine = grid.value[currentRow.value];

  const isComplete = currentLine.every(cell => cell.char !== '.' && cell.char !== '');
  if (!isComplete) {
    showMessage("WORD INCOMPLETE!");
    shakeRow.value = true;
    setTimeout(() => shakeRow.value = false, 500);
    return;
  }

  const attempt = currentLine.map(c => c.char).join('');
  const target = targetWord.value;
  const targetArr = target.split('');

  for (let i = 0; i < target.length; i++) {
    const char = attempt[i];
    if (char === target[i]) {
      currentLine[i].status = 'correct';
      targetArr[i] = null;
      updateKeyboard(char, 'correct');
    } else {
      currentLine[i].status = 'miss';
    }
  }

  for (let i = 0; i < target.length; i++) {
    const char = attempt[i];
    if (currentLine[i].status !== 'correct') {
      const idx = targetArr.indexOf(char);
      if (idx !== -1) {
        currentLine[i].status = 'present';
        targetArr[idx] = null;
        updateKeyboard(char, 'present');
      } else {
        updateKeyboard(char, 'miss');
      }
    }
  }

  if (attempt === target) {
    setTimeout(() => {
      step.value = 'win';
      props.teams[activeTeamKey.value].score += 20;
    }, 500);
  } else {
    if (currentRow.value < MAX_ATTEMPTS - 1) {
      prepareNextRow(currentLine);
    } else {
      step.value = 'lose';
    }
  }
};

const updateKeyboard = (char, status) => {
  const current = keyboardState.value[char];
  if (current === 'correct') return;
  if (current === 'present' && status === 'miss') return;
  keyboardState.value[char] = status;
};

const prepareNextRow = (prevLine) => {
  const nextRowIdx = currentRow.value + 1;
  const nextLine = grid.value[nextRowIdx];

  for (let i = 0; i < prevLine.length; i++) {
    if (prevLine[i].status === 'correct' || prevLine[i].status === 'fixed') {
      nextLine[i].char = prevLine[i].char;
      nextLine[i].status = 'fixed';
    } else {
      nextLine[i].char = '.';
      nextLine[i].status = 'empty';
    }
  }

  currentRow.value++;

  const firstEmptyIndex = nextLine.findIndex(c => c.char === '.');
  if (firstEmptyIndex !== -1) {
    currentCol.value = firstEmptyIndex;
  } else {
    currentCol.value = prevLine.length;
  }
};

const showMessage = (msg) => {
  message.value = msg;
  setTimeout(() => message.value = '', 2000);
};

onMounted(() => window.addEventListener('keydown', handleKeydown));
onUnmounted(() => window.removeEventListener('keydown', handleKeydown));
</script>

<template>
  <div class="motus-screen">

    <div class="header-bar">
      <div class="team-score blue" :class="{ 'active': activeTeamKey === 'team1' }">
        {{ teams.team1.name }} <span class="s-val">{{ teams.team1.score }}</span>
      </div>

      <div class="motus-logo">
        <span class="l-m">M</span>
        <span class="l-o">O</span>
        <span class="l-t">T</span>
        <span class="l-u">U</span>
        <span class="l-s">S</span>
      </div>

      <div class="team-score red" :class="{ 'active': activeTeamKey === 'team2' }">
        {{ teams.team2.name }} <span class="s-val">{{ teams.team2.score }}</span>
      </div>
    </div>

    <div v-if="step === 'intro'" class="intro-box">
      <h1>READY TO PLAY?</h1>
      <p>Guess the word. <span style="color:#e74c3c">Red</span> is correct, <span style="color:#f1c40f">Yellow</span> is misplaced.</p>
      <div class="selection-btns">
        <button @click="startRound('team1')" class="sel-btn blue">{{ teams.team1.name }} PLAYS</button>
        <div class="vs">OR</div>
        <button @click="startRound('team2')" class="sel-btn red">{{ teams.team2.name }} PLAYS</button>
      </div>
    </div>

    <div v-if="step === 'playing' || step === 'win' || step === 'lose'" class="game-area">

      <div v-if="message" class="alert-msg">{{ message }}</div>

      <div class="grid-container">
        <div
            v-for="(row, rIndex) in grid"
            :key="rIndex"
            class="grid-row"
            :class="{ 'current': rIndex === currentRow, 'shake': rIndex === currentRow && shakeRow }"
        >
          <div
              v-for="(cell, cIndex) in row"
              :key="cIndex"
              class="grid-cell"
              :class="[cell.status, { 'active-cursor': rIndex === currentRow && cIndex === currentCol }]"
          >
            {{ cell.char }}
          </div>
        </div>
      </div>

      <div class="virtual-keyboard">
        <div v-for="(row, i) in keyboardRows" :key="i" class="kb-row">
          <button
              v-for="key in row"
              :key="key"
              @click="addChar(key)"
              class="kb-key"
              :class="keyboardState[key]"
          >
            {{ key }}
          </button>
        </div>
        <div class="kb-row actions">
          <button @click="deleteChar" class="kb-key action del"><Delete :size="20" /></button>
          <button @click="submitRow" class="kb-key action enter">ENTER</button>
        </div>
      </div>

    </div>

    <div v-if="step === 'win'" class="result-modal win">
      <Trophy :size="80" class="bounce" />
      <h2>MOTUS!</h2>
      <div class="word-reveal">
        <span v-for="(l, i) in targetWord" :key="i" class="reveal-letter">{{ l }}</span>
      </div>
      <p class="points-gain">+20 POINTS</p>
      <button @click="step = 'intro'" class="next-btn">NEXT ROUND <ArrowRight /></button>
    </div>

    <div v-if="step === 'lose'" class="result-modal lose">
      <X :size="80" class="shake" />
      <h2>GAME OVER</h2>
      <div class="word-reveal">
        <span v-for="(l, i) in targetWord" :key="i" class="reveal-letter">{{ l }}</span>
      </div>
      <button @click="step = 'intro'" class="next-btn">TRY AGAIN <ArrowRight /></button>
    </div>

    <div class="footer-bar">
      <button @click="emit('quit')" class="quit-btn">
        <LogOut :size="18" /> EXIT GAME
      </button>
    </div>

  </div>
</template>

<style scoped>
.motus-screen {
  width: 100vw; height: 100vh;
  background: radial-gradient(circle at center, #005f9e 0%, #001f3f 100%);
  display: flex; flex-direction: column; align-items: center;
  color: white; font-family: 'Arial', sans-serif;
  overflow: hidden; user-select: none;
}

.header-bar {
  width: 100%; height: 90px; display: flex; justify-content: space-between; align-items: center;
  padding: 0 40px; background: rgba(0,0,0,0.3); border-bottom: 2px solid #f1c40f;
  box-shadow: 0 5px 15px rgba(0,0,0,0.3); z-index: 10; flex-shrink: 0;
}

.motus-logo {
  font-family: 'Anton'; font-size: 3rem; display: flex; gap: 2px;
  filter: drop-shadow(0 0 10px rgba(0,0,0,0.5));
}
.motus-logo span { color: #e74c3c; text-shadow: 2px 2px 0 #fff; }

.team-score {
  font-family: 'Anton'; font-size: 1.2rem; padding: 10px 25px; border-radius: 50px;
  background: rgba(0,0,0,0.4); opacity: 0.6; transition: all 0.3s; border: 2px solid transparent;
}
.team-score.active { opacity: 1; transform: scale(1.05); border-color: white; box-shadow: 0 0 20px rgba(255,255,255,0.2); }
.blue.active { background: #3498db; } .red.active { background: #e74c3c; }
.s-val { font-size: 1.8rem; margin-left: 10px; color: #f1c40f; }

.footer-bar {
  width: 100%; height: 60px; display: flex; justify-content: center; align-items: center;
  background: rgba(0,0,0,0.2); margin-top: auto; flex-shrink: 0;
}
.quit-btn {
  background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2);
  color: #aaa; cursor: pointer; padding: 8px 25px; border-radius: 20px;
  display: flex; gap: 8px; align-items: center; font-family: 'Anton'; font-size: 0.9rem;
  transition: all 0.2s;
}
.quit-btn:hover { background: #e74c3c; border-color: #e74c3c; color: white; }

.intro-box { margin-top: 50px; text-align: center; animation: slideUp 0.5s; }
.intro-box h1 { font-family: 'Anton'; font-size: 4rem; margin-bottom: 10px; text-transform: uppercase; text-shadow: 4px 4px 0 #000; }
.intro-box p { font-size: 1.2rem; margin-bottom: 40px; color: #ccc; }
.selection-btns { display: flex; gap: 20px; align-items: center; justify-content: center; }
.sel-btn {
  font-family: 'Anton'; font-size: 1.5rem; padding: 20px 40px; border: none; border-radius: 15px;
  cursor: pointer; transition: transform 0.2s; color: white; box-shadow: 0 5px 0 rgba(0,0,0,0.3);
}
.sel-btn:active { transform: translateY(5px); box-shadow: none; }
.sel-btn.blue { background: #3498db; } .sel-btn.red { background: #e74c3c; }
.vs { font-family: 'Anton'; font-size: 1.5rem; color: #7f8c8d; }

.game-area {
  flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 15px; width: 100%; padding: 10px;
}
.grid-container {
  display: flex; flex-direction: column; gap: 4px;
  background: #004070; padding: 10px; border-radius: 10px;
  border: 4px solid #f1c40f; box-shadow: 0 10px 30px rgba(0,0,0,0.5);
}
.grid-row { display: flex; gap: 4px; }
.grid-cell {
  width: 50px; height: 50px;
  background: #005f9e; border: 2px solid rgba(255,255,255,0.2);
  display: flex; align-items: center; justify-content: center;
  font-family: 'Anton'; font-size: 2.2rem; color: white;
  text-transform: uppercase; position: relative;
}
.grid-cell.empty { color: rgba(255,255,255,0.3); font-size: 2rem; }
.grid-cell.filled { color: white; border-color: white; }

.grid-cell.correct, .grid-cell.fixed { background: #e74c3c; border-color: #c0392b; z-index: 2; }
.grid-cell.present { background: #005f9e; color: black; z-index: 2; }
.grid-cell.present::before {
  content: ''; position: absolute; width: 40px; height: 40px;
  background: #f1c40f; border-radius: 50%; z-index: -1;
}
.grid-cell.present { z-index: 1; display: flex; align-items: center; justify-content: center; }
.grid-cell.active-cursor { border-color: #f1c40f; box-shadow: 0 0 10px #f1c40f; }

.alert-msg {
  position: absolute; top: 100px; background: #e74c3c; color: white;
  font-weight: bold; padding: 8px 25px; border-radius: 50px;
  font-family: 'Anton'; letter-spacing: 1px; z-index: 50;
  animation: pop 0.3s; box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}

.virtual-keyboard {
  display: flex; flex-direction: column; gap: 5px; align-items: center; margin-bottom: 10px;
}
.kb-row { display: flex; gap: 4px; }
.kb-key {
  width: 38px; height: 42px; background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2);
  color: white; font-family: 'Anton'; font-size: 1.1rem; cursor: pointer; border-radius: 5px;
  transition: all 0.1s;
}
.kb-key:hover { background: rgba(255,255,255,0.3); }
.kb-key:active { transform: translateY(2px); }
.kb-key.action { width: auto; padding: 0 15px; font-size: 0.9rem; }
.enter { background: #2ecc71; border-color: #27ae60; }
.del { background: #e74c3c; border-color: #c0392b; }

.kb-key.correct { background: #e74c3c; border-color: #c0392b; }
.kb-key.present { background: #f1c40f; border-color: #f39c12; color: black; }
.kb-key.miss { opacity: 0.2; }

.result-modal {
  position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
  background: rgba(0,0,0,0.95); border: 2px solid white; padding: 40px; border-radius: 30px;
  text-align: center; width: 450px; z-index: 100;
  box-shadow: 0 0 50px rgba(0,0,0,0.8);
}
.result-modal h2 { font-family: 'Anton'; font-size: 4rem; margin: 10px 0; line-height: 1; }
.word-reveal { display: flex; gap: 5px; justify-content: center; margin: 20px 0; }
.reveal-letter {
  background: #e74c3c; color: white; width: 50px; height: 50px;
  display: flex; align-items: center; justify-content: center;
  font-family: 'Anton'; font-size: 2rem; border-radius: 5px;
}
.points-gain { font-family: 'Anton'; font-size: 2rem; color: #f1c40f; margin-bottom: 20px; }
.win { border-color: #f1c40f; } .win h2 { color: #f1c40f; }
.lose { border-color: #e74c3c; } .lose h2 { color: #e74c3c; }

.next-btn {
  background: white; color: black; border: none; padding: 15px 40px;
  font-family: 'Anton'; font-size: 1.2rem; cursor: pointer; border-radius: 50px;
  display: flex; align-items: center; justify-content: center; gap: 10px; width: 100%;
}
.next-btn:hover { background: #ddd; transform: translateY(-2px); }

@keyframes shake { 0%, 100% { transform: translateX(0); } 25% { transform: translateX(-10px); } 75% { transform: translateX(10px); } }
@keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
@keyframes slideUp { from { transform: translateY(50px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
@keyframes pop { from { transform: scale(0.5); opacity: 0; } to { transform: scale(1); opacity: 1; } }
</style>