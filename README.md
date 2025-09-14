# su-okuzu-affet
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <title>Melek & İhsan</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: url("https://i.ibb.co/6t7wYwK/rose-bg.jpg") no-repeat center center fixed;
      background-size: cover;
      color: white;
      text-align: center;
      overflow: hidden;
    }
    #loginScreen, #mainContent { display: none; }
    #loginScreen {
      display: flex; flex-direction: column;
      justify-content: center; align-items: center;
      height: 100vh; background: rgba(0,0,0,0.6);
    }
    input[type="password"] {
      padding: 10px; font-size: 18px; border-radius: 8px; border: none;
    }
    button {
      margin-top: 10px; padding: 10px 20px;
      background: purple; color: white; border: none;
      border-radius: 8px; cursor: pointer; font-size: 16px;
    }
    #player { margin: 20px auto; }
    #bigHeart {
      position: absolute; top: 50%; left: 50%;
      transform: translate(-50%,-50%); font-size: 150px;
      display: none; z-index: 1000;
    }
    .heart {
      position: absolute; font-size: 24px; pointer-events: none;
      animation: float 3s linear forwards;
    }
    @keyframes float {
      from { transform: translateY(0); opacity: 1; }
      to { transform: translateY(-200px); opacity: 0; }
    }
  </style>
</head>
<body>
  <!-- Giriş -->
  <div id="loginScreen">
    <h2>Melek & İhsan 💕</h2>
    <input type="password" id="password" placeholder="Şifre">
    <button onclick="checkPassword()">Giriş</button>
  </div>

  <!-- İçerik -->
  <div id="mainContent">
    <h1>Melek & İhsan 🎶</h1>
    <audio id="audioPlayer" controls></audio><br>
    <button onclick="prevSong()">⏮</button>
    <button onclick="playPause()">⏯</button>
    <button onclick="nextSong()">⏭</button>
    <button onclick="triggerHeartAnimation()">💚 Kalpler</button>

    <div id="bigHeart">❤️</div>
  </div>

  <script>
    const PASSWORD = "melekihsan";

    function checkPassword() {
      const val = document.getElementById("password").value;
      if(val === PASSWORD) {
        document.getElementById("loginScreen").style.display = "none";
        document.getElementById("mainContent").style.display = "block";
        loadSong(currentIndex);
      } else {
        alert("Yanlış şifre!");
      }
    }

    // Müzik listesi
    const playlist = [
      {src: "https://www2.cs.uic.edu/~i101/SoundFiles/BabyElephantWalk60.wav"},
      {src: "https://www2.cs.uic.edu/~i101/SoundFiles/PinkPanther30.wav"}
    ];
    let currentIndex = 0;
    const audio = document.getElementById("audioPlayer");

    function loadSong(i) {
      audio.src = playlist[i].src;
    }
    function playPause() {
      if(audio.paused){ audio.play(); } else { audio.pause(); }
    }
    function nextSong() {
      currentIndex = (currentIndex+1)%playlist.length;
      loadSong(currentIndex); audio.play();
    }
    function prevSong() {
      currentIndex = (currentIndex-1+playlist.length)%playlist.length;
      loadSong(currentIndex); audio.play();
    }

    // Kalplerden büyük kalp animasyonu
    function triggerHeartAnimation(){
      const colors = ["💜","💚","❤️"];
      let hearts = [];
      for(let i=0;i<30;i++){
        let h = document.createElement("div");
        h.className = "heart";
        h.textContent = colors[Math.floor(Math.random()*colors.length)];
        h.style.left = Math.random()*window.innerWidth+"px";
        h.style.top = Math.random()*window.innerHeight+"px";
        document.body.appendChild(h);
        hearts.push(h);
        setTimeout(()=>{
          h.style.transition = "all 2s ease";
          h.style.left = (window.innerWidth/2)+"px";
          h.style.top = (window.innerHeight/2)+"px";
          h.style.opacity = 0;
        },100);
      }
      setTimeout(()=>{
        hearts.forEach(h=>h.remove());
        document.getElementById("bigHeart").style.display="block";
        setTimeout(()=>{document.getElementById("bigHeart").style.display="none";},2000);
      },2500);
    }

    // Şarkı başladığında otomatik animasyon
    audio.addEventListener("play", ()=>{
      triggerHeartAnimation();
    });
  </script>
</body>
</html>
