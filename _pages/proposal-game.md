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
}

.game-box {
  width: 750px;
  max-width: 90%;
  padding: 50px;
  border-radius: 30px;
  background: linear-gradient(135deg, #ffe3ec, #f9c5d1);
  text-align: center;
  box-shadow: 0 30px 60px rgba(0,0,0,0.25);
  position: relative;
  overflow: hidden;
}

.game-box h1 {
  color: #c9184a;
  margin-bottom: 10px;
}

.game-box p {
  color: #6d2e46;
  margin-bottom: 30px;
  font-size: 18px;
}

.heart {
  position: absolute;
  font-size: 36px;
  cursor: pointer;
  animation: float 6s linear infinite;
}

@keyframes float {
  from { transform: translateY(100px); opacity: 0; }
  to { transform: translateY(-600px); opacity: 1; }
}

.counter {
  font-size: 18px;
  margin-top: 20px;
  color: #6d2e46;
}
</style>

<div class="game-wrapper">
  <div class="game-box" id="game">
    <h1>Catch the hearts 💕</h1>
    <p>Click 5 hearts to unlock your message</p>
    <div class="counter" id="counter">0 / 5</div>
  </div>
</div>

<script>
const game = document.getElementById("game");
const counter = document.getElementById("counter");
let caught = 0;

function spawnHeart() {
  const heart = document.createElement("div");
  heart.className = "heart";
  heart.innerHTML = "❤️";

  heart.style.left = Math.random() * 90 + "%";
  heart.style.bottom = "-40px";
  heart.style.animationDuration = (4 + Math.random() * 3) + "s";

  heart.onclick = () => {
    heart.remove();
    caught++;
    counter.textContent = `${caught} / 5`;

    if (caught >= 5) {
      window.location.href = "{{ '/proposal/yes/' | relative_url }}";
    }
  };

  game.appendChild(heart);

  setTimeout(() => heart.remove(), 7000);
}

setInterval(spawnHeart, 900);
</script>
