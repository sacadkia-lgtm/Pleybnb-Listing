
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
  --muted:rgba(255,255,255,0.75);
  --speed:40;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%}
body{
  background:var(--bg);
  color:#fff;
  font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Arial;
  display:flex;
  align-items:center;
  justify-content:center;
  padding:28px;
  overflow:hidden;
}

.wrap{width:min(1200px,96vw)}
.card{
  background:rgba(255,255,255,0.02);
  border-radius:14px;
  padding:30px;
  border:1px solid rgba(255,255,255,0.04);
}

.header{display:flex;gap:16px;margin-bottom:18px}
.logo-box{
  width:100px;height:100px;border-radius:16px;
  background:radial-gradient(circle at 30% 25%,var(--gold-1),var(--gold-2));
}
.brand-title{font-size:42px;font-weight:900;color:var(--gold-1)}
.subtitle{font-size:14px;color:var(--muted)}

.marquee-wrap{
  overflow:hidden;
  background:rgba(0,0,0,0.4);
  padding:18px;
  border-radius:12px;
}
.marquee{display:flex;gap:34px;will-change:transform}
.track{display:flex;gap:34px}

.item{
  min-width:260px;
  padding:12px 16px;
  border-radius:12px;
  display:flex;
  align-items:center;
  gap:14px;
  background:rgba(255,255,255,0.02);
  font-weight:800;
  color:var(--muted);
}
.logo{
  width:120px;height:120px;
  border-radius:14px;
  background:var(--gold-1);
  display:flex;
  align-items:center;
  justify-content:center;
}
.logo img{
  width:78%;
  height:78%;
  object-fit:contain;
}
</style>
</head>

<body>
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
const logos = [
  {name:'Binance', file:'logos/binance.svg'},
  {name:'Coinbase', file:'logos/coinbase.svg'},
  {name:'OKX', file:'logos/okx.png'},
  {name:'KuCoin', file:'logos/kucoin.png'},
  {name:'Bybit', file:'logos/bybit.svg'},

  {name:'Uniswap', file:'logos/uniswap.svg'},
  {name:'PancakeSwap', file:'logos/pancakeswap.png'},
  {name:'SushiSwap', file:'logos/sushiswap.svg'},
  {name:'Curve', file:'logos/curve.png'},
  {name:'dYdX', file:'logos/dydx.svg'}
];

const marquee = document.getElementById('marquee');

function buildTrack(){
  const track = document.createElement('div');
  track.className='track';

  logos.forEach(l=>{
    const item=document.createElement('div');
    item.className='item';

    const box=document.createElement('div');
    box.className='logo';
    const img=document.createElement('img');
    img.src=l.file;
    img.alt=l.name;

    box.appendChild(img);
    item.appendChild(box);
    item.appendChild(document.createTextNode(l.name));
    track.appendChild(item);
  });

  return track;
}

const t1=buildTrack();
const t2=t1.cloneNode(true);
marquee.appendChild(t1);
marquee.appendChild(t2);

requestAnimationFrame(()=>{
  const w=t1.offsetWidth;
  const duration=w/40;
  const style=document.createElement('style');
  style.innerHTML=`@keyframes scroll{from{transform:translateX(0)}to{transform:translateX(-${w}px)}}`;
  document.head.appendChild(style);
  marquee.style.animation=`scroll ${duration}s linear infinite`;
});
</script>
</body>
</html>
