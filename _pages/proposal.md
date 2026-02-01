---
layout: default
permalink: /proposal/
title: Will You Be My Valentine? 💕
nav: false
---

<style>
/* --- Page setup --- */
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
  box-shadow: 0 30px 60px rgba(0,0,0,0.25);
  background: linear-gradient(135deg, #ffe3ec, #f9c5d1);
}

.hidden { display: none; }
.fade-in { animation: fadeIn 1.4s ease forwards; }
.fade-out { animation: fadeOut 1s ease forwards; }

@keyframes fadeIn { from {opacity:0; transform:scale(0.97);} to{opacity:1; transform:scale(1);} }
@keyframes fadeOut{ to {opacity:0; transform:scale(0.97);} }

/* --- Choice Buttons --- */
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
}
#yesBtn { background:#ff5d8f; color:white; }
#noBtn { background:#ccc; }

/* --- Game hearts --- */
.heart { position:absolute; font-size:32px; cursor:pointer; }

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
.slideshow img.active { opacity:1; }

@media(max-width:600px) { .slideshow{height:250px;} }
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
  document.body.addEventListener("click", () => music.play(), { once:true });
});

/* ===== PARTICLES ===== */
const canvas = document.getElementById("particles");
const ctx = canvas.getContext("2d");
function resize(){ canvas.width=innerWidth; canvas.height=innerHeight; }
resize(); addEventListener("resize",resize);
const particles = Array.from({length:60},()=>({x:Math.random()*canvas.width, y:Math.random()*canvas.height, r:Math.random()*2+1, dx:(Math.random()-0.5)*0.3, dy:(Math.random()-0.5)*0.3}));
(function draw(){ ctx.clearRect(0,0,canvas.width,canvas.height); particles.forEach(p=>{ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,Math.PI*2);ctx.fillStyle="rgba(255,255,255,0.5)";ctx.fill();p.x+=p.dx;p.y+=p.dy;if(p.x<0||p.x>canvas.width)p.dx*=-1;if(p.y<0||p.y>canvas.height)p.dy*=-1; }); requestAnimationFrame(draw); })();

/* ===== CHOICE BUTTONS ===== */
const choiceBox = document.getElementById("choiceBox");
const yesBtn = document.getElementById("yesBtn");
const noBtn = document.getElementById("noBtn");

function moveNo() {
  const container = document.querySelector(".choice-container");
  const w = container.offsetWidth - noBtn.offsetWidth;
  const h = container.offsetHeight - noBtn.offsetHeight;
  const x = Math.random() * w;
  const y = Math.random() * h;
  noBtn.style.transform = `translate(${x}px,${y}px)`;
}
noBtn.addEventListener("mouseenter", moveNo); // desktop
noBtn.addEventListener("touchstart", e=>{ e.preventDefault(); moveNo(); }); // mobile

yesBtn.addEventListener("click", () => {
  choiceBox.classList.add("fade-out");
  setTimeout(()=>{
    choiceBox.classList.add("hidden");
    startGame();
  }, 1000);
});

/* ===== GAME ===== */
const HEARTS_TO_WIN = 5;
let caught=0;
const gameBox = document.getElementById("gameBox");
const counter = document.getElementById("counter");
function spawnHeart(){
  const heart=document.createElement("div");
  heart.className="heart";
  heart.innerHTML="❤️";
  heart.style.left=Math.random()*85+"%";
  heart.style.bottom="-30px";
  gameBox.appendChild(heart);
  heart.animate([{transform:"translateY(0)"},{transform:"translateY(-600px)"}],{duration:4500});
  heart.onclick=()=>{ heart.remove(); caught++; counter.textContent=`${caught} / ${HEARTS_TO_WIN}`; if(caught>=HEARTS_TO_WIN) finishGame(); };
  setTimeout(()=>heart.remove(),5000);
}
let heartInterval;
function startGame(){
  gameBox.classList.remove("hidden");
  gameBox.classList.add("fade-in");
  counter.textContent=`0 / ${HEARTS_TO_WIN}`;
  caught=0;
  heartInterval=setInterval(spawnHeart,700);
}

/* ===== CONFETTI ===== */
function confetti(){
  for(let i=0;i<120;i++){
    const c=document.createElement("div");
    c.style.position="absolute";
    c.style.width="8px"; c.style.height="8px";
    c.style.background=["#ff4d6d","#ff9ad3","#f8b195"][Math.floor(Math.random()*3)];
    c.style.top="50%"; c.style.left="50%";
    c.style.transform=`rotate(${Math.random()*360}deg)`;
    c.style.animation="fly 1.8s ease-out forwards";
    document.body.appendChild(c);
    setTimeout(()=>c.remove(),1800);
  }
}

/* ===== FINISH GAME ===== */
const loveBox = document.getElementById("loveBox");
function finishGame(){
  clearInterval(heartInterval);
  confetti();
  gameBox.classList.add("fade-out");
  setTimeout(()=>{
    gameBox.classList.add("hidden");
    loveBox.classList.remove("hidden");
    loveBox.classList.add("fade-in");
    startSlideshow();
  },1000);
}

/* ===== SLIDESHOW ===== */
function startSlideshow(){
  const slides = document.querySelectorAll("#slideshow img");
  let i=0;
  if(slides.length){
    slides[0].classList.add("active");
    setInterval(()=>{
      slides[i].classList.remove("active");
      i=(i+1)%slides.length;
      slides[i].classList.add("active");
    },3500);
  }
}
</script>

<style>
@keyframes fly { to{ transform: translate(300px,-500px); opacity:0;} }
</style>
