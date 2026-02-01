---
layout: default
permalink: /proposal/
title: Will You Be My Valentine? 💕
nav: false
---

<style>
/* --- General Styles --- */
body {
  background: linear-gradient(45deg, #ff8f8f, #ff4d6d);
  font-family: 'Arial', sans-serif;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

/* --- Page Setup --- */
.page {
  min-height: 90vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden;
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
  background: rgba(255, 255, 255, 0.9); /* white overlay with transparency */
  backdrop-filter: blur(10px); /* optional: blur background for better contrast */
}

.hidden { display: none; }
.fade-in { animation: fadeIn 1.4s ease forwards; }
.fade-out { animation: fadeOut 1s ease forwards; }

@keyframes fadeIn { from {opacity:0; transform:scale(0.97);} to{opacity:1; transform:scale(1);} }
@keyframes fadeOut{ to {opacity:0; transform:scale(0.97);} }

/* --- Button Styles --- */
.choice-container {
  display: flex;
  justify-content: center;
  gap: 30px;
  margin-top: 20px;
  position: relative;
}
#yesBtn, #noBtn {
  padding: 18px 32px;
  font-size: 20px;
  border-radius: 12px;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
  z-index: 2;
}
#yesBtn { background: #ff5d8f; color: white; }
#yesBtn:hover { transform: scale(1.1); box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2); }
#noBtn { background: #ccc; }
#noBtn:hover { transform: scale(1.1); box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2); }

/* --- Heart Game --- */
.heart { position: absolute; font-size: 32px; cursor: pointer; }

/* --- Slideshow --- */
.slideshow {
  width: 100%;
  max-width: 600px;
  height: 360px;
  margin: 25px auto;
  border-radius: 20px;
  overflow: hidden;
  position: relative;
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
.slideshow img.active { opacity: 1; }

@media(max-width:600px) { .slideshow { height: 250px; } }

</style>

<div class="page">
  <canvas id="particles"></canvas>

  <!-- MUSIC -->
  <audio id="music" loop>
    <source src="{{ '/assets/audio/romance.mp3' | relative_url }}" type="audio/mpeg">
  </audio>

  <!-- --- Choice Page --- -->
  <div class="box" id="choiceBox">
    <h1>Will you be my Valentine, Diana? 💖</h1>
    <div class="choice-container">
      <button id="yesBtn">Yes 💕</button>
      <button id="noBtn">No 😅</button>
    </div>
  </div>

  <!-- --- Game Page --- -->
  <div class="box hidden" id="gameBox">
    <h1>Catch the hearts 💕</h1>
    <p>Tap 5 hearts to unlock your special message</p>
    <div id="counter">0 / 5</div>
  </div>

  <!-- --- Love Message + Slideshow --- -->
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
music.play().catch(() => {
  document.body.addEventListener("click", () => music.play(), { once: true });
});

/* ===== PARTICLES ===== */
const canvas = document.getElementById("particles");
const ctx = canvas.getContext("2d");

function resize() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}
resize();
window.addEventListener("resize", resize);

const particles = Array.from({ length: 60 }, () => ({
  x: Math.random() * canvas.width,
  y: Math.random() * canvas.height,
  r: Math.random() * 2 + 1,
  dx: (Math.random() - 0.5) * 0.3,
  dy: (Math.random() - 0.5) * 0.3,
}));

function drawParticles() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  particles.forEach((p) => {
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    ctx.fillStyle = "rgba(255, 255, 255, 0.5)";
    ctx.fill();
    p.x += p.dx;
    p.y += p.dy;
    if (p.x < 0 || p.x > canvas.width) p.dx *= -1;
    if (p.y < 0 || p.y > canvas.height) p.dy *= -1;
  });
  requestAnimationFrame(drawParticles);
}

drawParticles();

/* ===== CHOICE BUTTONS ===== */
const choiceBox = document.getElementById("choiceBox");
const yesBtn = document.getElementById("yesBtn");
const noBtn = document.getElementById("noBtn");

