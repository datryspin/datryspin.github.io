<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Valentine</title>
  <style>
    :root{
      --bg1: #ffdee9;
      --bg2: #b5fffc;
      --accent: #ff2d95;
      --card: rgba(255,255,255,0.85);
    }
    html,body{height:100%;margin:0;font-family: "Segoe UI", Roboto, Helvetica, Arial, sans-serif;}
    body{
      display:flex;
      align-items:center;
      justify-content:center;
      background: radial-gradient(circle at 10% 20%, rgba(255,210,220,0.9), transparent 10%),
                  radial-gradient(circle at 90% 80%, rgba(180,255,252,0.9), transparent 12%),
                  linear-gradient(135deg,var(--bg1),var(--bg2));
      overflow:hidden;
    }

    .card{
      width: min(860px, 94vw);
      padding: 36px;
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.12);
      background: var(--card);
      text-align:center;
      position:relative;
      transform: translateY(-4px);
    }

    h1{
      margin: 0 0 12px 0;
      font-size: clamp(20px, 3.8vw, 40px);
      letter-spacing: 1px;
      color: #333;
      text-transform: uppercase;
    }

    .message{
      font-size: clamp(18px, 2.4vw, 26px);
      color: #111;
      font-weight: 600;
      margin-bottom: 18px;
    }

    .sub{
      color:#444;
      margin-bottom: 24px;
      font-size: 14px;
      opacity:0.9;
    }

    .heart-wrap{
      display:flex;
      align-items:center;
      justify-content:center;
      gap:16px;
      margin-bottom:10px;
    }

    .heart{
      width:120px;height:120px;
      background: linear-gradient(180deg,#ff6ea0,#ff2d95);
      clip-path: path("M60 20 C 35 20, 20 35, 20 55 C20 85, 60 102, 60 102 C 60 102, 100 85, 100 55 C 100 35, 85 20, 60 20 Z");
      border-radius: 12px;
      display:inline-block;
      cursor:pointer;
      transform-origin:center;
      box-shadow: 0 8px 18px rgba(255,45,149,0.28), inset 0 -6px 16px rgba(0,0,0,0.06);
      transition: transform 220ms cubic-bezier(.2,.9,.3,1);
      position:relative;
    }

    /* fallback for browsers that don't support clip-path path() */
    .heart::before{
      content:"";
      position:absolute; inset:0;
      background: linear-gradient(180deg,#ff6ea0,#ff2d95);
      border-radius: 50% 50% 44% 44%;
      transform: rotate(-45deg) translateY(2px);
      clip-path: polygon(50% 0%, 100% 38%, 79% 100%, 50% 80%, 21% 100%, 0% 38%);
    }

    .heart:active{ transform: scale(.95) rotate(-6deg); }

    .pulse {
      animation: pulse 1200ms ease-out;
    }
    @keyframes pulse {
      0%{ transform: scale(1); filter: drop-shadow(0 0 0 rgba(255,45,149,0.0)); }
      20%{ transform: scale(1.12); filter: drop-shadow(0 8px 26px rgba(255,45,149,0.18)); }
      50%{ transform: scale(0.96); }
      100%{ transform: scale(1); filter: drop-shadow(0 0 0 rgba(255,45,149,0.0)); }
    }

    .button{
      display:inline-block;
      padding: 10px 18px;
      border-radius: 999px;
      background: white;
      color: var(--accent);
      border: 2px solid rgba(255,45,149,0.12);
      font-weight:700;
      cursor:pointer;
      transition: transform .18s ease, box-shadow .18s;
      box-shadow: 0 6px 18px rgba(0,0,0,0.08);
      text-decoration:none;
    }
    .button:hover{ transform: translateY(-3px); box-shadow: 0 18px 40px rgba(0,0,0,0.12); }

    /* confetti dots */
    .confetti{
      position: absolute;
      inset: 0;
      pointer-events: none;
      overflow: hidden;
    }
    .dot{
      position:absolute;
      width:10px;height:10px;
      border-radius:50%;
      opacity:0;
      transform: translateY(-20vh) scale(.6);
      animation: fall linear forwards;
    }
    @keyframes fall {
      0%{ opacity:1; transform: translateY(-15vh) rotate(0deg) scale(.6); }
      100%{ opacity:1; transform: translateY(110vh) rotate(480deg) scale(1); }
    }

    footer{ margin-top: 18px; font-size: 13px; color: #666; }

    /* small screen tweak */
    @media (max-width:520px){
      .heart{ width:90px;height:90px; }
    }
  </style>
</head>
<body>
  <div class="card" role="main" aria-labelledby="main-title">
    <h1 id="main-title">A special question</h1>

    <div class="message">WILL U BE MY VALENTINES BABE ? <span style="display:block;font-weight:800;">FROM KIPROP</span></div>
    <div class="sub">Click the heart if you say "YES" 💖</div>

    <div class="heart-wrap">
      <div id="heart" class="heart" role="button" aria-pressed="false" aria-label="Heart button - click to accept"></div>
    </div>

    <div>
      <a id="replyBtn" class="button" href="#" style="display:none">Send a reply</a>
    </div>

    <footer>Made with ♥ — Good luck!</footer>

    <div id="confetti" class="confetti" aria-hidden="true"></div>
  </div>

  <script>
    // Simple interaction: pulse heart + confetti when clicked, reveal reply button
    const heart = document.getElementById('heart');
    const confetti = document.getElementById('confetti');
    const replyBtn = document.getElementById('replyBtn');

    function random(min,max){ return Math.random()*(max-min)+min; }

    function spawnConfetti(count=28){
      // clear previous
      confetti.innerHTML = '';
      const colors = ['#ff2d95','#ffb3d9','#ffd166','#6ee7b7','#74c0ff','#a78bfa'];
      for(let i=0;i<count;i++){
        const d = document.createElement('div');
        d.className='dot';
        d.style.background = colors[Math.floor(Math.random()*colors.length)];
        d.style.left = (random(10,90)) + '%';
        d.style.width = d.style.height = (Math.floor(random(6,14))) + 'px';
        d.style.animationDuration = (random(1.2,2.4)).toFixed(2) + 's';
        d.style.animationDelay = (random(0,0.18)).toFixed(2) + 's';
        confetti.appendChild(d);
      }
      // remove after animation
      setTimeout(()=> confetti.innerHTML='', 3000);
    }

    heart.addEventListener('click', ()=>{
      // visual pulse
      heart.classList.remove('pulse');
      // force reflow to restart animation
      // eslint-disable-next-line no-unused-expressions
      void heart.offsetWidth;
      heart.classList.add('pulse');

      // small scale pop
      heart.style.transform = 'scale(1.04)';
      setTimeout(()=> heart.style.transform = '', 260);

      // reveal reply button and confetti
      replyBtn.style.display = 'inline-block';
      replyBtn.href = 'mailto:?subject=Yes!&body=Yes%20I%20will%20be%20your%20valentine%20%F0%9F%92%96';
      replyBtn.textContent = 'I say YES 💓';

      spawnConfetti(34);
      heart.setAttribute('aria-pressed','true');
    });

    // keyboard accessible (space/enter)
    heart.addEventListener('keydown', (e)=>{
      if(e.key === 'Enter' || e.key === ' ') { e.preventDefault(); heart.click(); }
    });

    // optional: animate on load once
    window.addEventListener('load', ()=>{
      setTimeout(()=> {
        heart.classList.add('pulse');
        setTimeout(()=> heart.classList.remove('pulse'), 1200);
      }, 320);
    });
  </script>
</body>
</html>
 
