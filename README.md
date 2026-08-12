<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>KERNEL::FORGE — Advanced Programming</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&family=JetBrains+Mono:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0A141F; --panel:#0D1B2A; --panel2:#0F2233; --line:#1E3A52; --line2:#16324A;
    --amber:#FFB454; --cyan:#35E0D0; --coral:#FF6B7A; --lime:#B7F05E;
    --ink:#C9D7E8; --dim:#7FA3C0; --faint:#51708F;
    --mono:"JetBrains Mono",ui-monospace,Menlo,Consolas,monospace;
    --disp:"Space Grotesk","Segoe UI",system-ui,sans-serif;
  }
  *{margin:0;padding:0;box-sizing:border-box}
  html{scroll-behavior:smooth}
  body{
    background:var(--bg); color:var(--ink);
    font-family:var(--mono); font-size:15px; line-height:1.6;
    overflow-x:hidden;
  }

  /* ---------- ambient layers ---------- */
  .grid-bg{
    position:fixed; inset:0; z-index:-3; pointer-events:none;
    background-image:
      repeating-linear-gradient(26.57deg, transparent 0 23px, rgba(53,224,208,.045) 23px 24px),
      repeating-linear-gradient(-26.57deg, transparent 0 23px, rgba(53,224,208,.045) 23px 24px);
    mask-image:radial-gradient(ellipse at 50% 20%, #000 30%, transparent 85%);
    -webkit-mask-image:radial-gradient(ellipse at 50% 20%, #000 30%, transparent 85%);
  }
  .orb{position:fixed; z-index:-2; border-radius:50%; filter:blur(90px); pointer-events:none; opacity:.14}
  .orb.a{width:480px;height:480px;background:var(--amber);top:-140px;left:-120px;animation:drift 26s ease-in-out infinite alternate}
  .orb.b{width:560px;height:560px;background:var(--cyan);top:30%;right:-200px;animation:drift 32s ease-in-out infinite alternate-reverse}
  .orb.c{width:420px;height:420px;background:var(--coral);bottom:-160px;left:20%;animation:drift 38s ease-in-out infinite alternate}
  @keyframes drift{from{transform:translate(0,0)}to{transform:translate(60px,40px)}}
  .glyph{position:fixed;z-index:-1;font-family:var(--mono);color:var(--faint);opacity:.16;pointer-events:none;animation:floaty 14s ease-in-out infinite alternate}
  .glyph:nth-child(1){top:18%;left:6%;font-size:28px;animation-duration:13s}
  .glyph:nth-child(2){top:52%;left:3%;font-size:20px;animation-duration:17s;color:var(--amber)}
  .glyph:nth-child(3){top:30%;right:5%;font-size:24px;animation-duration:15s;color:var(--cyan)}
  .glyph:nth-child(4){top:70%;right:8%;font-size:30px;animation-duration:19s}
  .glyph:nth-child(5){top:86%;left:12%;font-size:22px;animation-duration:16s;color:var(--lime)}
  @keyframes floaty{from{transform:translateY(0) rotate(-3deg)}to{transform:translateY(-26px) rotate(3deg)}}

  /* ---------- topbar ---------- */
  .topbar{
    position:fixed; top:0; left:0; right:0; z-index:50;
    display:flex; justify-content:space-between; align-items:center;
    padding:10px 22px; font-size:11px; letter-spacing:1.5px; color:var(--faint);
    background:rgba(10,20,31,.82); backdrop-filter:blur(6px);
    border-bottom:1px solid var(--line2);
  }
  .topbar b{color:var(--amber);font-weight:700}
  #progress{position:fixed;top:0;left:0;height:2px;width:100%;z-index:51;
    background:linear-gradient(90deg,var(--amber),var(--coral),var(--cyan));
    transform-origin:0 50%; transform:scaleX(0)}

  main{max-width:1100px;margin:0 auto;padding:0 24px}
  .hero-wrap{margin-top:52px}
  .hero-wrap svg{display:block;width:100%;height:auto}

  /* ---------- section headers ---------- */
  section{padding:64px 0 8px}
  .sec-head{display:flex;align-items:baseline;gap:16px;margin-bottom:28px;flex-wrap:wrap}
  .sec-no{font-size:12px;letter-spacing:3px;color:var(--amber)}
  .sec-head h2{
    font-family:var(--disp); font-weight:700; text-transform:uppercase;
    font-size:clamp(24px,4vw,38px); letter-spacing:2px; color:#EAF6FF;
  }
  .sec-line{flex:1;height:1px;background:linear-gradient(90deg,var(--line),transparent);min-width:60px}
  .sec-tag{font-size:11px;color:var(--faint);letter-spacing:1px}

  /* ---------- identity duo ---------- */
  .duo{display:grid;grid-template-columns:1.15fr .85fr;gap:22px}
  @media(max-width:860px){.duo{grid-template-columns:1fr}}
  .window{
    background:var(--panel); border:1px solid var(--line); border-radius:10px; overflow:hidden;
    box-shadow:0 18px 50px rgba(0,0,0,.35);
    transition:transform .35s ease, border-color .35s ease, box-shadow .35s ease;
  }
  .window:hover{transform:translateY(-4px);border-color:rgba(53,224,208,.5);box-shadow:0 24px 60px rgba(0,0,0,.5),0 0 30px rgba(53,224,208,.08)}
  .win-bar{display:flex;align-items:center;gap:8px;padding:10px 14px;border-bottom:1px solid var(--line2);background:#0B1930}
  .win-bar i{width:10px;height:10px;border-radius:50%;display:inline-block}
  .win-bar i:nth-child(1){background:var(--coral)} .win-bar i:nth-child(2){background:var(--amber)} .win-bar i:nth-child(3){background:var(--lime)}
  .win-bar span{margin-left:8px;font-size:11px;color:var(--faint);letter-spacing:.5px}
  pre{padding:18px 20px;overflow-x:auto;font-size:13.5px;line-height:1.75}
  .k{color:var(--cyan)} .t{color:var(--amber)} .s{color:var(--lime)} .c{color:var(--faint);font-style:italic} .w{color:#EAF6FF}
  .term pre{color:var(--dim)}
  .term .p{color:var(--lime)} .term .v{color:#EAF6FF}
  .cursor{display:inline-block;width:8px;height:15px;background:var(--lime);vertical-align:-2px;animation:blink 1.1s steps(1) infinite}
  @keyframes blink{50%{opacity:0}}

  .bullets{list-style:none;margin-top:26px;display:grid;gap:12px}
  .bullets li{padding-left:26px;position:relative;color:var(--dim)}
  .bullets li::before{content:"◤";position:absolute;left:0;color:var(--coral)}
  .bullets b{color:#EAF6FF}

  /* ---------- tables ---------- */
  .tbl-wrap{border:1px solid var(--line);border-radius:10px;overflow:hidden;background:var(--panel)}
  table{width:100%;border-collapse:collapse;font-size:13.5px}
  th{
    text-align:left;padding:12px 18px;font-size:11px;letter-spacing:2px;text-transform:uppercase;
    color:var(--faint);background:#0B1930;border-bottom:1px solid var(--line);
  }
  td{padding:13px 18px;border-bottom:1px solid var(--line2);vertical-align:top}
  tr:last-child td{border-bottom:none}
  tbody tr{transition:background .25s ease, box-shadow .25s ease}
  tbody tr:hover{background:rgba(53,224,208,.05);box-shadow:inset 3px 0 0 var(--cyan)}
  td b{color:#EAF6FF}
  .layer-chip{display:inline-block;font-size:11px;padding:2px 8px;border-radius:4px;border:1px solid currentColor}
  .l0{color:var(--coral)} .l1{color:var(--amber)} .l2{color:var(--cyan)} .l3{color:var(--lime)} .l4{color:var(--dim)}

  .conveyor{margin:56px 0 0;border-radius:12px;overflow:hidden;border:1px solid var(--line2)}
  .conveyor svg{display:block;width:100%;height:auto}

  /* ---------- badges ---------- */
  .badges{display:flex;flex-wrap:wrap;gap:10px;margin-bottom:14px}
  .badges img{
    transition:transform .25s ease, filter .25s ease; border-radius:6px;
  }
  .badges img:hover{transform:translateY(-4px) rotate(-1.5deg);filter:drop-shadow(0 8px 16px rgba(0,0,0,.5))}

  /* ---------- project cards ---------- */
  .ops{display:grid;grid-template-columns:repeat(2,1fr);gap:18px}
  @media(max-width:760px){.ops{grid-template-columns:1fr}}
  .op-card{
    position:relative; background:linear-gradient(160deg,var(--panel2),var(--panel));
    border:1px solid var(--line); border-radius:8px; padding:22px 24px; overflow:hidden;
    transition:transform .3s ease, border-color .3s ease, box-shadow .3s ease;
  }
  .op-card::before,.op-card::after{content:"";position:absolute;width:14px;height:14px;border:2px solid var(--amber);opacity:.55;transition:opacity .3s}
  .op-card::before{top:8px;left:8px;border-right:none;border-bottom:none}
  .op-card::after{bottom:8px;right:8px;border-left:none;border-top:none}
  .op-card:hover{transform:translateY(-5px);border-color:rgba(255,180,84,.55);box-shadow:0 20px 44px rgba(0,0,0,.45)}
  .op-card:hover::before,.op-card:hover::after{opacity:1}
  .op-card .spot{
    position:absolute;inset:0;pointer-events:none;opacity:0;transition:opacity .3s;
    background:radial-gradient(220px circle at var(--mx,50%) var(--my,50%), rgba(53,224,208,.12), transparent 70%);
  }
  .op-card:hover .spot{opacity:1}
  .op-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px}
  .op-top code{font-size:16px;font-weight:700;color:var(--amber)}
  .op-star{font-size:12px;color:var(--lime);border:1px solid rgba(183,240,94,.4);padding:2px 9px;border-radius:20px}
  .op-card p{color:var(--dim);font-size:13px;margin-bottom:14px}
  .op-tags{display:flex;gap:8px;flex-wrap:wrap}
  .op-tags span{font-size:11px;color:var(--cyan);background:rgba(53,224,208,.08);border:1px solid rgba(53,224,208,.25);padding:2px 9px;border-radius:4px}

  /* ---------- pipeline ---------- */
  .pipe{display:flex;align-items:stretch;gap:0;flex-wrap:wrap}
  .stage{
    flex:1;min-width:220px;background:var(--panel);border:1px solid var(--line);border-radius:8px;padding:16px;
    transition:border-color .3s, transform .3s;
  }
  .stage:hover{border-color:var(--amber);transform:translateY(-3px)}
  .stage h3{font-size:11px;letter-spacing:2px;color:var(--amber);margin-bottom:12px;text-transform:uppercase}
  .stage .nodes{display:flex;flex-wrap:wrap;gap:6px;align-items:center}
  .node{font-size:12px;padding:5px 11px;border-radius:5px;background:#0B1930;border:1px solid var(--line2);color:var(--ink);transition:border-color .25s,color .25s}
  .node:hover{border-color:var(--cyan);color:#fff}
  .arr{color:var(--faint);font-size:13px;padding:0 2px}
  .link{
    align-self:center;width:44px;height:2px;flex:none;
    background:repeating-linear-gradient(90deg,var(--cyan) 0 6px,transparent 6px 12px);
    animation:march 1s linear infinite;
  }
  @keyframes march{from{background-position:0 0}to{background-position:12px 0}}
  @media(max-width:860px){
    .pipe{flex-direction:column}
    .link{width:2px;height:34px;align-self:center;background:repeating-linear-gradient(180deg,var(--cyan) 0 6px,transparent 6px 12px);animation-name:marchV}
    @keyframes marchV{from{background-position:0 0}to{background-position:0 12px}}
  }

  /* ---------- telemetry ---------- */
  .stats{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:16px}
  @media(max-width:760px){.stats{grid-template-columns:1fr}}
  .stats img{width:100%;height:auto;border-radius:8px;border:1px solid var(--line2);background:var(--panel);transition:transform .3s}
  .stats img:hover{transform:scale(1.015)}

  .divider{margin:64px 0 0}
  .divider svg{display:block;width:100%;height:auto}

  /* ---------- handshake ---------- */
  details{margin-top:30px;border:1px dashed var(--line);border-radius:8px;padding:0;transition:border-color .3s}
  details[open]{border-color:rgba(255,180,84,.5)}
  summary{cursor:pointer;padding:14px 20px;font-size:13px;color:var(--amber);list-style:none;user-select:none}
  summary::-webkit-details-marker{display:none}
  summary::before{content:"▸ ";color:var(--faint)}
  details[open] summary::before{content:"▾ "}
  details pre{border-top:1px solid var(--line2);font-size:12.5px;color:var(--dim)}

  footer{
    text-align:center;padding:44px 20px 56px;font-size:11px;letter-spacing:2px;color:var(--faint);
    border-top:1px solid var(--line2);margin-top:64px;
  }
  footer b{color:var(--amber)}

  /* ---------- reveals ---------- */
  .reveal{opacity:0;transform:translateY(22px);transition:opacity .7s ease, transform .7s ease}
  .reveal.in{opacity:1;transform:none}
</style>
</head>
<body>

<div class="grid-bg"></div>
<div class="orb a"></div><div class="orb b"></div><div class="orb c"></div>
<span class="glyph">λ</span><span class="glyph">&amp;</span><span class="glyph">⟁</span><span class="glyph">#</span><span class="glyph">⌥</span>

<div id="progress"></div>
<nav class="topbar">
  <span>▛ <b>KERNEL::FORGE</b> — iso/30° render</span>
  <span>v3.2.1 · zero frameworks · <b>ADVANCED PROGRAMMING</b></span>
</nav>

<main>

  <!-- ═══════════ HERO — inlined hero.svg ═══════════ -->
  <div class="hero-wrap">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1040 480" role="img" aria-label="isometric dev forge">
      <defs>
        <linearGradient id="bg" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0" stop-color="#102135"/><stop offset="1" stop-color="#0A141F"/>
        </linearGradient>
        <radialGradient id="vig" cx="0.5" cy="0.45" r="0.8">
          <stop offset="0.55" stop-color="#050B12" stop-opacity="0"/>
          <stop offset="1" stop-color="#050B12" stop-opacity="0.55"/>
        </radialGradient>
        <linearGradient id="platTop" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0" stop-color="#16354E"/><stop offset="1" stop-color="#0F2438"/>
        </linearGradient>
        <linearGradient id="scanG" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0" stop-color="#35E0D0" stop-opacity="0"/>
          <stop offset="0.5" stop-color="#35E0D0" stop-opacity="0.5"/>
          <stop offset="1" stop-color="#35E0D0" stop-opacity="0"/>
        </linearGradient>
        <pattern id="iso" width="48" height="24" patternUnits="userSpaceOnUse">
          <path d="M0 12 L24 0 L48 12 L24 24 Z" fill="none" stroke="#35E0D0" stroke-opacity="0.05"/>
        </pattern>
        <pattern id="isoGrid" width="48" height="24" patternUnits="userSpaceOnUse">
          <path d="M0 12 L24 0 L48 12 L24 24 Z" fill="none" stroke="#35E0D0" stroke-opacity="0.12"/>
        </pattern>
        <filter id="soft" x="-80%" y="-80%" width="260%" height="260%"><feGaussianBlur stdDeviation="16"/></filter>
        <clipPath id="platClip"><polygon points="520,210 770,335 520,460 270,335"/></clipPath>
        <g id="cube">
          <polygon points="0,0 40,20 0,40 -40,20" fill="currentColor"/>
          <polygon points="-40,20 0,40 0,86 -40,66" fill="currentColor"/>
          <polygon points="-40,20 0,40 0,86 -40,66" fill="#061019" opacity="0.42"/>
          <polygon points="40,20 0,40 0,86 40,66" fill="currentColor"/>
          <polygon points="40,20 0,40 0,86 40,66" fill="#061019" opacity="0.60"/>
          <path d="M0 0 L40 20 L40 66 L0 86 L-40 66 L-40 20 Z M0 40 L0 86 M-40 20 L0 40 L40 20" fill="none" stroke="#DFF6FF" stroke-opacity="0.15"/>
        </g>
      </defs>
      <rect width="1040" height="480" fill="url(#bg)"/>
      <rect width="1040" height="480" fill="url(#iso)"/>
      <ellipse cx="352" cy="300" rx="170" ry="95" fill="#FFB454" opacity="0.07" filter="url(#soft)"/>
      <ellipse cx="700" cy="210" rx="190" ry="105" fill="#35E0D0" opacity="0.08" filter="url(#soft)"/>
      <ellipse cx="180" cy="420" rx="150" ry="70" fill="#FF6B7A" opacity="0.05" filter="url(#soft)"/>
      <polygon points="520,460 770,335 770,357 520,482" fill="#0A1B2B"/>
      <polygon points="520,460 270,335 270,357 520,482" fill="#0D2133"/>
      <polygon points="520,210 770,335 520,460 270,335" fill="url(#platTop)" stroke="#35E0D0" stroke-opacity="0.35"/>
      <rect x="270" y="210" width="500" height="250" fill="url(#isoGrid)" clip-path="url(#platClip)"/>
      <g fill="#35E0D0">
        <polygon points="450,295 458,300 450,305 442,300"><animate attributeName="opacity" values="0.2;0.9;0.2" dur="3s" begin="0s" repeatCount="indefinite"/></polygon>
        <polygon points="590,295 598,300 590,305 582,300"><animate attributeName="opacity" values="0.2;0.9;0.2" dur="3s" begin="0.6s" repeatCount="indefinite"/></polygon>
        <polygon points="520,260 528,265 520,270 512,265"><animate attributeName="opacity" values="0.2;0.9;0.2" dur="3s" begin="1.2s" repeatCount="indefinite"/></polygon>
        <polygon points="450,365 458,370 450,375 442,370"><animate attributeName="opacity" values="0.2;0.9;0.2" dur="3s" begin="1.8s" repeatCount="indefinite"/></polygon>
        <polygon points="590,365 598,370 590,375 582,370"><animate attributeName="opacity" values="0.2;0.9;0.2" dur="3s" begin="2.4s" repeatCount="indefinite"/></polygon>
      </g>
      <g>
        <circle cx="470" cy="350" r="2" fill="#35E0D0" opacity="0">
          <animate attributeName="cy" values="360;265" dur="4.5s" repeatCount="indefinite"/>
          <animate attributeName="opacity" values="0;0.9;0" keyTimes="0;0.25;1" dur="4.5s" repeatCount="indefinite"/>
        </circle>
        <circle cx="700" cy="330" r="2" fill="#FFB454" opacity="0">
          <animate attributeName="cy" values="340;245" dur="5.5s" begin="1.2s" repeatCount="indefinite"/>
          <animate attributeName="opacity" values="0;0.8;0" keyTimes="0;0.25;1" dur="5.5s" begin="1.2s" repeatCount="indefinite"/>
        </circle>
        <circle cx="540" cy="400" r="1.6" fill="#FF6B7A" opacity="0">
          <animate attributeName="cy" values="410;320" dur="5s" begin="2s" repeatCount="indefinite"/>
          <animate attributeName="opacity" values="0;0.7;0" keyTimes="0;0.25;1" dur="5s" begin="2s" repeatCount="indefinite"/>
        </circle>
        <circle cx="760" cy="360" r="1.8" fill="#B7F05E" opacity="0">
          <animate attributeName="cy" values="370;285" dur="6s" begin="3s" repeatCount="indefinite"/>
          <animate attributeName="opacity" values="0;0.8;0" keyTimes="0;0.25;1" dur="6s" begin="3s" repeatCount="indefinite"/>
        </circle>
      </g>
      <g>
        <animateTransform attributeName="transform" type="translate" values="0 0;0 -6;0 0" dur="7s" repeatCount="indefinite" calcMode="spline" keySplines="0.45 0 0.55 1;0.45 0 0.55 1"/>
        <use href="#cube" transform="translate(352,294)" style="color:#FF6B7A"/>
        <use href="#cube" transform="translate(352,248)" style="color:#FFB454"/>
        <use href="#cube" transform="translate(352,202)" style="color:#35E0D0"/>
        <ellipse cx="352" cy="245" rx="104" ry="36" fill="none" stroke="#35E0D0" stroke-opacity="0.55" stroke-dasharray="5 9">
          <animate attributeName="stroke-dashoffset" from="0" to="-56" dur="1.8s" repeatCount="indefinite"/>
        </ellipse>
        <ellipse cx="352" cy="245" rx="128" ry="47" fill="none" stroke="#FFB454" stroke-opacity="0.35" stroke-dasharray="4 10">
          <animate attributeName="stroke-dashoffset" from="0" to="56" dur="2.4s" repeatCount="indefinite"/>
        </ellipse>
        <circle r="3.2" fill="#35E0D0">
          <animateMotion dur="8s" repeatCount="indefinite" path="M248,245 a104,36 0 1,0 208,0 a104,36 0 1,0 -208,0"/>
        </circle>
        <circle r="2.4" fill="#FFB454">
          <animateMotion dur="12s" repeatCount="indefinite" calcMode="linear" keyPoints="1;0" keyTimes="0;1" path="M224,245 a128,47 0 1,0 256,0 a128,47 0 1,0 -256,0"/>
        </circle>
      </g>
      <g transform="translate(352,108) scale(0.55)">
        <g>
          <animateTransform attributeName="transform" type="translate" values="0 0;0 -14;0 0" dur="4.5s" repeatCount="indefinite" calcMode="spline" keySplines="0.45 0 0.55 1;0.45 0 0.55 1"/>
          <use href="#cube" style="color:#B7F05E"/>
        </g>
      </g>
      <text x="398" y="120" font-size="22" fill="#B7F05E" opacity="0.9" font-family="ui-monospace,Menlo,Consolas,monospace">λ
        <animateTransform attributeName="transform" type="translate" values="0 0;0 -9;0 0" dur="5s" repeatCount="indefinite"/>
      </text>
      <g font-family="ui-monospace,Menlo,Consolas,monospace" font-size="10">
        <line x1="264" y1="337" x2="308" y2="337" stroke="#3D5A75" stroke-dasharray="3 4"/>
        <rect x="136" y="326" width="128" height="22" rx="6" fill="#0F2233" stroke="#FF6B7A" stroke-opacity="0.5"/>
        <text x="200" y="341" fill="#FF8C98" text-anchor="middle">L1 · kernel-space</text>
        <line x1="264" y1="291" x2="308" y2="291" stroke="#3D5A75" stroke-dasharray="3 4"/>
        <rect x="136" y="280" width="128" height="22" rx="6" fill="#0F2233" stroke="#FFB454" stroke-opacity="0.5"/>
        <text x="200" y="295" fill="#FFC178" text-anchor="middle">L2 · runtime</text>
        <line x1="264" y1="245" x2="308" y2="245" stroke="#3D5A75" stroke-dasharray="3 4"/>
        <rect x="136" y="234" width="128" height="22" rx="6" fill="#0F2233" stroke="#35E0D0" stroke-opacity="0.5"/>
        <text x="200" y="249" fill="#6FE8DC" text-anchor="middle">L3 · toolchain</text>
      </g>
      <g transform="translate(560,200)">
        <g>
          <animateTransform attributeName="transform" type="translate" values="0 0;0 -7;0 0" dur="6s" repeatCount="indefinite" calcMode="spline" keySplines="0.45 0 0.55 1;0.45 0 0.55 1"/>
          <g transform="matrix(1,-0.5,0,1,0,0)">
            <rect x="-8" y="-8" width="256" height="181" rx="10" fill="#35E0D0" opacity="0.10" filter="url(#soft)"/>
            <rect x="0" y="0" width="240" height="165" rx="8" fill="#0B1930" fill-opacity="0.94" stroke="#35E0D0" stroke-opacity="0.45"/>
            <line x1="0" y1="22" x2="240" y2="22" stroke="#1E3A52"/>
            <circle cx="14" cy="11" r="3.5" fill="#FF6B7A"/>
            <circle cx="27" cy="11" r="3.5" fill="#FFB454"/>
            <circle cx="40" cy="11" r="3.5" fill="#B7F05E"/>
            <text x="54" y="15" font-size="9" fill="#7FA3C0" font-family="ui-monospace,Menlo,Consolas,monospace">build --release --target riscv64</text>
            <g font-family="ui-monospace,Menlo,Consolas,monospace" font-size="10.5">
              <text x="14" y="40" fill="#B7F05E" opacity="0">#![no_std]<animate attributeName="opacity" to="1" dur="0.25s" begin="0.4s" fill="freeze"/></text>
              <text x="14" y="54" fill="#6E8BAB" opacity="0">#[repr(C, align(64))]<animate attributeName="opacity" to="1" dur="0.25s" begin="0.7s" fill="freeze"/></text>
              <text x="14" y="68" fill="#EAF6FF" opacity="0"><tspan fill="#35E0D0">struct</tspan> Ring<tspan fill="#FFB454">&lt;T, const N: usize&gt;</tspan> {<animate attributeName="opacity" to="1" dur="0.25s" begin="1.0s" fill="freeze"/></text>
              <text x="14" y="82" fill="#EAF6FF" opacity="0">    head: <tspan fill="#35E0D0">AtomicU64</tspan>,<animate attributeName="opacity" to="1" dur="0.25s" begin="1.3s" fill="freeze"/></text>
              <text x="14" y="96" fill="#EAF6FF" opacity="0">    slots: [<tspan fill="#FFB454">Cell&lt;T&gt;</tspan>; N],<animate attributeName="opacity" to="1" dur="0.25s" begin="1.6s" fill="freeze"/></text>
              <text x="14" y="110" fill="#EAF6FF" opacity="0">}<animate attributeName="opacity" to="1" dur="0.25s" begin="1.9s" fill="freeze"/></text>
              <text x="14" y="126" fill="#51708F" font-style="italic" opacity="0">// wait-free · single producer<animate attributeName="opacity" to="1" dur="0.25s" begin="2.3s" fill="freeze"/></text>
              <text x="14" y="148" fill="#B7F05E" opacity="0">❯<animate attributeName="opacity" to="1" dur="0.1s" begin="2.7s" fill="freeze"/></text>
            </g>
            <rect x="26" y="139" width="7" height="12" fill="#B7F05E" opacity="0">
              <animate attributeName="opacity" to="1" dur="0.1s" begin="2.7s" fill="freeze"/>
              <animate attributeName="opacity" values="1;0;1" dur="1.1s" begin="2.9s" repeatCount="indefinite"/>
            </rect>
            <rect x="0" y="-30" width="240" height="26" fill="url(#scanG)" opacity="0.35">
              <animate attributeName="y" values="-30;165" dur="5s" repeatCount="indefinite"/>
            </rect>
          </g>
        </g>
      </g>
      <g font-family="'Segoe UI',system-ui,sans-serif" font-size="12.5" font-weight="700" text-anchor="middle">
        <g><animateTransform attributeName="transform" type="translate" values="0 0;0 -8;0 0" dur="5s" repeatCount="indefinite"/>
          <rect x="118" y="150" width="66" height="26" rx="8" fill="#0F2233" stroke="#FF6B7A" stroke-opacity="0.6"/>
          <text x="151" y="167" fill="#FF8C98">eBPF</text></g>
        <g><animateTransform attributeName="transform" type="translate" values="0 0;0 -10;0 0" dur="6s" begin="0.8s" repeatCount="indefinite"/>
          <rect x="150" y="84" width="66" height="26" rx="8" fill="#0F2233" stroke="#35E0D0" stroke-opacity="0.6"/>
          <text x="183" y="101" fill="#6FE8DC">SIMD</text></g>
        <g><animateTransform attributeName="transform" type="translate" values="0 0;0 -7;0 0" dur="5.5s" begin="1.6s" repeatCount="indefinite"/>
          <rect x="66" y="292" width="96" height="26" rx="8" fill="#0F2233" stroke="#B7F05E" stroke-opacity="0.6"/>
          <text x="114" y="309" fill="#C9F584">&amp;mut self</text></g>
        <g><animateTransform attributeName="transform" type="translate" values="0 0;0 -9;0 0" dur="5.2s" begin="0.4s" repeatCount="indefinite"/>
          <rect x="846" y="58" width="72" height="26" rx="8" fill="#0F2233" stroke="#FFB454" stroke-opacity="0.6"/>
          <text x="882" y="75" fill="#FFC178">WASM</text></g>
        <g><animateTransform attributeName="transform" type="translate" values="0 0;0 -8;0 0" dur="6.5s" begin="2s" repeatCount="indefinite"/>
          <rect x="888" y="292" width="62" height="26" rx="8" fill="#0F2233" stroke="#35E0D0" stroke-opacity="0.6"/>
          <text x="919" y="309" fill="#6FE8DC">O(1)</text></g>
        <g><animateTransform attributeName="transform" type="translate" values="0 0;0 -10;0 0" dur="5.8s" begin="1.2s" repeatCount="indefinite"/>
          <rect x="836" y="380" width="104" height="26" rx="8" fill="#0F2233" stroke="#FF6B7A" stroke-opacity="0.6"/>
          <text x="888" y="397" fill="#FF8C98">#![unsafe]</text></g>
      </g>
      <g stroke="#3D5A75" stroke-width="1.5" fill="none">
        <path d="M238 58 h12 M244 52 v12"><animate attributeName="opacity" values="0.2;0.9;0.2" dur="3.4s" repeatCount="indefinite"/></path>
        <path d="M962 178 h12 M968 172 v12"><animate attributeName="opacity" values="0.2;0.9;0.2" dur="4s" begin="1s" repeatCount="indefinite"/></path>
        <path d="M62 214 h12 M68 208 v12"><animate attributeName="opacity" values="0.2;0.9;0.2" dur="3.7s" begin="2s" repeatCount="indefinite"/></path>
      </g>
      <rect width="1040" height="480" fill="url(#vig)"/>
      <g font-family="ui-monospace,Menlo,Consolas,monospace" fill="#51708F" font-size="10" letter-spacing="1">
        <text x="24" y="32">▛ KERNEL::FORGE — iso/30° render</text>
        <text x="1016" y="32" text-anchor="end">v3.2.1 · zero frameworks</text>
        <text x="520" y="474" text-anchor="middle" font-size="11" letter-spacing="6" fill="#3D5A75">ADVANCED PROGRAMMING</text>
        <polygon points="368,470 374,473.5 368,477 362,473.5" fill="#3D5A75"/>
        <polygon points="672,470 678,473.5 672,477 666,473.5" fill="#3D5A75"/>
      </g>
    </svg>
  </div>

  <!-- ═══════════ 00+01 · identity & running process ═══════════ -->
  <section id="identity">
    <header class="sec-head reveal">
      <span class="sec-no">▚▚ 01</span>
      <h2>Identity &amp; Process</h2>
      <span class="sec-line"></span>
      <span class="sec-tag">struct definition · live state</span>
    </header>
    <div class="duo">
      <div class="window reveal">
        <div class="win-bar"><i></i><i></i><i></i><span>src/engineer.rs</span></div>
<pre><code><span class="c">#[repr(C)]</span>
<span class="k">pub struct</span> <span class="t">Engineer</span> {
    name:   &amp;<span class="k">'static</span> <span class="t">str</span>,          <span class="c">// ← put yours here</span>
    role:   <span class="t">Role</span>::StaffSystems,
    stack:  [<span class="t">Layer</span>::Metal, <span class="t">Layer</span>::Kernel, <span class="t">Layer</span>::Runtime, <span class="t">Layer</span>::Language],
    editor: <span class="t">Editor</span>::Neovim,          <span class="c">// tmux · bare-metal energy</span>
}

<span class="k">impl</span> <span class="t">Engineer</span> {
    <span class="k">pub const fn</span> <span class="w">focus</span>() -&gt; [&amp;<span class="k">'static</span> <span class="t">str</span>; <span class="s">3</span>] {
        [<span class="s">"compilers &amp; VMs"</span>, <span class="s">"lock-free systems"</span>, <span class="s">"gpu compute"</span>]
    }
    <span class="k">pub fn</span> <span class="w">ship</span>(&amp;<span class="k">self</span>) -&gt; <span class="t">Proof</span> {
        measure().then_optimize()    <span class="c">// benchmarked, never guessed</span>
    }
}</code></pre>
      </div>
      <div class="window term reveal">
        <div class="win-bar"><i></i><i></i><i></i><span>./whoami --verbose</span></div>
<pre><code><span class="p">$</span> ./whoami --verbose
name        : <span class="v">Kael Voss</span>
timezone    : <span class="v">low-latency (UTC±0)</span>
currently   : verifying a lock-free slab
              allocator in <span class="v">TLA+</span>
reading     : "Engineering a Compiler",
              ch. 9 — again
open_to     : <span class="v">staff/principal systems ·
              compiler OSS · weird GPUs</span>
<span class="p">$</span> <span class="cursor"></span></code></pre>
      </div>
    </div>
    <ul class="bullets reveal">
      <li><b>language machinery</b> — interpreters with inline caches, LLVM passes, shader DSLs</li>
      <li><b>concurrency that survives contact</b> — wait-free queues, epoch reclamation, io_uring pipelines</li>
      <li><b>gpu paths</b> — CUDA kernels tuned to the register, path tracers at 60 fps</li>
    </ul>
  </section>

  <!-- ═══════════ 02 · depth map ═══════════ -->
  <section id="depth">
    <header class="sec-head reveal">
      <span class="sec-no">▚▚ 02</span>
      <h2>Depth Map</h2>
      <span class="sec-line"></span>
      <span class="sec-tag">layers &amp; evidence</span>
    </header>
    <div class="tbl-wrap reveal">
      <table>
        <thead><tr><th>layer</th><th>territory</th><th>evidence</th></tr></thead>
        <tbody>
          <tr><td><span class="layer-chip l0">L0 · metal</span></td><td>SIMD, cache-line choreography, GPU warps</td><td>path tracer, SIMD csv parser</td></tr>
          <tr><td><span class="layer-chip l1">L1 · kernel</span></td><td>eBPF, io_uring, tracing, zero-copy</td><td>profiler, ring buffer</td></tr>
          <tr><td><span class="layer-chip l2">L2 · runtime</span></td><td>allocators, GC-free async, lock-free</td><td>bytecode VM, LSM engine</td></tr>
          <tr><td><span class="layer-chip l3">L3 · language</span></td><td>type systems, MIR passes, codegen</td><td>rustc lint plugin, DSL compiler</td></tr>
          <tr><td><span class="layer-chip l4">L4 · protocol</span></td><td>wire formats, gRPC, WASM at the edge</td><td>mesh PoC, schema compiler</td></tr>
        </tbody>
      </table>
    </div>

    <!-- inlined stack.svg — dual conveyor -->
    <div class="conveyor reveal">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1040 168" role="img" aria-label="tech conveyor">
        <defs>
          <g id="cap">
            <rect x="0" y="5" width="112" height="40" rx="9" fill="#071119"/>
            <rect x="0" y="0" width="112" height="40" rx="9" fill="#132C42" stroke="#2A4A66"/>
          </g>
          <linearGradient id="fl" x1="0" y1="0" x2="1" y2="0">
            <stop offset="0" stop-color="#0C1826"/><stop offset="1" stop-color="#0C1826" stop-opacity="0"/>
          </linearGradient>
          <linearGradient id="fr" x1="1" y1="0" x2="0" y2="0">
            <stop offset="0" stop-color="#0C1826"/><stop offset="1" stop-color="#0C1826" stop-opacity="0"/>
          </linearGradient>
        </defs>
        <rect width="1040" height="168" fill="#0C1826"/>
        <rect width="1040" height="2" fill="#16324A"/>
        <rect y="166" width="1040" height="2" fill="#16324A"/>
        <line x1="0" y1="84" x2="1040" y2="84" stroke="#16324A" stroke-dasharray="2 8"/>
        <g transform="translate(0,20)">
          <g>
            <animateTransform attributeName="transform" type="translate" values="0 0;-1188 0" dur="26s" repeatCount="indefinite"/>
            <g id="r1" font-family="'Segoe UI',system-ui,sans-serif" font-size="14" font-weight="700">
              <g transform="translate(0,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#35E0D0"/><text x="26" y="26" fill="#35E0D0">Rust</text></g>
              <g transform="translate(132,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FFB454"/><text x="26" y="26" fill="#FFB454">C++</text></g>
              <g transform="translate(264,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#B7F05E"/><text x="26" y="26" fill="#B7F05E">Zig</text></g>
              <g transform="translate(396,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#35E0D0"/><text x="26" y="26" fill="#35E0D0">Go</text></g>
              <g transform="translate(528,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FFB454"/><text x="26" y="26" fill="#FFB454">Python</text></g>
              <g transform="translate(660,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FF6B7A"/><text x="26" y="26" fill="#FF6B7A">TypeScript</text></g>
              <g transform="translate(792,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#B7F05E"/><text x="26" y="26" fill="#B7F05E">C</text></g>
              <g transform="translate(924,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#35E0D0"/><text x="26" y="26" fill="#35E0D0">SQL</text></g>
              <g transform="translate(1056,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FFB454"/><text x="26" y="26" fill="#FFB454">Bash</text></g>
            </g>
            <use href="#r1" x="1188"/>
          </g>
        </g>
        <g transform="translate(0,100)">
          <g>
            <animateTransform attributeName="transform" type="translate" values="-1188 0;0 0" dur="34s" repeatCount="indefinite"/>
            <g id="r2" font-family="'Segoe UI',system-ui,sans-serif" font-size="14" font-weight="700">
              <g transform="translate(0,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FF6B7A"/><text x="26" y="26" fill="#FF6B7A">LLVM</text></g>
              <g transform="translate(132,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#B7F05E"/><text x="26" y="26" fill="#B7F05E">CUDA</text></g>
              <g transform="translate(264,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#35E0D0"/><text x="26" y="26" fill="#35E0D0">eBPF</text></g>
              <g transform="translate(396,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FFB454"/><text x="26" y="26" fill="#FFB454">WASM</text></g>
              <g transform="translate(528,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FF6B7A"/><text x="26" y="26" fill="#FF6B7A">Linux</text></g>
              <g transform="translate(660,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#B7F05E"/><text x="26" y="26" fill="#B7F05E">io_uring</text></g>
              <g transform="translate(792,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#35E0D0"/><text x="26" y="26" fill="#35E0D0">Docker</text></g>
              <g transform="translate(924,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FFB454"/><text x="26" y="26" fill="#FFB454">K8s</text></g>
              <g transform="translate(1056,0)"><use href="#cap"/><rect x="10" y="9" width="4" height="27" rx="2" fill="#FF6B7A"/><text x="26" y="26" fill="#FF6B7A">Postgres</text></g>
            </g>
            <use href="#r2" x="1188"/>
          </g>
        </g>
        <rect width="130" height="168" fill="url(#fl)"/>
        <rect x="910" width="130" height="168" fill="url(#fr)"/>
      </svg>
    </div>
  </section>

  <!-- ═══════════ 03 · arsenal ═══════════ -->
  <section id="arsenal">
    <header class="sec-head reveal">
      <span class="sec-no">▚▚ 03</span>
      <h2>Arsenal</h2>
      <span class="sec-line"></span>
      <span class="sec-tag">weapons of choice</span>
    </header>
    <div class="badges reveal">
      <img src="https://img.shields.io/badge/RUST-FFB454?style=for-the-badge&logo=rust&logoColor=0D1B2A" alt="rust">
      <img src="https://img.shields.io/badge/C%2B%2B-FF6B7A?style=for-the-badge&logo=cplusplus&logoColor=0D1B2A" alt="cpp">
      <img src="https://img.shields.io/badge/ZIG-B7F05E?style=for-the-badge&logo=zig&logoColor=0D1B2A" alt="zig">
      <img src="https://img.shields.io/badge/GO-35E0D0?style=for-the-badge&logo=go&logoColor=0D1B2A" alt="go">
      <img src="https://img.shields.io/badge/PYTHON-7FA3C0?style=for-the-badge&logo=python&logoColor=0D1B2A" alt="python">
      <img src="https://img.shields.io/badge/TYPESCRIPT-5AA7E0?style=for-the-badge&logo=typescript&logoColor=0D1B2A" alt="typescript">
    </div>
    <div class="badges reveal">
      <img src="https://img.shields.io/badge/LLVM-FF6B7A?style=for-the-badge&logo=llvm&logoColor=0D1B2A" alt="llvm">
      <img src="https://img.shields.io/badge/CUDA-B7F05E?style=for-the-badge&logo=nvidia&logoColor=0D1B2A" alt="cuda">
      <img src="https://img.shields.io/badge/LINUX-FFB454?style=for-the-badge&logo=linux&logoColor=0D1B2A" alt="linux">
      <img src="https://img.shields.io/badge/DOCKER-35E0D0?style=for-the-badge&logo=docker&logoColor=0D1B2A" alt="docker">
      <img src="https://img.shields.io/badge/K8S-5AA7E0?style=for-the-badge&logo=kubernetes&logoColor=0D1B2A" alt="kubernetes">
      <img src="https://img.shields.io/badge/POSTGRES-7FA3C0?style=for-the-badge&logo=postgresql&logoColor=0D1B2A" alt="postgres">
      <img src="https://img.shields.io/badge/REDIS-FF6B7A?style=for-the-badge&logo=redis&logoColor=0D1B2A" alt="redis">
    </div>
  </section>

  <!-- ═══════════ 04 · featured operations ═══════════ -->
  <section id="ops">
    <header class="sec-head reveal">
      <span class="sec-no">▚▚ 04</span>
      <h2>Featured Operations</h2>
      <span class="sec-line"></span>
      <span class="sec-tag">selected builds</span>
    </header>
    <div class="ops">
      <article class="op-card reveal"><div class="spot"></div>
        <div class="op-top"><code>obsidian</code><span class="op-star">2.1k ★</span></div>
        <p>bytecode VM — inline caches, NaN boxing, tracing JIT</p>
        <div class="op-tags"><span>C++20</span><span>asm</span></div>
      </article>
      <article class="op-card reveal"><div class="spot"></div>
        <div class="op-top"><code>glacier</code><span class="op-star">1.4k ★</span></div>
        <p>LSM storage engine — MVCC, io_uring flushes</p>
        <div class="op-tags"><span>Rust</span></div>
      </article>
      <article class="op-card reveal"><div class="spot"></div>
        <div class="op-top"><code>spectra</code><span class="op-star">980 ★</span></div>
        <p>eBPF profiler — flame graphs, zero agents</p>
        <div class="op-tags"><span>Rust</span><span>eBPF</span></div>
      </article>
      <article class="op-card reveal"><div class="spot"></div>
        <div class="op-top"><code>lumen</code><span class="op-star">760 ★</span></div>
        <p>GPU path tracer — 60 fps @ 1080p, denoised</p>
        <div class="op-tags"><span>CUDA</span><span>OptiX</span></div>
      </article>
    </div>
  </section>

  <!-- ═══════════ 05 · pipeline ═══════════ -->
  <section id="pipeline">
    <header class="sec-head reveal">
      <span class="sec-no">▚▚ 05</span>
      <h2>Pipeline</h2>
      <span class="sec-line"></span>
      <span class="sec-tag">how a program earns its bits</span>
    </header>
    <div class="pipe reveal">
      <div class="stage">
        <h3>▚ front end</h3>
        <div class="nodes"><span class="node">source</span><span class="arr">→</span><span class="node">lexer</span><span class="arr">→</span><span class="node">parser · AST</span></div>
      </div>
      <div class="link"></div>
      <div class="stage">
        <h3>▚ optimizer</h3>
        <div class="nodes"><span class="node">MIR</span><span class="arr">→</span><span class="node">42 passes</span><span class="arr">→</span><span class="node">reg alloc</span></div>
      </div>
      <div class="link"></div>
      <div class="stage">
        <h3>▚ codegen</h3>
        <div class="nodes"><span class="node">asm emit</span><span class="arr">→</span><span class="node">⚙ machine code</span></div>
      </div>
    </div>
  </section>

  <!-- ═══════════ 06 · telemetry ═══════════ -->
  <section id="telemetry">
    <header class="sec-head reveal">
      <span class="sec-no">▚▚ 06</span>
      <h2>Telemetry</h2>
      <span class="sec-line"></span>
      <span class="sec-tag">live from github — swap YOUR_USERNAME</span>
    </header>
    <div class="stats reveal">
      <img loading="lazy" src="https://github-readme-stats.vercel.app/api?username=YOUR_USERNAME&show_icons=true&hide_border=true&rank_icon=percentile&bg_color=0D1B2A&title_color=FFB454&icon_color=35E0D0&text_color=C9D7E8" alt="github stats">
      <img loading="lazy" src="https://streak-stats.demolab.com/?user=YOUR_USERNAME&hide_border=true&background=0D1B2A&ring=FFB454&fire=FF6B7A&currStreakNum=35E0D0&sideNums=C9D7E8&sideLabels=7FA3C0&currStreakLabel=FFB454" alt="streak">
      <img loading="lazy" src="https://github-readme-stats.vercel.app/api/top-langs/?username=YOUR_USERNAME&layout=compact&hide_border=true&bg_color=0D1B2A&title_color=FFB454&text_color=C9D7E8&langs_count=8" alt="top languages">
      <img loading="lazy" src="https://github-readme-activity-graph.vercel.app/graph?username=YOUR_USERNAME&bg_color=0d1b2a&color=35e0d0&line=ffb454&point=ff6b7a&area=true&hide_border=true&custom_title=compile%20activity" alt="activity graph">
    </div>
  </section>

  <!-- inlined divider.svg -->
  <div class="divider reveal">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1040 56" role="img" aria-label="divider">
      <rect width="1040" height="56" fill="#0C1826"/>
      <line x1="0" y1="28" x2="1040" y2="28" stroke="#16324A"/>
      <g>
        <polygon points="40,21 52,28 40,35 28,28" fill="#35E0D0"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="0s" repeatCount="indefinite"/></polygon>
        <polygon points="120,21 132,28 120,35 108,28" fill="#FFB454"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="0.18s" repeatCount="indefinite"/></polygon>
        <polygon points="200,21 212,28 200,35 188,28" fill="#35E0D0"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="0.36s" repeatCount="indefinite"/></polygon>
        <polygon points="280,21 292,28 280,35 268,28" fill="#FFB454"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="0.54s" repeatCount="indefinite"/></polygon>
        <polygon points="360,21 372,28 360,35 348,28" fill="#35E0D0"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="0.72s" repeatCount="indefinite"/></polygon>
        <polygon points="440,21 452,28 440,35 428,28" fill="#FFB454"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="0.9s" repeatCount="indefinite"/></polygon>
        <polygon points="520,21 532,28 520,35 508,28" fill="#FF6B7A"><animate attributeName="opacity" values="0.3;1;0.3" dur="2.6s" begin="1.08s" repeatCount="indefinite"/></polygon>
        <polygon points="600,21 612,28 600,35 588,28" fill="#FFB454"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="1.26s" repeatCount="indefinite"/></polygon>
        <polygon points="680,21 692,28 680,35 668,28" fill="#35E0D0"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="1.44s" repeatCount="indefinite"/></polygon>
        <polygon points="760,21 772,28 760,35 748,28" fill="#FFB454"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="1.62s" repeatCount="indefinite"/></polygon>
        <polygon points="840,21 852,28 840,35 828,28" fill="#35E0D0"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="1.8s" repeatCount="indefinite"/></polygon>
        <polygon points="920,21 932,28 920,35 908,28" fill="#FFB454"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="1.98s" repeatCount="indefinite"/></polygon>
        <polygon points="1000,21 1012,28 1000,35 988,28" fill="#35E0D0"><animate attributeName="opacity" values="0.2;1;0.2" dur="2.6s" begin="2.16s" repeatCount="indefinite"/></polygon>
      </g>
      <circle r="2.5" cy="28" fill="#FFB454">
        <animate attributeName="cx" values="-10;1050" dur="6s" repeatCount="indefinite"/>
        <animate attributeName="opacity" values="0;1;1;0" keyTimes="0;0.08;0.92;1" dur="6s" repeatCount="indefinite"/>
      </circle>
    </svg>
  </div>

  <!-- ═══════════ 07 · handshake ═══════════ -->
  <section id="handshake">
    <header class="sec-head reveal">
      <span class="sec-no">▚▚ 07</span>
      <h2>Handshake</h2>
      <span class="sec-line"></span>
      <span class="sec-tag">establish connection</span>
    </header>
    <div class="badges reveal">
      <a href="mailto:you@kernel.dev"><img src="https://img.shields.io/badge/you@kernel.dev-FF6B7A?style=for-the-badge&logo=protonmail&logoColor=0D1B2A" alt="email"></a>
      <a href="https://yourblog.dev"><img src="https://img.shields.io/badge/things%20compilers%20whisper-FFB454?style=for-the-badge&logo=hackthedocs&logoColor=0D1B2A" alt="blog"></a>
      <a href="https://x.com/yourhandle"><img src="https://img.shields.io/badge/@yourhandle-35E0D0?style=for-the-badge&logo=x&logoColor=0D1B2A" alt="x"></a>
      <img src="https://img.shields.io/badge/gpg%200xDEAD%C2%B7BEEF-B7F05E?style=for-the-badge&logo=gnuprivacyguard&logoColor=0D1B2A" alt="gpg">
    </div>

    <details class="reveal">
      <summary><code>$ sudo cat /root/about.env</code></summary>
<pre><code>        +-----------+
       /|          /|        uptime        : 10y in production
      / |         / |        editor wars   : settled (vim keys win)
     +-----------+  |        tabs vs spaces: 4, obviously
     |  |        | /         mass          : 1 keyboard (+2 spares)
     |  +-------- |/        favorite op   : `perf record -g`
     | /         /
     |/         /
     +---------+

_if your abstraction leaks, i'm probably the one who patched it._</code></pre>
    </details>
  </section>

</main>

<footer>
  ▚ hand-forged svg · single html file · no frameworks were harmed · © 2025 <b>YOUR_USERNAME</b>
</footer>

<script>
  // scroll reveals
  const io = new IntersectionObserver(entries => {
    entries.forEach(e => {
      if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target); }
    });
  }, { threshold: 0.12 });
  document.querySelectorAll('.reveal').forEach((el, i) => {
    el.style.transitionDelay = (i % 4) * 70 + 'ms';
    io.observe(el);
  });

  // top progress bar
  const bar = document.getElementById('progress');
  addEventListener('scroll', () => {
    const h = document.documentElement;
    bar.style.transform = 'scaleX(' + (h.scrollTop / (h.scrollHeight - h.clientHeight)) + ')';
  }, { passive: true });

  // pointer-tracked glow on project cards
  document.querySelectorAll('.op-card').forEach(card => {
    card.addEventListener('mousemove', ev => {
      const r = card.getBoundingClientRect();
      card.style.setProperty('--mx', (ev.clientX - r.left) + 'px');
      card.style.setProperty('--my', (ev.clientY - r.top) + 'px');
    });
  });
</script>
</body>
</html>
