<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Birthday Surprise 🎂</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      text-align: center;
      background: linear-gradient(to right, #ffdde1, #ee9ca7);
      font-family: 'Poppins', sans-serif;
      overflow: hidden;
    }
    canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
    }
    #surprise-message {
      display: none;
      animation: fadeIn 2s ease-in-out;
      position: relative;
      z-index: 2;
    }
    h1 {
      font-size: 2.5em;
      margin-top: 50px;
      color: #ff0066;
      text-shadow: 2px 2px 5px #fff;
    }
    p {
      font-size: 1.2em;
      color: #222;
      margin: 20px;
    }
    .btn {
      margin-top: 200px;
      padding: 15px 30px;
      font-size: 1.5em;
      background: #ff4081;
      color: white;
      border: none;
      border-radius: 30px;
      cursor: pointer;
      box-shadow: 0 4px 10px rgba(0,0,0,0.2);
      transition: 0.3s;
      z-index: 3;
      position: relative;
    }
    .btn:hover {
      background: #e60073;
      transform: scale(1.1);
    }
    .balloon {
      position: absolute;
      bottom: -150px;
      width: 80px;
      height: 100px;
      background: red;
      border-radius: 50%;
      animation: float 8s infinite ease-in;
      z-index: 1;
    }
    .balloon:before {
      content: "";
      position: absolute;
      top: 100px;
      left: 38px;
      width: 2px;
      height: 100px;
      background: #444;
    }
    @keyframes float {
      0% { transform: translateY(0); opacity: 1; }
      100% { transform: translateY(-120vh); opacity: 0; }
    }
    .heart {
      position: absolute;
      font-size: 2rem;
      animation: rise 6s linear infinite;
      z-index: 1;
    }
    @keyframes rise {
      0% { transform: translateY(0); opacity: 1; }
      100% { transform: translateY(-100vh); opacity: 0; }
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    /* Slideshow styles */
    .slideshow {
      margin-top: 20px;
      position: relative;
      width: 300px;
      height: 400px;
      margin-left: auto;
      margin-right: auto;
      overflow: hidden;
      border-radius: 20px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.4);
    }
    .slideshow img {
      position: absolute;
      width: 100%;
      height: 100%;
      object-fit: cover;
      opacity: 0;
      transition: opacity 1.5s ease-in-out;
    }
    .slideshow img.active {
      opacity: 1;
    }
  </style>
</head>
<body>
  <canvas id="fireworks"></canvas>
  <button class="btn" onclick="revealSurprise()">🎁 Click to Reveal Surprise 🎁</button>
  <div id="surprise-message">
    <h1>🎉 Happy Birthday, Talkative Tangiii 💖♾️ 🎂</h1>
    <p>✨ You are my best friend, my guide, and my forever support.<br>
       Wishing you endless joy, love, and laughter on your special day! 💐</p>
    <div class="slideshow">
      <img src="sister1.jpg" class="slide active">
      <img src="sister2.jpg" class="slide">
      <img src="sister3.jpg" class="slide">
    </div>
  </div>
  <audio id="birthdaySong" src="https://www2.cs.uic.edu/~i101/SoundFiles/HappyBirthday.mid"></audio>
  <script>
    function revealSurprise() {
      document.querySelector(".btn").style.display = "none";
      document.getElementById("surprise-message").style.display = "block";
      document.getElementById("birthdaySong").play();
      setInterval(createBalloon, 1000);
      setInterval(createHeart, 700);
      launchFireworks();
      startSlideshow();
    }
    function createBalloon() {
      const balloon = document.createElement("div");
      balloon.classList.add("balloon");
      balloon.style.left = Math.random() * window.innerWidth + "px";
      balloon.style.background = `hsl(${Math.random() * 360}, 70%, 60%)`;
      document.body.appendChild(balloon);
      setTimeout(() => balloon.remove(), 8000);
    }
    function createHeart() {
      const heart = document.createElement("div");
      heart.classList.add("heart");
      heart.style.left = Math.random() * window.innerWidth + "px";
      heart.innerHTML = "💖";
      document.body.appendChild(heart);
      setTimeout(() => heart.remove(), 6000);
    }
    function launchFireworks() {
      const canvas = document.getElementById("fireworks");
      const ctx = canvas.getContext("2d");
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      let particles = [];
      function random(min, max) {
        return Math.random() * (max - min) + min;
      }
      function createFirework(x, y) {
        const count = 100;
        for (let i = 0; i < count; i++) {
          particles.push({
            x: x,
            y: y,
            speed: random(1, 6),
            angle: random(0, Math.PI * 2),
            radius: 2,
            alpha: 1,
            color: `hsl(${Math.random() * 360}, 100%, 60%)`
          });
        }
      }
      function updateFireworks() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        for (let i = 0; i < particles.length; i++) {
          let p = particles[i];
          p.x += Math.cos(p.angle) * p.speed;
          p.y += Math.sin(p.angle) * p.speed;
          p.alpha -= 0.01;
          if (p.alpha <= 0) {
            particles.splice(i, 1);
            i--;
            continue;
          }
          ctx.globalAlpha = p.alpha;
          ctx.fillStyle = p.color;
          ctx.beginPath();
          ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
          ctx.fill();
        }
        requestAnimationFrame(updateFireworks);
      }
      canvas.addEventListener("click", (e) => {
        createFirework(e.clientX, e.clientY);
      });
      setInterval(() => {
        createFirework(random(100, canvas.width - 100), random(100, canvas.height - 100));
      }, 1500);
      updateFireworks();
    }
    function startSlideshow() {
      let slides = document.querySelectorAll(".slideshow img");
      let index = 0;
      setInterval(() => {
        slides[index].classList.remove("active");
        index = (index + 1) % slides.length;
        slides[index].classList.add("active");
      }, 4000);
    }
  </script>
</body>
</html>