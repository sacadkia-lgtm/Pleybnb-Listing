<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Play BNB — Exchanges Marquee (Large Logos, Slow)</title>

  <style>
    :root{
      --bg:#000;
      --card-bg:rgba(255,255,255,0.02);
      --gold-1:#ffd84d;
      --gold-2:#ffb300;
      --muted:rgba(255,255,255,0.75);
      --marquee-speed-px-per-sec:40;
    }
    *{box-sizing:border-box;margin:0;padding:0}
    html,body{height:100%}
    body{
      font-family:Inter,system-ui,-apple-system,"Segoe UI",Roboto,Arial;
      background:var(--bg);
      color:#fff;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:28px;
      overflow:hidden;
    }

    .bg{position:fixed;inset:0;z-index:0;pointer-events:none}
    .dot{
      position:absolute;border-radius:50%;
      background:radial-gradient(circle at 30% 25%,var(--gold-1),var(--gold-2));
      box-shadow:0 0 8px rgba(255,215,0,.45);
      mix-blend-mode:screen;
    }
    @keyframes floatY{0%{transform:translateY(0)}50%{transform:translateY(-12px)}100%{transform:translateY(0)}}
    @keyframes driftX{0%{transform:translateX(0)}50%{transform:translateX(8px)}100%{transform:translateX(0)}}

    .wrap{position:relative;z-index:2;width:min(1200px,96vw)}
    .card{
      background:linear-gradient(180deg,rgba(255,255,255,.02),rgba(255,255,255,.01));
      border-radius:14px;
      padding:30px;
      border:1px solid rgba(255,255,255,.04);
      box-shadow:0 28px 80px rgba(0,0,0,.75);
    }

    .header{display:flex;gap:18px;margin-bottom:18px}
    .logo-box{
      width:110px;height:110px;border-radius:18px;
      background:radial-gradient(circle at 30% 25%,var(--gold-1),var(--gold-2));
      display:flex;align-items:center;justify-content:center;
    }
    .brand-title{font-weight:900;color:var(--gold-1);font-size:clamp(30px,4.6vw,56px)}
    .subtitle{color:var(--muted);font-size:14px;margin-top:6px}

    .marquee-wrap{
      overflow:hidden;border-radius:12px;
      background:rgba(0,0,0,.35);
      padding:18px 12px;
    }
    .marquee{display:flex;gap:34px;will-change:transform}
    .marquee-track{display:flex;gap:34px}
    .marquee-item{
      min-width:260px;
      padding:10px 16px;
      border-radius:12px;
      display:flex;gap:12px;align-items:center;justify-content:center;
      background:linear-gradient(90deg,rgba(255,255,255,.01),rgba(255,255,255,.005));
      font-weight:800;color:var(--muted);
    }
    .marquee-logo{
      width:120px;height:120px;border-radius:14px;
      background:var(--gold-1);
      display:flex;align-items:center;justify-content:center;
      overflow:hidden;
    }
    .marquee-logo svg,.marquee-logo img{
      width:78%;height:78%;object-fit:contain;
    }
    .marquee-logo img{
      filter:grayscale(1) contrast(1.1) brightness(1.2)
             sepia(.9) saturate(2) hue-rotate(-10deg);
      mix-blend-mode:multiply;
    }
  </style>
</head>

<body>
<div class="bg" id="bg"></div>

<div class="wrap">
  <div class="card">
    <div class="header">
      <div class="logo-box"></div>
      <div>
        <div class="brand-title">Play BNB</div>
        <div class="subtitle">Highlighted Exchanges</div>
      </div>
    </div>

    <div class="marquee-wrap">
      <div id="marquee" class="marquee"></div>
    </div>
  </div>
</div>

<script>
/* ====== LOGOS (OFFICIAL EXCHANGES) ====== */
const logos = [
  // CEX
  { name:'Binance',   url:'https://www.binance.com/img/binance_logo.svg' },
  { name:'Coinbase',  url:'https://www.coinbase.com/assets/press/coinbase-logo-black.svg' },
  { name:'OKX',       url:'https://static.okx.com/cdn/assets/imgs/221/6F1B5F4A0E0B4E0E.png' },
  { name:'KuCoin',    url:'https://assets.staticimg.com/cms/media/kucoin-logo.png' },
  { name:'Bybit',     url:'https://www.bybit.com/app/themes/bybit/assets/images/logo.svg' },

  // DEX
  { name:'Uniswap',      url:'https://uniswap.org/cdn/assets/brand-assets/uniswap-logo.svg' },
  { name:'PancakeSwap', url:'https://pancakeswap.finance/images/logo.png' },
  { name:'SushiSwap',   url:'https://www.sushi.com/images/logo.svg' },
  { name:'Curve',       url:'https://raw.githubusercontent.com/curvefi/curve-assets/main/logos/curve.png' },
  { name:'dYdX',        url:'https://dydx.exchange/brand/dydx-logo.svg' }
];

const marquee=document.getElementById('marquee');

function buildTrack(){
  const track=document.createElement('div');
  track.className='marquee-track';
  logos.forEach(item=>{
    const el=document.createElement('div');
    el.className='marquee-item';
    const box=document.createElement('div');
    box.className='marquee-logo';
    const img=document.createElement('img');
    img.src=item.url;
    img.alt=item.name;
    box.appendChild(img);
    const label=document.createElement('div');
    label.textContent=item.name;
    el.appendChild(box);
    el.appendChild(label);
    track.appendChild(el);
  });
  return track;
}

const a=buildTrack();
const b=a.cloneNode(true);
marquee.appendChild(a);
marquee.appendChild(b);

requestAnimationFrame(()=>{
  const w=a.getBoundingClientRect().width;
  const speed=40;
  const dur=w/speed;
  const style=document.createElement('style');
  style.innerHTML=`@keyframes marquee{0%{transform:translateX(0)}100%{transform:translateX(-${w}px)}}`;
  document.head.appendChild(style);
  marquee.style.animation=`marquee ${dur}s linear infinite`;
});

/* background dots */
const bg=document.getElementById('bg');
for(let i=0;i<48;i++){
  const d=document.createElement('div');
  d.className='dot';
  const s=4+Math.random()*8;
  d.style.width=s+'px';d.style.height=s+'px';
  d.style.left=Math.random()*100+'vw';
  d.style.top=Math.random()*100+'vh';
  d.style.animation=`floatY ${1.5+Math.random()*3}s ease-in-out infinite,
                     driftX ${2+Math.random()*3}s ease-in-out infinite`;
  bg.appendChild(d);
}
</script>
</body>
</html>
