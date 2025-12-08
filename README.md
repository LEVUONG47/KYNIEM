<!-- File: love.html (phiên bản nhiều chuyển động) -->
<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Tình yêu màu hồng</title>
  <meta name="theme-color" content="#ff4d8d" />
  <style>
    :root{
      --pink:#ff4d8d; --pink-2:#ff8ab5; --bg:#fff0f5; --text:#2b2230; --muted:#6f5f6a;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0; font-family:ui-sans-serif,system-ui,Segoe UI,Roboto,Helvetica,Arial;
      color:var(--text);
      background: radial-gradient(1200px 800px at 20% -20%, #ffe1ee, #ffd6ea 50%, #ffecf4);
      display:grid; place-items:center; padding:20px; overflow:hidden;
      animation:bgShift 12s ease-in-out infinite alternate;
    }
    @keyframes bgShift{
      0%{ background-position:0 0; filter:saturate(1) }
      100%{ background-position:40% 20%; filter:saturate(1.08) }
    }
    .scene{position:fixed; inset:0; z-index:-1}
    .card{
      width:100%; max-width:760px; background:#fff; border-radius:28px;
      padding:28px; box-shadow:0 24px 80px rgba(255,77,141,.25);
      border:1px solid #ffd3e4; transform-style:preserve-3d;
      will-change:transform, box-shadow;
      animation:popIn .7s cubic-bezier(.2,.9,.2,1) 1 both;
    }
    @keyframes popIn{from{opacity:0; transform:translateY(12px) scale(.98)} to{opacity:1; transform:none}}
    header{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:8px}
    .brand{display:flex;align-items:center;gap:12px}
    .logo{width:40px;height:40px;border-radius:12px;background:conic-gradient(from 0deg,var(--pink),#ffa6c9,#ffe3f0,var(--pink)); animation:spin 6s linear infinite}
    @keyframes spin{to{transform:rotate(360deg)}}
    h1{margin:0;font-size:20px;letter-spacing:.3px;color:#a31d53}
    .subtitle{margin:0;color:var(--muted);font-size:14px}
    main{display:grid;gap:16px}
    .heart-wrap{position:relative;width:240px;height:210px;margin:12px auto 8px; transform:translateZ(30px)}
    .heart{
      position:absolute;inset:0;margin:auto;width:150px;height:150px;background:var(--pink);
      transform:rotate(45deg);border-radius:20px;box-shadow:0 30px 80px rgba(255,77,141,.35);
      animation:pulse 1.2s ease-in-out infinite;
    }
    .heart::before,.heart::after{content:"";position:absolute;background:var(--pink);width:150px;height:150px;border-radius:50%}
    .heart::before{left:-75px;top:0}
    .heart::after{top:-75px;left:0}
    @keyframes pulse{0%,100%{transform:rotate(45deg) scale(1)}50%{transform:rotate(45deg) scale(1.12)}}
    .msg{font-size:18px;color:#7b2551;text-align:center;margin:0 auto 6px;max-width:640px}
    .big{font-size:28px;font-weight:800;color:#a31d53;text-align:center;margin:6px 0 0}
    .bubble{
      margin:10px auto 0;max-width:660px;padding:14px 16px;border-radius:14px;background:#fff6fb;
      border:1px solid #ffd0e2;color:#8c3a60;text-align:center;word-break:break-word; min-height:56px
    }
    .caret{display:inline-block; width:10px; background:linear-gradient(90deg,#ffaccd,#ff4d8d); margin-left:2px; animation:blink .9s step-end infinite}
    @keyframes blink{50%{opacity:0}}
    .actions{display:flex;justify-content:center;gap:10px;flex-wrap:wrap;margin-top:12px}
    button{
      appearance:none;border:none;border-radius:12px;padding:10px 14px;font-weight:700;cursor:pointer;
      box-shadow:0 8px 20px rgba(255,77,141,.25);transition:transform .06s ease,filter .2s ease;
    }
    .btn-primary{background:linear-gradient(90deg,var(--pink),var(--pink-2));color:#fff}
    .btn-ghost{background:#ffe3f0;color:#a31d53}
    button:hover{filter:brightness(1.05)}
    button:active{transform:translateY(1px)}
    footer{margin-top:16px;color:#9c5d7b;font-size:12px;text-align:center}

    .toolbar{display:flex;gap:8px;align-items:center}
    .toggle{
      display:inline-flex; align-items:center; gap:6px; background:#ffe3f0; padding:6px 10px; border-radius:999px;
      color:#a31d53; font-weight:700; cursor:pointer; user-select:none
    }
    .toggle input{appearance:none; width:20px; height:20px; border-radius:50%; background:#fff; border:1px solid #ffa6c9; position:relative; outline:none}
    .toggle input:checked::after{content:""; position:absolute; inset:3px; background:linear-gradient(90deg,var(--pink),var(--pink-2)); border-radius:50%}

    @media (prefers-reduced-motion: reduce){
      *{animation:none!important; transition:none!important}
    }
  </style>
</head>
<body>
  <!-- lớp canvas mưa trái tim -->
  <canvas class="scene" id="hearts"></canvas>

  <div class="card" id="card" role="application" aria-label="Trang tỏ tình màu hồng">
    <header>
      <div class="brand">
        <div class="logo" aria-hidden="true"></div>
        <div>
          <h1>Tình yêu màu hồng</h1>
          <p class="subtitle">Một lời nhắn nhỏ dành cho em/anh 💗</p>
        </div>
      </div>
      <div class="toolbar">
        <label class="toggle" title="Bật tắt hiệu ứng mạnh">
          <input id="fx-strong" type="checkbox" />
          <span>Hiệu ứng: Mạnh</span>
        </label>
        <button id="btn-retype" class="btn-ghost">Gõ lại ✍️</button>
        <div id="status" class="subtitle" aria-live="polite">Sẵn sàng</div>
      </div>
    </header>

    <main>
      <div class="heart-wrap" aria-hidden="true"><div class="heart"></div></div>
      <p class="big">Yêu em nhiều như… vô cực ♾️</p>
      <p class="msg">Thông điệp gửi tới trái tim bạn:</p>
      <div id="love-text" class="bubble"><span id="typed"></span><span class="caret"></span></div>
      <div class="actions">
        <button id="btn-confetti" class="btn-primary">Bắn pháo hoa 🎉</button>
        <button id="btn-share" class="btn-ghost">Chia sẻ 💌</button>
      </div>
      <footer>Tip: đổi nội dung bằng <code>?m=...</code> trong URL • Kéo chuột/nghiêng máy để thấy hiệu ứng.</footer>
    </main>
  </div>

  <script>
    // ===== Helpers
    const $ = (id)=>document.getElementById(id);
    const prefersReduced = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;

    function getParam(name){
      const u = new URL(location.href);
      const v = u.searchParams.get(name);
      return v ? v.trim() : "";
    }

    // ===== Typewriter
    async function typeText(el, text, speed=22){
      el.textContent = "";
      const chars = [...text];
      for (let i=0;i<chars.length;i++){
        el.textContent += chars[i];
        await new Promise(r=>setTimeout(r, speed + Math.random()*12)); // why: tự nhiên hơn
      }
    }

    function setLoveText(){
      const raw = getParam("m");
      return raw || "Anh yêu em rất rất nhiều! 💖";
    }

    // ===== Confetti
    function fireConfetti(ms=1100, strong=false){
      const end = Date.now() + ms;
      const base = strong ? 260 : 140;
      const colors = ["#ff4d8d","#ff9ac2","#ffd1e3","#ffffff","#ffc3d9"];
      const cvs = document.createElement("canvas");
      cvs.style.cssText = "position:fixed;inset:0;pointer-events:none;z-index:9999";
      cvs.width = innerWidth; cvs.height = innerHeight;
      document.body.appendChild(cvs);
      const ctx = cvs.getContext("2d");
      const P = Array.from({length: base}, () => ({
        x: Math.random()*cvs.width, y: -20+Math.random()*20,
        sx: -3+Math.random()*6, sy: 4+Math.random()*4,
        s: 4+Math.random()*6, c: colors[(Math.random()*colors.length)|0],
        r: Math.random()*Math.PI, sr: -0.2+Math.random()*0.4
      }));
      (function frame(){
        ctx.clearRect(0,0,cvs.width,cvs.height);
        for(const p of P){
          p.x+=p.sx; p.y+=p.sy; p.r+=p.sr;
          ctx.save(); ctx.translate(p.x,p.y); ctx.rotate(p.r);
          ctx.fillStyle=p.c; ctx.fillRect(-p.s/2,-p.s/2,p.s,p.s); ctx.restore();
        }
        if(Date.now()<end) requestAnimationFrame(frame); else cvs.remove();
      })();
    }

    // ===== Heart rain (canvas background)
    (function heartsRain(){
      const cvs = $("hearts"); const ctx = cvs.getContext("2d");
      let W=0, H=0, DPR = Math.max(1, Math.min(2, devicePixelRatio||1));
      const pool = [];
      let running = true;
      const MAX_BASE = 80; // nhẹ
      const MAX_STRONG = 160; // mạnh

      function resize(){
        W = innerWidth; H = innerHeight;
        cvs.style.width = W+"px"; cvs.style.height = H+"px";
        cvs.width = Math.floor(W*DPR); cvs.height = Math.floor(H*DPR);
        ctx.setTransform(DPR,0,0,DPR,0,0);
      }
      function spawn(n){
        for(let i=0;i<n;i++){
          const s = 10 + Math.random()*26;
          pool.push({
            x: Math.random()*W, y: -40 - Math.random()*H,
            vx: -0.8 + Math.random()*1.6,
            vy: 0.8 + Math.random()*1.6,
            size: s, rot: Math.random()*Math.PI, vr: -0.02 + Math.random()*0.04,
            col: `hsl(${330+Math.random()*20}deg 100% ${70+Math.random()*15}%)`
          });
        }
      }
      function heartPath(x,y,s,rot){
        ctx.save();
        ctx.translate(x,y); ctx.rotate(rot); ctx.scale(s/32, s/32);
        ctx.beginPath();
        ctx.moveTo(16,28);
        ctx.bezierCurveTo(4,18,0,10,8,6);
        ctx.bezierCurveTo(14,3,18,6,16,10);
        ctx.moveTo(16,28);
        ctx.bezierCurveTo(28,18,32,10,24,6);
        ctx.bezierCurveTo(18,3,14,6,16,10);
        ctx.fill(); ctx.restore();
      }
      function step(){
        const strong = $("fx-strong").checked;
        const target = strong ? MAX_STRONG : MAX_BASE;
        if(pool.length < target) spawn(6);
        ctx.clearRect(0,0,W,H);
        ctx.globalAlpha = 0.85;
        for(let i=pool.length-1;i>=0;i--){
          const p = pool[i];
          p.x += p.vx; p.y += p.vy; p.rot += p.vr;
          ctx.fillStyle = p.col;
          heartPath(p.x,p.y,p.size,p.rot);
          if(p.y > H+60) pool.splice(i,1);
        }
        if(running) requestAnimationFrame(step);
      }
      addEventListener("resize", resize);
      document.addEventListener("visibilitychange", ()=>{ running = document.visibilityState==="visible"; if(running) step(); });
      resize();
      if(!prefersReduced){ spawn(40); step(); }
    })();

    // ===== Parallax tilt (mouse/gyro)
    (function parallax(){
      const card = $("card");
      let active = true;
      function apply(x, y){
        const rx = (y-0.5)*-10, ry = (x-0.5)*10;
        card.style.transform = `perspective(1000px) rotateX(${rx}deg) rotateY(${ry}deg) translateZ(0)`;
        card.style.boxShadow = `${-ry*1.2}px ${10+Math.abs(rx)*1.5}px 60px rgba(255,77,141,.25)`;
      }
      window.addEventListener("mousemove", (e)=>{
        if(!active) return;
        const x = e.clientX / innerWidth, y = e.clientY / innerHeight;
        apply(x,y);
      });
      if(window.DeviceOrientationEvent && typeof DeviceOrientationEvent.requestPermission === "function"){
        // iOS cần user gesture
        window.addEventListener("click", async ()=>{
          try{ const g = await DeviceOrientationEvent.requestPermission(); if(g==="granted"){ active=true; } }catch{}
        }, {once:true});
      }
    })();

    // ===== Share
    async function shareLove(){
      const text = "💌 " + $("typed").textContent + " — Gửi tặng bạn một trái tim 💗";
      try{
        if(navigator.share){ await navigator.share({title:"Lời tỏ tình", text, url: location.href}); }
        else if(navigator.clipboard){ await navigator.clipboard.writeText(text); alert("Đã sao chép lời tỏ tình!"); }
        else{ alert(text); }
      }catch{}
    }

    // ===== Wire-up
    (async function init(){
      const msg = setLoveText();
      const speed = prefersReduced ? 0 : 22;
      if(speed===0){ $("typed").textContent = msg; } else { await typeText($("typed"), msg, speed); }
      $("btn-share").addEventListener("click", shareLove);
      $("btn-confetti").addEventListener("click", ()=> fireConfetti(1200, $("fx-strong").checked));
      $("btn-retype").addEventListener("click", async ()=>{
        await typeText($("typed"), setLoveText(), prefersReduced ? 0 : 18);
        fireConfetti(900, $("fx-strong").checked);
      });
    })();
  </script>
</body>
</html>
