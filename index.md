<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Play BNB — Showcase</title>
  <style>
    :root{
      --bg: #000000;
      --card-bg: rgba(0,0,0,0.45);
      --gold-1: #ffd84d;
      --gold-2: #ffb300;
      --muted: rgba(255,255,255,0.75);
      --accent: #7e3bff;
    }

    /* Reset */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    html,body { height: 100%; }
    body {
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background: var(--bg);
      color: #fff;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 28px;
      overflow: hidden;
      line-height: 1.2;
    }

    /* Background animated gold dots */
    .bg {
      position: fixed;
      inset: 0;
      z-index: 0;
      pointer-events: none;
      overflow: hidden;
    }
    .dot {
      position: absolute;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 25%, var(--gold-1), var(--gold-2));
      box-shadow: 0 0 8px rgba(255,215,0,0.45), inset 0 0 3px rgba(255,255,255,0.06);
      opacity: 0.95;
      will-change: transform, opacity;
      mix-blend-mode: screen;
    }
    @keyframes floatY {
      0% { transform: translateY(0) scale(1); }
      50% { transform: translateY(-14px) scale(1.06); }
      100% { transform: translateY(0) scale(1); }
    }
    @keyframes driftX {
      0% { transform: translateX(0); }
      50% { transform: translateX(10px); }
      100% { transform: translateX(0); }
    }

    /* Main container & card */
    .wrap {
      position: relative;
      z-index: 2;
      width: min(1100px, 96vw);
      max-width: 1100px;
      margin: 0 auto;
    }
    .card {
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius: 14px;
      padding: 28px;
      border: 1px solid rgba(255,255,255,0.04);
      box-shadow: 0 24px 80px rgba(0,0,0,0.75);
    }

    /* Header: logo + title */
    .header {
      display: flex;
      align-items: center;
      gap: 18px;
    }

    /* Logo: uses provided image as mask so the logo shape is filled with gold.
       Save your provided logo as "playbnb-logo.png" next to this HTML file. */
    .logo {
      width: 98px;
      height: 98px;
      border-radius: 18px;
      display: inline-block;
      background: radial-gradient(circle at 30% 25%, var(--gold-1), var(--gold-2));
      -webkit-mask-size: contain;
      -webkit-mask-repeat: no-repeat;
      -webkit-mask-position: center;
      mask-size: contain;
      mask-repeat: no-repeat;
      mask-position: center;
      box-shadow: 0 8px 28px rgba(255, 180, 20, 0.08), inset 0 1px 0 rgba(255,255,255,0.06);
      border: 1px solid rgba(255,215,0,0.16);
      flex-shrink: 0;
    }
    /* apply the PNG as mask (works in modern browsers). Fallback shows the image inside. */
    .logo.masked {
      -webkit-mask-image: url('playbnb-logo.png');
      mask-image: url('playbnb-logo.png');
      background-color: transparent; /* keep gradient inside mask only */
      background: radial-gradient(circle at 30% 25%, var(--gold-1), var(--gold-2));
    }
    /* fallback image inside the logo container (for browsers that don't support mask or if mask fails) */
    .logo img {
      width: 70%;
      height: 70%;
      object-fit: contain;
      display: block;
      margin: auto;
      filter: drop-shadow(0 6px 14px rgba(0,0,0,0.6));
      background: transparent;
    }

    .title {
      font-weight: 900;
      color: var(--gold-1);
      font-size: clamp(28px, 4.6vw, 56px);
      text-shadow: 0 10px 30px rgba(125,54,255,0.06);
      letter-spacing: 0.02em;
    }
    .subtitle {
      margin-top: 6px;
      color: var(--muted);
      font-size: 14px;
    }

    /* Marquee (logos moving right-to-left) */
    .marquee-wrap {
      margin-top: 18px;
      overflow: hidden;
      padding: 10px 8px;
      border-radius: 12px;
      background: rgba(0,0,0,0.35);
      border: 1px solid rgba(255,255,255,0.02);
    }
    .marquee {
      display: flex;
      gap: 26px;
      align-items: center;
      padding: 6px 0;
      will-change: transform;
      /* we animate with JS to calculate smooth duration */
    }
    .marquee .item {
      display: flex;
      gap: 10px;
      align-items: center;
      padding: 8px 14px;
      border-radius: 999px;
      background: linear-gradient(90deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border: 1px solid rgba(255,255,255,0.02);
      color: var(--muted);
      font-weight: 700;
      min-width: 170px;
      justify-content: center;
    }
    .ex-logo {
      width: 36px;
      height: 36px;
      border-radius: 8px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, rgba(255,255,255,0.03), rgba(0,0,0,0.06));
      flex-shrink: 0;
    }
    .ex-logo img { width: 22px; height: 22px; object-fit: contain; }

    /* small note */
    .note {
      margin-top: 14px;
      text-align: center;
      color: var(--muted);
      font-size: 13px;
    }

    /* Responsive tweaks */
    @media (max-width: 820px) {
      .title { font-size: clamp(22px, 6.6vw, 42px); }
      .logo { width: 76px; height: 76px; border-radius: 14px; }
      .marquee .item { min-width: 140px; padding: 8px 10px; font-size: 14px; gap: 8px; }
    }
  </style>
