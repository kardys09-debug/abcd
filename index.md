<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Zostaniesz moją walentynką? 💘</title>
  <style>
    * { box-sizing: border-box; }

    body {
      margin: 0;
      font-family: system-ui, -apple-system, Arial, sans-serif;
      background: linear-gradient(135deg, #ff758c, #ff7eb3);
      min-height: 100vh;
      overflow: hidden;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 16px;
      transition: background 1s ease;
    }

    .card {
      background: white;
      width: 100%;
      max-width: 420px;
      border-radius: 24px;
      padding: 28px 20px 32px;
      text-align: center;
      box-shadow: 0 12px 30px rgba(0,0,0,0.25);
      z-index: 2;
    }

    h1 {
      font-size: 24px;
      margin-bottom: 20px;
      line-height: 1.3;
    }

    .emoji-image {
      font-size: 64px;
      margin-bottom: 16px;
    }

    .buttons {
      display: flex;
      flex-direction: column;
      gap: 14px;
      margin-top: 20px;
    }

    button {
      width: 100%;
      padding: 16px 0;
      font-size: 20px;
      border-radius: 14px;
      border: none;
      cursor: pointer;
      touch-action: manipulation;
    }

    #yesBtn {
      background: #ff3b6f;
      color: white;
      font-weight: 600;
    }

    #noBtn {
      background: #eee;
      color: #333;
    }

    .hidden { display: none; }

    .result {
      margin-top: 10px;
      animation: fadeIn 0.6s ease forwards;
    }

    .big-sad {
      font-size: 28px;
      font-weight: 700;
      margin-top: 20px;
      color: #ff3b6f;
      animation: fadeIn 0.6s ease forwards;
    }

    .emoji, .firework, .loveMessage {
      position: fixed;
      pointer-events: none;
      user-select: none;
      white-space: nowrap;
      font-size: 28px;
    }

    .loveMessage {
      font-size: 24px;
      font-weight: bold;
      color: #ff3b6f;
      animation: rise 3s ease-out forwards, pulse 1s infinite alternate, rotate3d 3s infinite alternate;
      transform-origin: center;
    }

    .firework {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      animation: explode 1s ease-out forwards;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes explode {
      from { transform: scale(1); opacity: 1; }
      to { transform: translate(var(--x), var(--y)) scale(1.5); opacity: 0; }
    }

    @keyframes rise {
      from { transform: translateY(0); opacity: 1; }
      to { transform: translateY(-150px); opacity: 0; }
    }

    @keyframes pulse {
      from { transform: scale(1); }
      to { transform: scale(1.3); }
    }

    @keyframes rotate3d {
      0% { transform: rotateY(0deg) rotateX(0deg); }
      50% { transform: rotateY(15deg) rotateX(10deg); }
      100% { transform: rotateY(-15deg) rotateX(-10deg); }
    }
  </style>
</head>
<body>

  <div class="card">
    <h1>Zostaniesz moją walentynką? 💘</h1>

    <!-- Emoji trzymające różę -->
    <div class="emoji-image">🥰🌹</div>

    <div class="buttons" id="buttons">
      <button id="yesBtn">TAK</button>
      <button id="noBtn">NIE</button>
    </div>

    <div id="result" class="result hidden">
      <h2>Wiedziałem ezzz😌</h2>
    </div>

    <div id="sad" class="big-sad hidden">
      trudno może uda się za rok….😔😪
    </div>
  </div>

  <audio id="popSound">
    <source src="https://assets.mixkit.co/sfx/preview/mixkit-fast-small-sweep-transition-166.mp3" type="audio/mpeg">
  </audio>

  <script>
    const yesBtn = document.getElementById("yesBtn");
    const noBtn = document.getElementById("noBtn");
    const buttons = document.getElementById("buttons");
    const result = document.getElementById("result");
    const sad = document.getElementById("sad");
    const popSound = document.getElementById("popSound");
    const body = document.body;

    const loveEmojis = ["😝","😈","😘","😩"];
    const sadEmojis = ["😭","😢","😖","☹️"];
    const fireworkColors = ['#ff4d6d', '#ffd700', '#4dff88', '#4db8ff', '#ff4dbf', '#ffa64d'];

    yesBtn.addEventListener("click", () => {
      popSound.currentTime = 0;
      popSound.play();
      if (navigator.vibrate) navigator.vibrate(150);

      buttons.classList.add("hidden");
      result.classList.remove("hidden");

      // Tworzenie wielu unoszących się wiadomości
      for (let i = 0; i < 6; i++) {
        setTimeout(() => createLoveMessage(), i * 300);
      }

      // Animowany wybuch uczuć + fajerwerki przez 3 sekundy
      let interval = 0;
      const explosion = setInterval(() => {
        for (let i = 0; i < 8; i++) createEmoji(loveEmojis);
        for (let i = 0; i < 6; i++) createFirework();
        interval += 100;
        if (interval >= 3000) clearInterval(explosion);
      }, 100);
    });

    noBtn.addEventListener("click", () => {
      // zmiana tła na smutny kolor
      body.style.background = "#2c3e50";
      buttons.classList.add("hidden");
      sad.classList.remove("hidden");

      // Deszcz emotek smutku przez 10 sekund
      let interval = 0;
      const rain = setInterval(() => {
        for (let i = 0; i < 6; i++) createEmoji(sadEmojis);
        interval += 100;
        if (interval >= 10000) clearInterval(rain);
      }, 100);
    });

    function createEmoji(array) {
      const e = document.createElement("div");
      e.className = "emoji";
      e.innerText = array[Math.floor(Math.random() * array.length)];
      e.style.left = Math.random() * window.innerWidth + "px";
      e.style.top = Math.random() * window.innerHeight + "px";

      const angle = Math.random() * Math.PI * 2;
      const distance = 60 + Math.random() * 80;
      e.style.setProperty("--x", Math.cos(angle) * distance + "px");
      e.style.setProperty("--y", Math.sin(angle) * distance + "px");

      e.style.animation = "explode 1.5s ease-out forwards";

      document.body.appendChild(e);
      setTimeout(() => e.remove(), 1500);
    }

    function createFirework() {
      const fw = document.createElement("div");
      fw.className = "firework";
      fw.style.left = Math.random() * window.innerWidth + "px";
      fw.style.top = Math.random() * window.innerHeight + "px";

      fw.style.background = fireworkColors[Math.floor(Math.random() * fireworkColors.length)];

      const angle = Math.random() * Math.PI * 2;
      const distance = 60 + Math.random() * 80;
      fw.style.setProperty("--x", Math.cos(angle) * distance + "px");
      fw.style.setProperty("--y", Math.sin(angle) * distance + "px");

      document.body.appendChild(fw);
      setTimeout(() => fw.remove(), 1000);
    }

    function createLoveMessage() {
      const msg = document.createElement("div");
      msg.className = "loveMessage";
      msg.innerText = "kocham cię najbardziej na świecie rozalcia";
      msg.style.left = (Math.random() * (window.innerWidth - 300)) + "px";
      msg.style.top = (window.innerHeight * 0.6 + Math.random() * 50) + "px";

      document.body.appendChild(msg);
      setTimeout(() => msg.remove(), 3000);
    }
  </script>

</body>
</html>
