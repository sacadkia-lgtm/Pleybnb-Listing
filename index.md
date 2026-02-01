<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Play BNB — Exchanges</title>

<style>
body{
  margin:0;
  background:#000;
  color:#fff;
  font-family:Inter,Arial;
  display:flex;
  justify-content:center;
  align-items:center;
  padding:30px;
}
.wrap{width:1100px}
.marquee-wrap{
  overflow:hidden;
  background:rgba(255,255,255,0.03);
  padding:20px;
  border-radius:12px;
}
.marquee{display:flex;gap:40px;will-change:transform}
.item{
  min-width:240px;
  display:flex;
  align-items:center;
  gap:14px;
  background:rgba(255,255,255,0.04);
  padding:14px;
  border-radius:12px;
}
.logo{
  width:90px;height:90px;
  background:#ffd84d;
  border-radius:14px;
  display:flex;
  align-items:center;
  justify-content:center;
}
.logo svg{width:70%;height:70%}
</style>
</head>

<body>
<div class="wrap">
  <div class="marquee-wrap">
    <div id="marquee" class="marquee">

      <!-- BINANCE -->
      <div class="item">
        <div class="logo">
          <svg viewBox="0 0 24 24" fill="#000">
            <path d="M12 2l4 4-4 4-4-4 4-4zm6 6l4 4-4 4-4-4 4-4zM12 14l4 4-4 4-4-4 4-4zm-6-6l4 4-4 4-4-4 4-4z"/>
          </svg>
        </div>Binance
      </div>

      <!-- COINBASE -->
      <div class="item">
        <div class="logo">
          <svg viewBox="0 0 24 24" fill="#000">
            <circle cx="12" cy="12" r="9"/>
            <circle cx="12" cy="12" r="5" fill="#ffd84d"/>
          </svg>
        </div>Coinbase
      </div>

      <!-- OKX -->
      <div class="item">
        <div class="logo">
          <svg viewBox="0 0 24 24" fill="#000">
            <rect x="2" y="2" width="8" height="8"/>
            <rect x="14" y="2" width="8" height="8"/>
            <rect x="2" y="14" width="8" height="8"/>
            <rect x="14" y="14" width="8" height="8"/>
          </svg>
        </div>OKX
      </div>

      <!-- UNISWAP -->
      <div class="item">
        <div class="logo">
          <svg viewBox="0 0 24 24" fill="#000">
            <path d="M12 2c4 3 6 7 6 11s-3 7-6 9c-3-2-6-5-6-9S8 5 12 2z"/>
          </svg>
        </div>Uniswap
      </div>

    </div>
  </div>
</div>

<script>
const marquee=document.getElementById('marquee');
const clone=marquee.cloneNode(true);
marquee.appendChild(clone);

requestAnimationFrame(()=>{
  const w=marquee.scrollWidth/2;
  const style=document.createElement('style');
  style.innerHTML=`@keyframes scroll{to{transform:translateX(-${w}px)}}`;
  document.head.appendChild(style);
  marquee.style.animation=`scroll ${w/40}s linear infinite`;
});
</script>
</body>
</html>
