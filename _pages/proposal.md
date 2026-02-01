---
layout: default
permalink: /proposal/
title: A Question 💕
nav: false
---

<style>
.proposal-wrapper {
  min-height: 85vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.proposal-box {
  width: 700px;
  max-width: 90%;
  padding: 60px 50px 80px;
  border-radius: 30px;
  background:
    radial-gradient(circle at top left, #ffdde1, transparent 60%),
    radial-gradient(circle at bottom right, #ee9ca7, transparent 55%),
    linear-gradient(135deg, #fdfbfb, #f7e7ea);
  box-shadow: 0 30px 60px rgba(0,0,0,0.25);
  position: relative;
  overflow: hidden;
}

.proposal-box::after {
  content: "💖";
  position: absolute;
  font-size: 180px;
  opacity: 0.08;
  bottom: -40px;
  right: -20px;
}

.proposal-box h1 {
  font-size: 40px;
  color: #c9184a;
  text-align: center;
  margin-bottom: 15px;
}

.subtitle {
  text-align: center;
  font-size: 18px;
  color: #6d2e46;
  margin-bottom: 50px;
}

.button-area {
  position: relative;
  height: 140px;
}

.button-area button {
  padding: 16px 42px;
  font-size: 18px;
  border-radius: 999px;
  border: none;
  cursor: pointer;
  position: absolute;
}

#yes-btn {
  left: 50%;
  transform: translateX(-120%);
  background: linear-gradient(135deg, #ff4d6d, #ff758f);
  color: white;
  box-shadow: 0 12px 25px rgba(255,77,109,0.45);
}

#yes-btn:hover {
  transform: translateX(-120%) scale(1.08);
}

#no-btn {
  left: 50%;
  transform: translateX(20%);
  background: #f1f1f1;
  color: #666;
}
</style>

<div class="proposal-wrapper">
  <div class="proposal-box" id="box">
    <h1>Diana, will you be my Valentine?</h1>
    <div class="subtitle">
      I have a feeling I already know the answer… 💕
    </div>

    <div class="button-area" id="area">
      <button id="yes-btn" onclick="sayYes()">Yes 💖</button>
      <button id="no-btn">No 🙃</button>
    </div>
  </div>
</div>

<script>
const noBtn = document.getElementById("no-btn");
const area = document.getElementById("area");

let activated = false;

noBtn.addEventListener("mouseenter", () => {
  if (!activated) {
    activated = true; // first hover triggers movement
  }

  const areaRect = area.getBoundingClientRect();
  const btnRect = noBtn.getBoundingClientRect();

  const maxX = areaRect.width - btnRect.width;
  const maxY = areaRect.height - btnRect.height;

  const x = Math.random() * maxX;
  const y = Math.random() * maxY;

  noBtn.style.left = `${x}px`;
  noBtn.style.top = `${y}px`;
});

function sayYes() {
  window.location.href = "{{ '/proposal/yes/' | relative_url }}";
}
</script>
