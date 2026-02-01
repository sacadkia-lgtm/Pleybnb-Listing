<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Play BNB — Exchanges Marquee</title>

<style>
:root{
  --bg:#000;
  --gold-1:#ffd84d;
  --gold-2:#ffb300;
  --muted:rgba(255,255,255,.75);
  --marquee-speed-px-per-sec:40;
}

*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%}

body{
  font-family:Inter,system-ui,Arial;
  background:var(--bg);
  color:#fff;
  display:flex;
  align-items:center;
  justify-content:center;
  padding:28px;
  overflow:hidden;
}

/* Background dots */
.bg{position:fixed;inset:0;z-index:0;pointer-events:none}
.dot{
  position:absolute;
  border-radius:50%;
  background:radial-gradient(circle,var(--gold-1),var(--gold-2));
  opacity:.7;
  animation:float 4s ease-in-out infinite alternate;
}
@keyframes float{
  from{transform:translateY(0)}
  to{transform:translateY(-14px)}
}

/* Card */
.wrap{z-index:2;width:min(1200px,96vw)}
.card{
  background:rgba(255,255,255,.02);
  border-radius:16px;
  padding:30px;
  box-shadow:0 30px 90px rgba(0,0,0,.8);
}

/* Header */
.header{display:flex;gap:18px;align-items:center;margin-bottom:20px}
.logo-box{
  width:110px;height:110px;border-radius:18px;
  background:radial-gradient(circle,var(--gold-1),var(--gold-2));
  display:flex;align-items:center;justify-content:center;
}
.brand-title{
  font-size:clamp(30px,4vw,54px);
  font-weight:900;
  color:var(--gold-1)
}
.subtitle{font-size:14px;color:var(--muted)}

/* Marquee */
.marquee-wrap{
  overflow:hidden;
  border-radius:14px;
  padding:18px;
  background:rgba(0,0,0,.4);
}

.marquee{
  display:flex;
  gap:34px;
  will-change:transform;
}

.marquee-track{
  display:flex;
  gap:34px;
}

.marquee-item{
  min-width:260px;
  display:flex;
  align-items:center;
  gap:14px;
  padding:12px 18px;
  border-radius:14px;
  background:rgba(255,255,255,.03);
  font-weight:800;
  color:var(--muted);
}

.marquee-logo{
  width:120px;height:120px;
  border-radius:16px;
  background:var(--gold-1);
  display:flex;
  align-items:center;
  justify-content:center;
  overflow:hidden;
}

.marquee-logo svg{
  width:78%;
  height:78%;
  color:var(--gold-1);
}

.marquee-logo img{
  width:78%;
  height:78%;
  object-fit:contain;
  filter:grayscale(1) contrast(1.1) brightness(1.2)
          sepia(.9) saturate(2);
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
/* ===== LOGOS CONFIG (NO CODE CHANGE NEEDED LATER) ===== */
const logos = [
  { name:'Binance', file:'logos/binance.svg' },
  { name:'Coinbase', file:'logos/coinbase.svg' },
  { name:'OKX', file:'logos/okx.svg' },
  { name:'Bybit', file:'logos/bybit.svg' },
  { name:'KuCoin', file:'logos/kucoin.svg' },
  { name:'Uniswap', file:'logos/uniswap.svg' },
  { name:'PancakeSwap', file:'logos/pancakeswap.svg' }
];

const marquee = document.getElementById('marquee');

/* ===== SVG FETCH & INLINE ===== */
async function fetchSVG(url){
  try{
    const r = await fetch(url);
    if(!r.ok) return null;
    return await r.text();
  }catch{ return null }
}

async function buildTrack(){
  const track=document.createElement('div');
  track.className='marquee-track';

  for(const item of logos){
    const box=document.createElement('div');
    box.className='marquee-item';

    const logo=document.createElement('div');
    logo.className='marquee-logo';

    const label=document.createElement('div');
    label.textContent=item.name;

    const svgText=await fetchSVG(item.file);
    if(svgText){
      const wrap=document.createElement('div');
      wrap.innerHTML=svgText;
      const svg=wrap.querySelector('svg');
      if(svg){
        svg.removeAttribute('width');
        svg.removeAttribute('height');
        svg.querySelectorAll('*').forEach(el=>{
          el.setAttribute('fill','currentColor');
          el.setAttribute('stroke','currentColor');
        });
        logo.appendChild(svg);
      }
    }else{
      const img=document.createElement('img');
      img.src=item.file;
      logo.appendChild(img);
    }

    box.appendChild(logo);
    box.appendChild(label);
    track.appendChild(box);
  }
  return track;
}

(async()=>{
  const t1=await buildTrack();
  const t2=t1.cloneNode(true);
  marquee.append(t1,t2);

  requestAnimationFrame(()=>{
    const w=t1.getBoundingClientRect().width;
    const speed=parseFloat(
      getComputedStyle(document.documentElement)
      .getPropertyValue('--marquee-speed-px-per-sec')
    );
    const dur=Math.max(10,w/speed);

    const s=document.createElement('style');
    s.innerHTML=`
      @keyframes marqueeScroll{
        from{transform:translateX(0)}
        to{transform:translateX(-${w}px)}
      }
    `;
    document.head.appendChild(s);
    marquee.style.animation=`marqueeScroll ${dur}s linear infinite`;
  });
})();

/* Background dots */
const bg=document.getElementById('bg');
for(let i=0;i<40;i++){
  const d=document.createElement('div');
  d.className='dot';
  const s=4+Math.random()*8;
  d.style.width=d.style.height=s+'px';
  d.style.left=Math.random()*100+'vw';
  d.style.top=Math.random()*100+'vh';
  bg.appendChild(d);
}
</script>
</body>
</html>
