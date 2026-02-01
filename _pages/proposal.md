---
layout: default
permalink: /proposal/
title: Proposal
nav: true
nav_order: 1
---

<style>
.proposal-container {
  min-height: 80vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.proposal-card {
  background: linear-gradient(135deg, #ff9a9e, #fad0c4);
  padding: 60px;
  border-radius: 25px;
  text-align: center;
  box-shadow: 0 20px 40px rgba(0,0,0,0.25);
}

.proposal-card h1 {
  color: #fff;
  font-size: 42px;
  margin-bottom: 40px;
}

.proposal-buttons {
  position: relative;
  height: 120px;
}

.proposal-buttons button {
  padding: 15px 40px;
  font-size: 20px;
  border: none;
  border-radius: 30px;
  cursor: pointer;
}

#yes-btn {
  background-color: #ff4d6d;
  color: white;
  transition: transform 0.2s ease;
}

#yes-btn:hover {
  transform: scale(1.1);
}

#no-btn {
  background-color: #eee;
  color: #666;
  position: absolute;
  left: 220px;
  top: 0;
}
</style>

<div class="proposal-container">
  <div class="proposal-card">
    <h1>Diana, will you be my Valentine? 💖</h1>

    <div class="proposal-buttons">
      <button id="yes-btn" onclick="acceptLove()">Yes 💕</button>
      <button id="no-btn">No 🙃</button>
    </div>
  </div>
</div>

<script>
const noBtn = document.getElementById("no-btn");

noBtn.addEventListener("mouseover", () => {
  const maxX = window.innerWidth - noBtn.offsetWidth;
  const maxY = window.innerHeight - noBtn.offsetHeight;

  const x = Math.random() * maxX;
  const y = Math.random() * maxY;

  noBtn.style.left = `${x}px`;
  noBtn.style.top = `${y}px`;
});

function acceptLove() {
  window.location.href = "{{ '/proposal/yes/' | relative_url }}";
}
</script>
