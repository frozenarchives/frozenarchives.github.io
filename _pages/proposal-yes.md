---
layout: default
permalink: /proposal/yes/
title: Always You ❤️
nav: false
---

<style>
.yes-wrapper {
  min-height: 90vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  position: relative;
  overflow: hidden;
}

.particles {
  position: absolute;
  inset: 0;
  z-index: 0;
}

.yes-box {
  max-width: 900px;
  width: 100%;
  padding: 50px;
  border-radius: 30px;
  background: linear-gradient(135deg, #ff758f, #ff7eb3);
  color: white;
  text-align: center;
  box-shadow: 0 30px 60px rgba(0,0,0,0.3);
  position: relative;
  z-index: 1;
}

.heart {
  font-size: 72px;
  margin-bottom: 20px;
  animation: pulse 1.6s infinite;
}

.slideshow {
  width: 100%;
  max-width: 600px;
  height: 380px;
  margin: 30px auto;
  position: relative;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 15px 30px rgba(0,0,0,0.25);
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

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.25); }
  100% { transform: scale(1); }
}

@media (max-width: 600px) {
  .slideshow { height: 260px; }
  .yes-box { padding: 35px 25px; }
}
</style>

<div class="yes-wrapper">
  <canvas class="particles" id="particles"></canvas>

  <div class="yes-box">
    <div class="heart">❤️</div>
    <h1>You chose us.</h1>

    <div class="slideshow" id="slideshow">
      {% assign images = site.static_files | where_exp: "file", "file.path contains '/assets/photos/'" %}
      {% for img in images %}
        <img src="{{ img.path | relative_url }}" alt="Memory {{ forloop.index }}">
      {% endfor %}
    </div>

    <p>
      Diana, every moment with you is something I want to keep forever.  
      These are just a few of my favorite memories — and there are so many more to come.
      <br><br>
      Thank you for being my Valentine. Always.
    </p>
  </div>
</div>

<script>
/* ===== Slideshow ===== */
const slides = document.querySelectorAll("#slideshow img");
let current = 0;

if (slides.length > 0) {
  slides[0].classList.add("active");
  setInterval(() => {
    slides[current].classList.remove("active");
    current = (current + 1) % slides.length;
    slides[current].classList.add("active");
  }, 3500);
}

/* ===== Particle Animation ===== */
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
  a: Math.random() * 0.6 + 0.2
}));

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  particles.forEach(p => {
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    ctx.fillStyle = `rgba(255,255,255,${p.a})`;
    ctx.fill();
    p.x += p.dx;
    p.y += p.dy;

    if (p.x < 0 || p.x > canvas.width) p.dx *= -1;
    if (p.y < 0 || p.y > canvas.height) p.dy *= -1;
  });
  requestAnimationFrame(draw);
}
draw();
</script>