</head>
<body>
  <div class="bg" id="bg"></div>

  <div class="wrap">
    <div class="card" role="main" aria-label="Play BNB showcase">
      <header class="header" role="banner">
        <!-- Replace 'playbnb-logo.png' with the provided logo file (same folder).
             Mask is applied so the logo shape is filled with gold color. -->
        <div class="logo masked" aria-hidden="true"></div>

        <div>
          <div class="title">Play BNB</div>
          <div class="subtitle">The token for Web3 gaming & rewards</div>
        </div>
      </header>

      <!-- Marquee container -->
      <div class="marquee-wrap" aria-hidden="false">
        <div id="marquee" class="marquee" aria-live="off" aria-hidden="false">
          <!-- Sample exchange items. Replace img src with real logos if you have. -->
          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><circle cx='11' cy='11' r='10' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='10' text-anchor='middle' fill='%23000'>PS</text></svg>" alt="Pancake"/></div><div>PancakeSwap</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><rect width='22' height='22' rx='4' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='9' text-anchor='middle' fill='%23000'>MEX</text></svg>" alt="MEXC"/></div><div>MEXC</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><rect width='22' height='22' rx='4' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='8' text-anchor='middle' fill='%23000'>LKB</text></svg>" alt="LBank"/></div><div>LBank</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><circle cx='11' cy='11' r='10' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='9' text-anchor='middle' fill='%23000'>UNI</text></svg>" alt="Uniswap"/></div><div>Uniswap</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><circle cx='11' cy='11' r='10' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='8' text-anchor='middle' fill='%23000'>BN</text></svg>" alt="Binance"/></div><div>Binance</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><rect width='22' height='22' rx='4' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='8' text-anchor='middle' fill='%23000'>OTH</text></svg>" alt="Others"/></div><div>and more...</div></div>

          <!-- Duplicate to create seamless loop -->
          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><circle cx='11' cy='11' r='10' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='10' text-anchor='middle' fill='%23000'>PS</text></svg>" alt="Pancake"/></div><div>PancakeSwap</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><rect width='22' height='22' rx='4' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='9' text-anchor='middle' fill='%23000'>MEX</text></svg>" alt="MEXC"/></div><div>MEXC</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><rect width='22' height='22' rx='4' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='8' text-anchor='middle' fill='%23000'>LKB</text></svg>" alt="LBank"/></div><div>LBank</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><circle cx='11' cy='11' r='10' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='9' text-anchor='middle' fill='%23000'>UNI</text></svg>" alt="Uniswap"/></div><div>Uniswap</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><circle cx='11' cy='11' r='10' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='8' text-anchor='middle' fill='%23000'>BN</text></svg>" alt="Binance"/></div><div>Binance</div></div>

          <div class="item"><div class="ex-logo"><img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='22' height='22'><rect width='22' height='22' rx='4' fill='%23FFD84D'/><text x='11' y='15' font-family='Arial' font-size='8' text-anchor='middle' fill='%23000'>OTH</text></svg>" alt="Others"/></div><div>and more...</div></div>
        </div>
      </div>

      <div class="note">Replace the "playbnb-logo.png" file and the exchange images with your real SVG/PNG logos for best visual fidelity.</div>
    </div>
  </div>

  <script>
    /* Create animated gold dots in background */
    (function createDots(){
      const container = document.getElementById('bg');
      const COUNT = 48;
      const minSize = 4, maxSize = 12;
      for(let i=0;i<COUNT;i++){
        const d = document.createElement('div');
        d.className = 'dot';
        const size = Math.floor(Math.random()*(maxSize-minSize+1))+minSize;
        d.style.width = size + 'px';
        d.style.height = size + 'px';
        d.style.left = (Math.random()*100) + 'vw';
        d.style.top  = (Math.random()*100) + 'vh';

        const durY = (1.4 + Math.random()*2.6).toFixed(2);
        const durX = (1.8 + Math.random()*3.2).toFixed(2);
        const delay = (Math.random()*1.6).toFixed(2);

        d.style.animation = `floatY ${durY}s ease-in-out ${delay}s infinite alternate, driftX ${durX}s ease-in-out ${delay/2}s infinite alternate`;
        d.style.opacity = (0.6 + Math.random()*0.35).toFixed(2);
        container.appendChild(d);
      }
    })();

    /* Marquee animation: calculate width and set a duration that feels natural.
       Items are duplicated in HTML so we can translate by half width and loop seamlessly.
    */
    (function startMarquee(){
      const marquee = document.getElementById('marquee');
      // measure total width of items
      requestAnimationFrame(()=> {
        const items = marquee.children;
        let totalWidth = 0;
        for (let i=0;i<items.length;i++){
          totalWidth += items[i].getBoundingClientRect().width + 26; // include gap
        }
        // translate by half the total width (because we duplicated the list)
        // choose speed: px per second
        const pxPerSecond = 70; // adjust to make it faster/slower
        const duration = Math.max(10, (totalWidth/2) / pxPerSecond);
        marquee.style.animation = `marqueeMove ${duration}s linear infinite`;
        // define keyframes dynamically to translate by half width
        const style = document.createElement('style');
        style.textContent = `@keyframes marqueeMove { 0% { transform: translateX(0); } 100% { transform: translateX(-${totalWidth/2}px); } }`;
        document.head.appendChild(style);
      });
    })();
  </script>
</body>
</html>
