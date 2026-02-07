---
layout: default
permalink: /proposal/
title: Will You Be My Valentine? 💕
nav: false
---

<style>
/* ===== FORCE REMOVE THEME FOOTER ===== */
footer,
.site-footer,
.page-footer {
  display: none !important;
  height: 0 !important;
  padding: 0 !important;
  margin: 0 !important;
}

/* --- General Styles --- */
body {
  background: linear-gradient(45deg, #ff8f8f, #ff4d6d);
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  overflow-y: hidden;
}

/* --- Page Setup --- */
.page {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  padding: 20px;
}

canvas {
  position: absolute;
  inset: 0;
  z-index: 0;
}

.box {
  max-width: 900px;
  width: 100%;
  padding: 45px;
  border-radius: 30px;
  text-align: center;
  position: relative;
  z-index: 1;
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.25);
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
}

.hidden { display: none; }
.fade-in { animation: fadeIn 1.4s ease forwards; }
.fade-out { animation: fadeOut 1s ease forwards; }

@keyframes fadeIn {
  from { opacity: 0; transform: scale(0.97); }
  to   { opacity: 1; transform: scale(1); }
}
@keyframes fadeOut {
  to { opacity: 0; transform: scale(0.97); }
}

/* --- Buttons --- */
.choice-container {
  display: flex;
  justify-content: center;
  gap: 30px;
  margin-top: 20px;
}

#yesBtn, #noBtn {
  padding: 18px 32px;
  font-size: 20px;
  border-radius: 12px;
  border: none;
  cursor: pointer;
  transition: transform 0.2s ease;
}

#yesBtn {
  background: #ff5d8f;
  color: white;
}

#noBtn {
  background: #ccc;
  position: fixed;
}

/* --- Hearts --- */
.heart {
  position: absolute;
  font-size: 32px;
  cursor: pointer;
}

/* --- Slideshow --- */
.slideshow {
  width: 100%;
  max-width: 600px;
  height: 360px;
  margin: 25px auto;
  border-radius: 20px;
  overflow: hidden;
  position: relative;
  touch-action: pan-y;
}

.slideshow img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0;
  transition: opacity 1.5s ease;
}

.slideshow img.active {
  opacity: 1;
}

@media (max-width: 600px) {
  .slideshow { height: 250px; }
}
</style>

<div class="page">
  <canvas id="particles"></canvas>

  <audio id="music" loop>
    <source src="{{ '/assets/audio/romance.mp3' | relative_url }}" type="audio/mpeg">
  </audio>

  <!-- Choice -->
  <div class="box" id="choiceBox">
    <h1>Will you be my Valentine, Diana? 💖</h1>
    <div class="choice-container">
      <button id="yesBtn">Yes 💕</button>
      <button id="noBtn">No 😅</button>
    </div>
  </div>

  <!-- Game -->
  <div class="box hidden" id="gameBox">
    <h1>Catch the hearts 💕</h1>
    <p>Tap 5 hearts to unlock your special message</p>
    <div id="counter">0 / 5</div>
  </div>

  <!-- Final -->
  <div class="box hidden" id="loveBox">
    <h1>You chose us ❤️</h1>
    <div class="slideshow" id="slideshow">
      {% assign images = site.static_files | where_exp: "f", "f.path contains '/assets/photos/'" %}
      {% for img in images %}
        <img src="{{ img.path | relative_url }}">
      {% endfor %}
    </div>
    <p>
      Diana, every moment with you is something I want to keep forever.
      <br><br>
      Thank you for being my Valentine. Always.
    </p>
  </div>
</div>

<script>
/* ===== MUSIC ===== */
const music = document.getElementById("music");
music.play().catch(() =>
  document.body.addEventListener("click", () => music.play(), { once: true })
);

/* ===== PARTICLES ===== */
const canvas = document.getElementById("particles");
const ctx = canvas.getContext("2d");

function resize() {
  canvas.width = innerWidth;
  canvas.height = innerHeight;
}
resize();
addEventListener("resize", resize);

const particles = Array.from({ length: 60 }, () => ({
  x: Math.random() * innerWidth,
  y: Math.random() * innerHeight,
  r: Math.random() * 2 + 1,
  dx: (Math.random() - 0.5) * 0.3,
  dy: (Math.random() - 0.5) * 0.3
}));

