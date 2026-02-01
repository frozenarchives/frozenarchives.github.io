---
layout: default
permalink: /proposal/game/
title: For You ❤️
nav: false
---

<style>
.page {
  min-height: 90vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  position: relative;
  overflow: hidden;
}

canvas {
  position: absolute;
  inset: 0;
  z-index: 0;
}

.box {
  width: 900px;
  max-width: 95%;
  padding: 45px;
  border-radius: 30px;
  background: linear-gradient(135deg, #ffe3ec, #f9c5d1);
  box-shadow: 0 30px 60px rgba(0,0,0,0.25);
  text-align: center;
  position: relative;
  z-index: 1;
}

.hidden {
  display: none;
}

.fade-in {
  animation: fadeIn 1.4s ease forwards;
}

.fade-out {
  animation: fadeOut 1s ease forwards;
}

@keyframes fadeIn {
  from { opacity: 0; transform: scale(0.97); }
  to { opacity: 1; transform: scale(1); }
}

@keyframes fadeOut {
  to { opacity: 0; transform: scale(0.97); }
}

/* Game */
.heart {
  position: absolute;
  font-size: 32px;
  cursor: pointer;
}

/* Slideshow */
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

  <!-- GAME -->
  <div class="box" id="gameBox">
    <h1>Catch the hearts 💕</h1>
    <p>Tap 5 hearts to unlock something special</p>
    <div id="counter">0 / 5</div>
  </div>

  <!-- MESSAGE -->
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

/* ===== GAME ===== */
const HEARTS_TO_WIN = 5;
let caught = 0;

const gameBox = document.getElementById("gameBox");
const counter = document.getElementById("counter");

counter.textContent = `0 / ${HEARTS_TO_WIN}`;

function spawnHeart() {
  const heart = document.createElement("div");
  heart.className = "heart";
  heart.innerHTML = "❤️";
  heart.style.left = Math.random() * 85 + "%";
  heart.style.bottom = "-30px";

  gameBox.appendChild(heart);

  heart.animate(
    [
      { transform: "translateY(0)" },
      { transform: "translateY(-600px)" }
    ],
    { duration: 4500 }
  );

  heart.onclick = () => {
    heart.remove();
    caught++;
    counter.textContent = `${caught} / ${HEARTS_TO_WIN}`;

    if (caught >= HEARTS_TO_WIN) finishGame();
  };

  setTimeout(() => heart.remove(), 5000);
}

const heartInterval = setInterval(spawnHeart, 700);

/* ===== CONFETTI ===== */
function confetti() {
  for (let i = 0; i < 120; i++) {
    const c = document.createElement("div");
    c.style.position = "absolute";
    c.style.width = "8px";
    c.style.height = "8px";
    c.style.background = ["#ff4d6d","#ff9ad3","#f8b195"][Math.floor(Math.random()*3)];
    c.style.top = "50%";
    c.style.left = "50%";
    c.style.transform = `rotate(${Math.random()*360}deg)`;
    c.style.animation = "fly 1.8s ease-out forwards";
    document.body.appendChild(c);
    setTimeout(() => c.remove(), 1800);
  }
}

/* ===== TRANSITION ===== */
function finishGame() {
  clearInterval(heartInterval);
  confetti();

  gameBox.classList.add("fade-out");

  setTimeout(() => {
    gameBox.classList.add("hidden");
    showLove();
  }, 1000);
}

/* ===== SLIDESHOW ===== */
function showLove() {
  const loveBox = document.getElementById("loveBox");
  loveBox.classList.remove("hidden");
  loveBox.classList.add("fade-in");

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
  x: Math.random()*canvas.width,
  y: Math.random()*canvas.height,
  r: Math.random()*2+1,
  dx:(Math.random()-0.5)*0.3,
  dy:(Math.random()-0.5)*0.3
}));

(function draw(){
  ctx.clearRect(0,0,canvas.width,canvas.height);
  particles.forEach(p=>{
    ctx.beginPath();
    ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
    ctx.fillStyle="rgba(255,255,255,0.5)";
    ctx.fill();
    p.x+=p.dx; p.y+=p.dy;
    if(p.x<0||p.x>canvas.width)p.dx*=-1;
    if(p.y<0||p.y>canvas.height)p.dy*=-1;
  });
  requestAnimationFrame(draw);
})();
</script>

<style>
@keyframes fly {
  to { transform: translate(300px,-500px); opacity:0; }
}
</style>
