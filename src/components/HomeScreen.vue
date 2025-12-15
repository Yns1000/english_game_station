<script setup>
import { Play, Gamepad2, CodeXml, X } from 'lucide-vue-next';
import logoCentrale from '../assets/ig2i_white.png';
import logoThyltech from '../assets/thyltech_logo_name_white.png';

const emit = defineEmits(['go-to-setup']);
</script>

<template>
  <div class="home-screen">

    <div class="hero-content">

      <div class="icon-floating">
        <Gamepad2 :size="80" color="#f1c40f" stroke-width="1.5" />
      </div>

      <h1 class="game-title">
        <span class="line-1">FAMILY</span>
        <span class="line-2">FEUD</span>
      </h1>

      <div class="edition-badge">
        ENGLISH CLASS EDITION
      </div>

      <button @click="emit('go-to-setup')" class="start-btn">
        <Play :size="32" fill="currentColor" />
        <span>START GAME</span>
      </button>

    </div>

    <footer class="team-footer">

      <div class="logos-container">
        <img :src="logoCentrale" alt="Centrale Lille IG2I" class="footer-logo" />
        <X :size="24" color="#bdc3c7" class="logo-separator" />
        <img :src="logoThyltech" alt="Hyltech" class="footer-logo" />
      </div>

      <div class="team-label">
        <CodeXml :size="20" />
        <span>DEVELOPED BY</span>
      </div>
      <div class="team-names">
        <span class="member">Younes</span>
        <span class="dot">•</span>
        <span class="member">Hakim</span>
        <span class="dot">•</span>
        <span class="member">Tom</span>
      </div>
    </footer>

  </div>
</template>

<style scoped>
.home-screen {
  width: 100vw; height: 100vh;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  background-image: linear-gradient(rgba(0,0,0,0.1) 1px, transparent 1px), linear-gradient(90deg, rgba(0,0,0,0.1) 1px, transparent 1px);
  background-size: 50px 50px;
  position: relative;
  overflow: hidden;
}

.hero-content {
  text-align: center;
  display: flex; flex-direction: column; align-items: center;
  animation: zoomIn 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  z-index: 10;
  padding-bottom: 4rem;
}

.icon-floating {
  margin-bottom: 20px;
  animation: float 3s infinite ease-in-out;
  filter: drop-shadow(0 0 15px rgba(241, 196, 15, 0.5));
}

.game-title {
  display: flex; flex-direction: column;
  font-family: 'Anton', sans-serif;
  line-height: 0.85;
  margin: 0;
  text-transform: uppercase;
  filter: drop-shadow(0 10px 20px rgba(0,0,0,0.5));
}

.line-1 {
  font-size: 6rem; color: white; letter-spacing: 5px;
  text-shadow: 0 0 20px rgba(255,255,255,0.3);
}

.line-2 {
  font-size: 9rem; color: #f1c40f; letter-spacing: 10px;
  text-shadow: 4px 4px 0 #000, 0 0 30px rgba(241, 196, 15, 0.6);
  position: relative; top: -10px;
}

.edition-badge {
  background: #e74c3c;
  color: white;
  font-family: Arial, sans-serif; font-weight: bold; font-size: 1.2rem; letter-spacing: 3px;
  padding: 8px 30px;
  border-radius: 50px;
  margin-top: 10px; margin-bottom: 50px;
  border: 2px solid white;
  box-shadow: 0 5px 15px rgba(231, 76, 60, 0.5);
  transform: rotate(-2deg);
}

.start-btn {
  background: #2ecc71;
  color: white;
  border: none;
  padding: 20px 60px;
  font-family: 'Anton', sans-serif; font-size: 2.5rem; letter-spacing: 2px;
  border-radius: 60px;
  cursor: pointer;
  display: flex; align-items: center; gap: 15px;
  transition: all 0.2s;
  box-shadow: 0 10px 30px rgba(46, 204, 113, 0.4);
  position: relative;
  overflow: hidden;
}

.start-btn::after {
  content: ''; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
  background: linear-gradient(rgba(255,255,255,0.2), transparent);
}

.start-btn:hover {
  transform: scale(1.05) translateY(-5px);
  background: #27ae60;
  box-shadow: 0 20px 40px rgba(46, 204, 113, 0.6);
}
.start-btn:active { transform: scale(0.95); }

.team-footer {
  position: absolute; bottom: 30px;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(5px);
  padding: 15px 40px;
  border-radius: 20px;
  border: 1px solid rgba(255,255,255,0.1);
  display: flex; flex-direction: column; align-items: center; gap: 5px;
  animation: slideUp 1s ease-out 0.5s backwards;
}

.logos-container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 10px;
}

.footer-logo {
  max-height: 30px;
  width: auto;
  object-fit: contain;
}

.logo-separator {
  margin: 0 15px;
}

.team-label {
  display: flex; align-items: center; gap: 8px;
  color: #bdc3c7; font-size: 0.8rem; letter-spacing: 2px;
}

.team-names {
  display: flex; align-items: center; gap: 15px;
  color: white; font-family: 'Anton', sans-serif; font-size: 1.2rem; letter-spacing: 1px;
}

.dot { color: #f1c40f; font-size: 1.5rem; line-height: 0; }
.member { text-shadow: 0 2px 5px rgba(0,0,0,0.5); }

@keyframes float { 0%, 100% { transform: translateY(0) rotate(0deg); } 50% { transform: translateY(-15px) rotate(5deg); } }
@keyframes zoomIn { from { opacity: 0; transform: scale(0.8); } to { opacity: 1; transform: scale(1); } }
@keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
</style>