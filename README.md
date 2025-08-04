<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Birthday Cake with Candles</title>
<link href="https://fonts.googleapis.com/css2?family=Pacifico&display=swap" rel="stylesheet" />
<style>
  body {
    background: #ffe6f0;
    font-family: 'Arial', sans-serif;
    text-align: center;
    margin: 0;
    padding: 0;
  }

  .cake-container {
    position: relative;
    margin: 100px auto;
    width: 220px;
    height: 220px;
  }

  .cake {
    position: relative;
    width: 100%;
    height: 220px;
  }

  .tier {
    position: absolute;
    background: pink;
    border: 4px solid #ff99cc;
    border-radius: 10px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
    left: 50%;
    transform: translateX(-50%);
  }

  .tier.bottom {
    width: 220px;
    height: 110px;
    bottom: 0;
    z-index: 1;
  }

  .tier.top {
    width: 160px;
    height: 90px;
    bottom: 100px;
    z-index: 2;
  }

  .sprinkles {
    position: absolute;
    width: 100%;
    height: 100%;
    pointer-events: none;
  }

  .sprinkle {
    width: 5px;
    height: 5px;
    border-radius: 50%;
    position: absolute;
  }

  .candle {
    position: absolute;
    width: 10px;
    height: 40px;
    background: yellow;
    border-radius: 2px;
    top: -45px;
    transition: opacity 0.6s ease, transform 0.6s ease;
  }

  .flame {
    width: 10px;
    height: 10px;
    background: orange;
    border-radius: 50%;
    margin: 0 auto;
    animation: flicker 0.3s infinite;
    transition: transform 0.6s ease, opacity 0.6s ease;
  }

  /* Alev küçülüp kaybolacak */
  .flame.out {
    transform: scale(0.1);
    opacity: 0;
  }

  /* Mum gövdesi kaybolacak */
  .candle.out {
    opacity: 0;
    transform: scale(0);
  }

  @keyframes flicker {
    0% { transform: scale(1); opacity: 1; }
    50% { transform: scale(1.2); opacity: 0.6; }
    100% { transform: scale(1); opacity: 1; }
  }

  button {
    margin-top: 30px;
    padding: 10px 20px;
    font-size: 16px;
    background: #ff4d88;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .message {
    display: none;
    font-size: 20px;
    color: #333;
    margin: 30px auto;
    max-width: 600px;
    line-height: 1.6;
    font-family: 'Pacifico', cursive;
  }

  .confetti {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 100;
    overflow: hidden;
  }

  .confetti-piece {
    width: 10px;
    height: 10px;
    background: red;
    position: absolute;
    animation: fall 3s linear infinite;
  }

  @keyframes fall {
    0% { transform: translateY(0) rotate(0); opacity: 1; }
    100% { transform: translateY(100vh) rotate(360deg); opacity: 0; }
  }
</style>
</head>
<body>

<div class="cake-container" id="cake-container">
  <div class="cake" id="cake">
    <!-- Alt Kat -->
    <div class="tier bottom">
      <div class="sprinkles" id="sprinkles-bottom"></div>
    </div>

    <!-- Üst Kat -->
    <div class="tier top">
      <div class="sprinkles" id="sprinkles-top"></div>

      <!-- Mumlar -->
      <div class="candle" style="left: 30px;">
        <div class="flame"></div>
      </div>
      <div class="candle" style="left: 70px;">
        <div class="flame"></div>
      </div>
      <div class="candle" style="left: 110px;">
        <div class="flame"></div>
      </div>
    </div>
  </div>
</div>

<button id="blowBtn" onclick="blowCandles()">Blow the Candles</button>

<div class="confetti" id="confetti"></div>

<div class="message" id="message">
  <h2>Happy Birthday!</h2>
  <p>
    I'm really glad I got the chance to know you. You have such a warm, kind energy — being around you just makes people feel comfortable and happy. I hope this new age brings you lots of health, peace, and beautiful memories. May life always treat you kindly, and may your days be full of laughter and love. So happy you were born — really, the world is better with you in it!
  </p>
</div>

<script>
  // Renkli sprinkle'lar
  function createSprinkles(containerId, count = 30) {
    const container = document.getElementById(containerId);
    const colors = ['#ff0', '#0f0', '#00f', '#f00', '#0ff', '#f0f'];
    for (let i = 0; i < count; i++) {
      const dot = document.createElement('div');
      dot.className = 'sprinkle';
      dot.style.left = Math.random() * 100 + '%';
      dot.style.top = Math.random() * 100 + '%';
      dot.style.background = colors[Math.floor(Math.random() * colors.length)];
      container.appendChild(dot);
    }
  }

  createSprinkles('sprinkles-top', 20);
  createSprinkles('sprinkles-bottom', 40);

  function blowCandles() {
    const candles = document.querySelectorAll('.candle');

    candles.forEach((candle, i) => {
      const flame = candle.querySelector('.flame');
      setTimeout(() => {
        // Önce alev küçülüp kaybolur
        flame.classList.add('out');
        // Alev sönünce mum gövdesi de kaybolur
        setTimeout(() => {
          candle.classList.add('out');
        }, 600);
      }, i * 600);
    });

    // Tüm mumlar sönene kadar bekle, sonra mesaj ve konfeti
    setTimeout(() => {
      document.getElementById('blowBtn').style.display = 'none';
      document.getElementById('cake-container').style.display = 'none';
      document.getElementById('message').style.display = 'block';
      launchConfetti();
    }, candles.length * 600 + 800);
  }

  // Konfeti efekti
  function launchConfetti() {
    const confettiContainer = document.getElementById('confetti');
    for (let i = 0; i < 100; i++) {
      const piece = document.createElement('div');
      piece.classList.add('confetti-piece');
      piece.style.left = Math.random() * 100 + 'vw';
      piece.style.backgroundColor = getRandomColor();
      piece.style.animationDuration = 2 + Math.random() * 2 + 's';
      confettiContainer.appendChild(piece);
      setTimeout(() => {
        piece.remove();
      }, 4000);
    }
  }

  // Rastgele renk
  function getRandomColor() {
    const colors = ['#ff0', '#f0f', '#0ff', '#0f0', '#f00', '#00f', '#ffa500'];
    return colors[Math.floor(Math.random() * colors.length)];
  }
</script>

</body>
</html>
