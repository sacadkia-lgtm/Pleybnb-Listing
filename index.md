
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Play BNB — Exchanges Marquee (Large Logos, Slow)</title>
  <meta name="description" content="Large exchange logos, gold-tinted, slow right-to-left marquee. Place logos in /logos or use remote URLs in the script.">

  <style>
    :root{
      --bg: #000;
      --card-bg: rgba(255,255,255,0.02);
      --gold-1: #ffd84d;
      --gold-2: #ffb300;
      --muted: rgba(255,255,255,0.75);
      --marquee-speed-px-per-sec: 40; /* smaller = slower marquee */
    }

    /* Reset */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    html,body { height: 100%; }
    body {
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background: var(--bg);
      color: #fff;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:28px;
      overflow:hidden;
    }

    /* Background small gold dots */
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
      opacity: 0.9;
      will-change: transform, opacity;
      mix-blend-mode: screen;
    }
    @keyframes floatY {
      0% { transform: translateY(0); }
      50% { transform: translateY(-12px) scale(1.02); }
      100% { transform: translateY(0); }
    }
    @keyframes driftX {
      0% { transform: translateX(0); }
      50% { transform: translateX(8px); }
      100% { transform: translateX(0); }
    }

    /* Page card */
    .wrap {
      position: relative;
      z-index: 2;
      width: min(1200px, 96vw);
      max-width: 1200px;
      margin: 0 auto;
    }
    .card {
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius: 14px;
      padding: 30px;
      border: 1px solid rgba(255,255,255,0.04);
      box-shadow: 0 28px 80px rgba(0,0,0,0.75);
    }

    /* Header */
    .header {
      display:flex;
      align-items:center;
      gap:18px;
      margin-bottom:18px;
    }
    .logo-box {
      width: 110px;
      height: 110px;
      border-radius: 18px;
      display:inline-flex;
      align-items:center;
      justify-content:center;
      background: radial-gradient(circle at 30% 25%, var(--gold-1), var(--gold-2));
      box-shadow: 0 8px 28px rgba(255,180,20,0.08), inset 0 1px 0 rgba(255,255,255,0.06);
      border: 1px solid rgba(255,215,0,0.14);
      flex-shrink:0;
    }
    .brand-title {
      font-weight: 900;
      color: var(--gold-1);
      font-size: clamp(30px, 4.6vw, 56px);
      letter-spacing: 0.01em;
    }
    .subtitle {
      color: var(--muted);
      font-size: 14px;
      margin-top: 6px;
    }

    /* Marquee */
    .marquee-wrap {
      overflow: hidden;
      border-radius: 12px;
      background: rgba(0,0,0,0.35);
      padding: 18px 12px;
      border: 1px solid rgba(255,255,255,0.02);
    }
    .marquee {
      display:flex;
      align-items:center;
      gap: 34px;
      will-change: transform;
      /* animation is set dynamically by JS based on content width */
    }
    .marquee-track { display:flex; gap:34px; align-items:center; }
    .marquee-item {
      display:flex;
      align-items:center;
      gap:12px;
      min-width: 260px;
      justify-content:center;
      padding: 10px 16px;
      border-radius: 12px;
      background: linear-gradient(90deg, rgba(255,255,255,0.01), rgba(255,255,255,0.005));
      color: var(--muted);
      font-weight: 800;
    }

    /* Large logo container per item */
    .marquee-logo {
      width: 120px;
      height: 120px;
      border-radius: 14px;
      display:inline-flex;
      align-items:center;
      justify-content:center;
      background: var(--gold-1);
      overflow: hidden;
      flex-shrink:0;
    }

    /* For inline SVG logos we recolor them via CSS; when injecting SVG into DOM the internal fills are changed to gold */
    .marquee-logo svg { width: 78%; height: 78%; display:block; }

    /* For raster images (png) we apply filters to approximate gold tint and put them on gold circle background */
    .marquee-logo img {
      width: 78%;
      height: 78%;
      object-fit: contain;
      filter: grayscale(1) contrast(1.1) brightness(1.2) sepia(0.9) saturate(2) hue-rotate(-10deg);
      mix-blend-mode: multiply;
    }

    /* Small note */
    .note { margin-top:14px; color:var(--muted); font-size:13px; text-align:center; }

    /* Responsive */
    @media (max-width:900px) {
      .marquee-item { min-width: 180px; gap:8px; padding: 8px 10px; }
      .marquee-logo { width: 86px; height: 86px; }
      .marquee-logo svg, .marquee-logo img { width: 72%; height:72%;}
      .brand-title { font-size: clamp(22px, 8vw, 38px); }
    }
  </style>
