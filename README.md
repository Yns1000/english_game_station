# English Game Station

**English Game Station** is an all-in-one interactive web platform designed to gamify classroom activities. Built with **Vue.js**, it features 4 distinct TV-show inspired game modes to challenge students on vocabulary, grammar, and general culture.

Designed for **full-screen projection**, with sound effects, animations, and a centralized scoring system.

---

## Game Modes

### 1. Family Feud
*Based on the famous TV show.*
* **Goal:** Two teams face off to guess the most popular answers to survey questions.
* **Features:** Flip-card animations, "Strike" buzzers, point banking system, and face-off mechanic.

### 2. Hot Seat
*Based on Taboo / Password.*
* **Goal:** One student sits with their back to the board and must guess a word described by their team within 60 seconds.
* **Features:** Countdown timer, forbidden words list, "Pass/Correct" controls, and intense music.

### 3. Motus (Lingo)
*Based on the French/Dutch TV show.*
* **Goal:** Guess the hidden word.
* **Mechanic:** Classic color-coded feedback (🔴 Red = Correct, 🟡 Yellow = Misplaced).
* **Features:** Virtual keyboard support and score streaks.

### 4. Who Wants to Be a Millionaire
*The ultimate quiz challenge.*
* **Goal:** Answer 15 increasingly difficult questions to reach the top.
* **Features:** 3 Lifelines (50:50, Phone a Friend, Ask Audience), safety nets, and "Walk Away" strategic option.

---

## Key Features

* **Global Session:** Team names and scores persist across all games. You can switch from Family Feud to Motus without losing points.
* **Immersive Audio:** Dynamic background music for each game state, sound effects (correct/wrong), and a global mute button.
* **Smooth UI:** Glassmorphism design, neon glows, and fluid animations (Vue Transition).

---

## Tech Stack

* **Framework:** [Vue.js 3](https://vuejs.org/) (Composition API)
* **Build Tool:** [Vite](https://vitejs.dev/)
* **Icons:** [Lucide Vue Next](https://lucide.dev/)
* **Styling:** CSS3 (Flexbox, Grid, Animations, Radial Gradients)

---

## How to Run

### Prerequisites
Make sure you have [Node.js](https://nodejs.org/) installed on your machine.

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Yns1000/english_game_station.git
    cd english_game_station
    ```

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Start the app:**
    ```bash
    npm run dev
    ```

4.  **Play:**
    Open your browser at `http://localhost:5173` (or the link shown in your terminal).

---

## Controls (Host Guide)

The interface is designed to be controlled by the teacher/host via mouse or keyboard shortcuts.

### Global
| Action | Control                                               |
| :--- |:------------------------------------------------------|
| **Mute/Unmute Music** | Click the 🔊 icon (bottom-right).                     |
| **Finish Session** | Click the **FINISH** button on the Home Screen.       |
| **Reset Data** | Click the **RESET** button (clears teams & scores). |

### Family Feud
| Action | Control                                           |
| :--- |:--------------------------------------------------|
| **Strike (X)** | Press **SPACEBAR** or click the "STRIKE" button.  |
| **Reveal Answer** | **Click** on the numbered card.                   |
| **Bank Points** | Click the green **BANK** button.                |

### Hot Seat
| Action | Control |
| :--- | :--- |
| **Valid Answer** | Click **✅ CORRECT (+5)**. |
| **Mistake / Skip** | Click **❌ TABOO** or **⏭ SKIP**. |
| **Pause Timer** | Click the **⏸** icon next to the timer. |

### Motus
| Action | Control |
| :--- | :--- |
| **Type Letters** | Physical Keyboard or On-screen click. |
| **Validate** | Press **ENTER**. |

### Millionaire
| Action | Control                                            |
| :--- |:---------------------------------------------------|
| **Select Answer** | Click on A, B, C, or D.                            |
| **Final Answer** | Click **YES, FINAL ANSWER** to confirm.            |
| **Jokers** | Click the icons (50:50, Phone, People) at the top. |
| **Secure Gains** | Click the **TAKE MONEY** button.                 |

---

## 👥 Authors & Credits

**Developed by:**
* Younes
* Hakim

**Context:**
Project realized for **Centrale Lille IG2I** & **Thyltech**.