function drawParticles() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  particles.forEach(p => {
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    ctx.fillStyle = "rgba(255,255,255,0.5)";
    ctx.fill();
    p.x += p.dx;
    p.y += p.dy;
    if (p.x < 0 || p.x > canvas.width) p.dx *= -1;
    if (p.y < 0 || p.y > canvas.height) p.dy *= -1;
  });
  requestAnimationFrame(drawParticles);
}
drawParticles();

/* ===== NO BUTTON DODGE ===== */
const yesBtn = document.getElementById("yesBtn");
const noBtn = document.getElementById("noBtn");
function placeNoInitially() {
  const yesRect = yesBtn.getBoundingClientRect();
  // Put No button to the right of Yes button, but within screen
  let left = yesRect.right + 20;
  let top = yesRect.top;

  // Ensure it stays within viewport
  if (left + noBtn.offsetWidth > window.innerWidth) {
    left = window.innerWidth - noBtn.offsetWidth - 20;
  }
  if (top + noBtn.offsetHeight > window.innerHeight) {
    top = window.innerHeight - noBtn.offsetHeight - 20;
  }

  noBtn.style.left = `${left}px`;
  noBtn.style.top = `${top}px`;
}

// Run after everything is loaded
window.addEventListener("load", placeNoInitially);
window.addEventListener("resize", placeNoInitially);

window.onload = () => {
  const r = yesBtn.getBoundingClientRect();
  noBtn.style.left = r.right + 30 + "px";
  noBtn.style.top = r.top + "px";
};

function moveNo() {
  const x = Math.random() * (innerWidth - noBtn.offsetWidth - 20);
  const y = Math.random() * (innerHeight - noBtn.offsetHeight - 20);
  noBtn.style.left = x + "px";
  noBtn.style.top = y + "px";
}

document.addEventListener("mousemove", e => {
  const rect = noBtn.getBoundingClientRect();
  const distance = Math.hypot(
    e.clientX - (rect.left + rect.width / 2),
    e.clientY - (rect.top + rect.height / 2)
  );
  if (distance < 120) moveNo();
});


/* ===== GAME ===== */
const gameBox = document.getElementById("gameBox");
const choiceBox = document.getElementById("choiceBox");
const counter = document.getElementById("counter");
let caught = 0;

yesBtn.onclick = () => {
  choiceBox.classList.add("fade-out");
  setTimeout(() => {
    choiceBox.classList.add("hidden");
    startGame();
  }, 1000);
};

function spawnHeart() {
  const h = document.createElement("div");
  h.className = "heart";
  h.textContent = "❤️";
  h.style.left = Math.random() * 85 + "%";
  h.style.bottom = "-30px";
  gameBox.appendChild(h);

  h.animate(
    [{ transform: "translateY(0)" }, { transform: "translateY(-600px)" }],
    { duration: 4500 }
  );

  h.onclick = () => {
    h.remove();
    counter.textContent = `${++caught} / 5`;
    if (caught === 5) finishGame();
  };

  setTimeout(() => h.remove(), 5000);
}

let heartInterval;
function startGame() {
  gameBox.classList.remove("hidden");
  gameBox.classList.add("fade-in");
  caught = 0;
  counter.textContent = "0 / 5";
  heartInterval = setInterval(spawnHeart, 700);
}

/* ===== CONFETTI + FINAL ===== */
const loveBox = document.getElementById("loveBox");

function finishGame() {
  clearInterval(heartInterval);
  gameBox.classList.add("fade-out");

  setTimeout(() => {
    gameBox.classList.add("hidden");
    loveBox.classList.remove("hidden");
    loveBox.classList.add("fade-in");
    document.body.style.overflowY = "auto";
    startSlideshow();
  }, 1000);
}

/* ===== SLIDESHOW ===== */
function startSlideshow() {
  const slides = [...document.querySelectorAll("#slideshow img")];
  let i = 0, paused = false;

  slides[0]?.classList.add("active");

  setInterval(() => {
    if (paused) return;
    slides[i].classList.remove("active");
    i = (i + 1) % slides.length;
    slides[i].classList.add("active");
  }, 3500);

  const s = document.getElementById("slideshow");
  s.onmouseenter = () => paused = true;
  s.onmouseleave = () => paused = false;

  let startX = 0;
  s.addEventListener("touchstart", e => startX = e.touches[0].clientX);
  s.addEventListener("touchend", e => {
    const dx = e.changedTouches[0].clientX - startX;
    if (Math.abs(dx) > 40) {
      slides[i].classList.remove("active");
      i = dx < 0 ? (i + 1) % slides.length : (i - 1 + slides.length) % slides.length;
      slides[i].classList.add("active");
    }
  });
}
</script>