function moveNo() {
  const maxX = window.innerWidth - noBtn.offsetWidth;
  const maxY = window.innerHeight - noBtn.offsetHeight;

  const x = Math.random() * maxX;
  const y = Math.random() * maxY;
  const angle = Math.random() * 2 * Math.PI;

  const xMovement = Math.cos(angle) * 200;
  const yMovement = Math.sin(angle) * 200;

  noBtn.style.transition = "transform 0.4s ease-in-out";
  noBtn.style.transform = `translate(${x + xMovement}px, ${y + yMovement}px)`;
}

noBtn.addEventListener("mouseenter", moveNo); // Desktop
noBtn.addEventListener("touchstart", (e) => { e.preventDefault(); moveNo(); }); // Mobile

yesBtn.addEventListener("click", () => {
  choiceBox.classList.add("fade-out");
  setTimeout(() => {
    choiceBox.classList.add("hidden");
    startGame();
  }, 1000);
});

/* ===== GAME ===== */
const HEARTS_TO_WIN = 5;
let caught = 0;
const gameBox = document.getElementById("gameBox");
const counter = document.getElementById("counter");

function spawnHeart() {
  const heart = document.createElement("div");
  heart.className = "heart";
  heart.innerHTML = "❤️";
  heart.style.left = Math.random() * 85 + "%";
  heart.style.bottom = "-30px";
  gameBox.appendChild(heart);

  heart.animate([
    { transform: "translateY(0)" },
    { transform: `translateY(-${520 + Math.random() * 120}px)` }
  ], { duration: 4200 + Math.random() * 800 });

  heart.onclick = () => {
    heart.remove();
    caught++;
    counter.textContent = `${caught} / ${HEARTS_TO_WIN}`;
    if (caught >= HEARTS_TO_WIN) finishGame();
  };

  setTimeout(() => heart.remove(), 5000);
}

let heartInterval;

function startGame() {
  gameBox.classList.remove("hidden");
  gameBox.classList.add("fade-in");
  counter.textContent = `0 / ${HEARTS_TO_WIN}`;
  caught = 0;
  heartInterval = setInterval(spawnHeart, 700);
}

/* ===== CONFETTI ===== */
function confetti() {
  const colors = ["#ff4d6d", "#ff9ad3", "#f8b195", "#fcd1d1", "#ffe3ec"];
  for (let i = 0; i < 500; i++) {
    const conf = document.createElement("div");
    const size = Math.random() * 15 + 5;
    conf.style.position = "absolute";
    conf.style.width = conf.style.height = `${size}px`;
    conf.style.background = colors[Math.floor(Math.random() * colors.length)];
    conf.style.top = `${Math.random() * window.innerHeight}px`;
    conf.style.left = `${Math.random() * window.innerWidth}px`;
    conf.style.opacity = Math.random() * 0.7 + 0.3;
    conf.style.borderRadius = "50%";
    conf.style.pointerEvents = "none";
    conf.style.transform = `rotate(${Math.random() * 360}deg)`;

    document.body.appendChild(conf);

    const fall = conf.animate([
      { transform: `translateY(0px) rotate(0deg)` },
      { transform: `translateY(${window.innerHeight + 50}px) rotate(${Math.random() * 720}deg)` }
    ], {
      duration: 3500 + Math.random() * 1500,
      easing: "ease-out",
      fill: "forwards"
    });

    fall.onfinish = () => conf.remove();
  }
}

/* ===== FINISH GAME ===== */
const loveBox = document.getElementById("loveBox");

function finishGame() {
  clearInterval(heartInterval);
  confetti();
  gameBox.classList.add("fade-out");
  setTimeout(() => {
    gameBox.classList.add("hidden");
    loveBox.classList.remove("hidden");
    loveBox.classList.add("fade-in");
    startSlideshow();
  }, 1000);
}

/* ===== SLIDESHOW ===== */
function startSlideshow() {
  const slides = document.querySelectorAll("#slideshow img");
  let i = 0;
  if (slides.length) {
    slides[0].classList.add("active");
    setInterval(() => {
      slides[i].classList.remove("active");
      i = (i + 1) % slides.length;
      slides[i].classList.add("active");
    }, 3500);
  }
}
</script>