</head>
<body>
  <div class="bg" id="bg"></div>

  <div class="wrap">
    <div class="card" role="main" aria-label="Play BNB exchanges">
      <div class="header">
        <div class="logo-box" id="playLogoBox" aria-hidden="true">
          <!-- Fallback: if you place playbnb-logo.svg/png into /logos the script will use it as mask for gold -->
          <img id="playLogoFallback" src="" alt="Play BNB" style="display:none; width:64%; height:64%; object-fit:contain;">
        </div>

        <div>
          <div class="brand-title">Play BNB</div>
          <div class="subtitle">Highlighted Exchanges</div>
        </div>
      </div>

      <div class="marquee-wrap" aria-hidden="false">
        <!-- marquee inner: JS will populate .marquee element with track content and duplicate for seamless looping -->
        <div id="marquee" class="marquee" aria-live="off"></div>
      </div>

      <div class="note">Place official logos (SVG preferred) in a /logos folder with names listed in the script or provide remote URLs in the config below.</div>
    </div>
  </div>

  <script>
    /*
      Professional, flexible marquee loader:
      - Provide a list of logos below. Each item should be either:
        { name: 'Binance', file: 'logos/binance.svg' }  (relative path)
        OR
        { name: 'Binance', url: 'https://.../binance.svg' } (remote URL)
      - SVG files will be inlined (fetched and injected) and recolored to gold for consistent look.
      - PNG/JPG will be placed inside a gold background and CSS filter will approximate gold tint.
      - Marquee speed is computed based on content width and CSS variable --marquee-speed-px-per-sec.
      - Make logos large by default; adjust .marquee-logo width/height in CSS if you want different size.
    */

    const logos = [
      // centralized exchanges
      { name: 'Binance', file: 'logos/binance.svg' },
      { name: 'Coinbase', file: 'logos/coinbase.svg' },
      { name: 'OKX', file: 'logos/okx.svg' },
      { name: 'KuCoin', file: 'logos/kucoin.svg' },
      { name: 'Bybit', file: 'logos/bybit.svg' },

      // DEX
      { name: 'Uniswap', file: 'logos/uniswap.svg' },
      { name: 'PancakeSwap', file: 'logos/pancakeswap.svg' },
      { name: 'SushiSwap', file: 'logos/sushiswap.svg' },
      { name: 'Curve', file: 'logos/curve.svg' },
      { name: 'dYdX', file: 'logos/dydx.svg' },

      // Add more items or remote URLs as needed:
      // { name: 'SomeExchange', url: 'https://example.com/logo.svg' }
    ];

    const marqueeEl = document.getElementById('marquee');

    // Helper: fetch text (SVG) with timeout
    async function fetchText(url, timeout = 10000) {
      const controller = new AbortController();
      const id = setTimeout(() => controller.abort(), timeout);
      try {
        const res = await fetch(url, { signal: controller.signal });
        clearTimeout(id);
        if (!res.ok) throw new Error('Fetch failed: ' + res.status);
        return await res.text();
      } catch (e) {
        clearTimeout(id);
        console.warn('Failed to fetch', url, e);
        return null;
      }
    }

    // Create one marquee track (sequence of items), duplicate later for smooth loop
    async function buildTrack() {
      const track = document.createElement('div');
      track.className = 'marquee-track';

      for (const item of logos) {
        const el = document.createElement('div');
        el.className = 'marquee-item';

        // logo box
        const logoBox = document.createElement('div');
        logoBox.className = 'marquee-logo';
        logoBox.setAttribute('aria-hidden', 'true');

        // label
        const label = document.createElement('div');
        label.textContent = item.name;

        // Decide source: file or url
        const src = item.file || item.url;
        if (!src) {
          // fallback placeholder (text)
          const placeholder = document.createElement('div');
          placeholder.style.color = 'rgba(0,0,0,0.6)';
          placeholder.style.fontWeight = '800';
          placeholder.textContent = item.name[0] || '?';
          logoBox.appendChild(placeholder);
        } else {
          // Try to load and inject SVG if possible, otherwise use <img>
          const lower = src.split('?')[0].toLowerCase();
          const isSVG = lower.endsWith('.svg');

          if (isSVG) {
            const svgText = await fetchText(src);
            if (svgText) {
              // parse SVG and clean/adjust fills to gold
              // create a container and insert SVG markup
              const wrapper = document.createElement('div');
              wrapper.innerHTML = svgText.trim();
              const svgEl = wrapper.querySelector('svg');
              if (svgEl) {
                // remove hardcoded fills and set a uniform gold color where possible
                // This tries to replace fill/stroke attributes, but won't always reach every internal style.
                svgEl.querySelectorAll('[fill]').forEach(n => n.setAttribute('fill', 'currentColor'));
                svgEl.querySelectorAll('[stroke]').forEach(n => n.setAttribute('stroke', 'currentColor'));
                svgEl.style.color = 'var(--gold-1)'; // use currentColor to tint
                svgEl.style.width = '78%';
                svgEl.style.height = '78%';
                // ensure viewbox exists
                svgEl.setAttribute('preserveAspectRatio', 'xMidYMid meet');
                logoBox.appendChild(svgEl);
              } else {
                // fallback to <img> if parsing failed
                const img = document.createElement('img');
                img.src = src;
                img.alt = item.name;
                logoBox.appendChild(img);
              }
            } else {
              // fetch failed; try to use <img> tag (may be blocked by CORS)
              const img = document.createElement('img');
              img.src = src;
              img.alt = item.name;
              logoBox.appendChild(img);
            }
          } else {
            // not an SVG -> use <img> with CSS filter tint
            const img = document.createElement('img');
            img.src = src;
            img.alt = item.name;
            logoBox.appendChild(img);
          }
        }

        el.appendChild(logoBox);
        el.appendChild(label);
        track.appendChild(el);
      }

      return track;
    }

    // Build marquee: create two tracks (original + duplicate) for seamless loop
    (async function initMarquee(){
      // Build track
      const trackA = await buildTrack();
      const trackB = trackA.cloneNode(true);

      // Append both tracks inside marqueeEl (side-by-side)
      marqueeEl.appendChild(trackA);
      marqueeEl.appendChild(trackB);

      // Once appended, measure width and set animation using CSS keyframes (translateX)
      // compute total width to move = width of one track
      requestAnimationFrame(() => {
        const trackWidth = trackA.getBoundingClientRect().width;
        // px per second from CSS variable
        const pxPerSec = parseFloat(getComputedStyle(document.documentElement).getPropertyValue('--marquee-speed-px-per-sec')) || 40;
        const durationSec = Math.max(10, trackWidth / pxPerSec);

        // Create keyframes dynamically - translate by trackWidth (move left)
        const style = document.createElement('style');
        style.innerHTML = `
          @keyframes marqueeScroll {
            0% { transform: translateX(0); }
            100% { transform: translateX(-${trackWidth}px); }
          }
        `;
        document.head.appendChild(style);

        // Apply animation to marquee element
        marqueeEl.style.animation = `marqueeScroll ${durationSec}s linear infinite`;

        // Small accessibility: pause on hover
        marqueeEl.addEventListener('mouseenter', () => marqueeEl.style.animationPlayState = 'paused');
        marqueeEl.addEventListener('mouseleave', () => marqueeEl.style.animationPlayState = 'running');
      });
    })();

    /* Create background dots */
    (function createDots(){
      const container = document.getElementById('bg');
      const COUNT = 48; // change to increase density
      const min = 4, max = 12;
      for (let i=0;i<COUNT;i++){
        const d = document.createElement('div');
        d.className = 'dot';
        const s = Math.floor(Math.random()*(max-min+1))+min;
        d.style.width = s + 'px';
        d.style.height = s + 'px';
        d.style.left = (Math.random()*100) + 'vw';
        d.style.top  = (Math.random()*100) + 'vh';
        const durY = (1.6 + Math.random()*2.8).toFixed(2);
        const durX = (2.0 + Math.random()*3.6).toFixed(2);
        const delay = (Math.random()*1.6).toFixed(2);
        d.style.animation = `floatY ${durY}s ease-in-out ${delay}s infinite alternate, driftX ${durX}s ease-in-out ${delay/2}s infinite alternate`;
        d.style.opacity = (0.6 + Math.random()*0.35).toFixed(2);
        container.appendChild(d);
      }
    })();

    /* Notes for you:
       - Place official logos into a folder named "logos" relative to this HTML file,
         using the filenames listed in the `logos` array above (e.g., logos/binance.svg).
       - SVG is preferred: the script inlines SVG and recolors it via currentColor
         (so the icon appears gold).
       - For PNG files the script will use CSS filters to approximate gold tint.
       - To use remote images, replace { file: 'logos/xxx.svg' } entries with { url: 'https://...' }.
       - Adjust the marquee speed by editing --marquee-speed-px-per-sec in :root (lower value = slower).
    */
  </script>
</body>
</html>
