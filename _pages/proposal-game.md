---
layout: default
permalink: /proposal/game/
title: Just One More Thing 💕
nav: false
---

<style>
.game-wrapper {
  min-height: 85vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.game-box {
  width: 800px;
  max-width: 95%;
  padding: 40px;
  border-radius: 28px;
  background: linear-gradient(135deg, #ffe3ec, #f9c5d1);
  text-align: center;
  box-shadow: 0 30px 60px rgba(0,0,0,0.25);
  position: relative;
  overflow: hidden;
}

.game-box h1 {
  color: #c9184a;
  margin-bottom: 12px;
  font-size: 32px;
}

.game-box p {
  color: #6d2e46;
  margin-bottom: 22px;
  font-size: 17px;
}

.counter {
  font-size: 18px;
  margin-top: 18px;
  color: #6d2e46;
}

@media (max-width: 600px) {
  .game-box h1 { font-size: 28px; }
  .game-box p { font-size: 15px; }
  .heart { font-size: 30px; }
}
</style>

<div class="game-wrapper">
  <div class="game-box" id="game">
    <audio id="bg-music" loop>
      <source src="{{ '/assets/audio/romance.mp3' | relative_url }}" type="audio/mpeg">
    </audio>

    <h1>Catch the hearts 💕</h1>
    <p>Tap 5 hearts to unlock your special message</p>
    <div class="counter" id="counter">0 / 5</div>
  </div>
</div>

<script>
// Start music when the page loads
const bgMusic = document.getElementById('bg-music');
document.addEventListener('DOMContentLoaded', () => {
  // Delay play until user interaction (required on mobile browsers)
  document.body.addEventListener('click', () => bgMusic.play(), { once: true });
});

const game = document.getElementById("game");
const counter = document.getElementById("counter");
let caught = 0;

// Confetti effect
function burstConfetti() {
  const count = 150;
  const colors = ["#ff9ad3", "#ff4d6d", "#f8b195", "#f67280", "#c06c84"];
  for (let i = 0; i < count; i++) {
    const confetti = document.createElement("div");
    confetti.style.position = "absolute";
    confetti.style.width = "8px";
    confetti.style.height = "8px";
    confetti.style.background = colors[Math.floor(Math.random() * colors.length)];
    confetti.style.top = "50%";
    confetti.style.left = "50%";
    confetti.style.opacity = "0.9";
    confetti.style.transform = `translate(-50%, -50%) rotate(${Math.random()*360}deg)`;
    confetti.style.animation = `fly 1.8s ease-out forwards`;
    game.appendChild(confetti);
    setTimeout(() => confetti.remove(), 1800);
  }
}

game.addEventListener("click", () => {}); // ensures interaction

function spawnHeart() {
  const heart = document.createElement("div");
  heart.className = "heart";
  heart.innerHTML = "❤️";
  heart.style.position = "absolute";
  heart.style.cursor = "pointer";

  heart.style.left = Math.random() * 85 + "%";
  heart.style.bottom = "-30px";
  heart.style.transition = "transform 6s linear";

  game.appendChild(heart);

  heart.animate(
    [
      { transform: "translateY(0)" },
      { transform: "translateY(-580px)" }
    ],
    { duration: 6000 }
  );

  heart.onclick = () => {
    heart.remove();
    caught++;
    counter.textContent = `${caught} / 5`;

    if (caught >= 5) {
      bgMusic.pause();
      burstConfetti();
      setTimeout(() => {
        window.location.href = "{{ '/proposal/yes/' | relative_url }}";
      }, 2100);
    }
  };

  setTimeout(() => heart.remove(), 6000);
}

setInterval(spawnHeart, 900);
</script>

<style>
@keyframes fly {
  0% { opacity: 0.9; transform: translate(-50%, -50%) scale(1); }
  100% { opacity: 0; transform: translate(calc(-50% + 300px), calc(-50% - 450px)) scale(0.5); }
}
</style>
