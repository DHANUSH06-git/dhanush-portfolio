[portfolio-3d.html](https://github.com/user-attachments/files/28737599/portfolio-3d.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dhanush · Visual Editor</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,200;0,300;0,400;0,500;1,300&family=Space+Mono&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{margin:0;padding:0;box-sizing:border-box;}
:root{
  --bg:#06060a;--surface:#0e0e14;--border:#1a1a24;
  --accent:#ff2d00;--accent2:#ffc200;--accent3:#00d97e;
  --text:#f0ece5;--muted:#52525e;--card:#0c0c12;
}
html{scroll-behavior:smooth;}
body{background:var(--bg);color:var(--text);font-family:'DM Sans',sans-serif;font-weight:300;overflow-x:hidden;cursor:none;}

/* ── CURSOR ── */
#cur{position:fixed;width:12px;height:12px;background:var(--accent);border-radius:50%;pointer-events:none;z-index:9999;transform:translate(-50%,-50%);mix-blend-mode:difference;}
#cur-ring{position:fixed;width:40px;height:40px;border:1.5px solid rgba(255,45,0,.5);border-radius:50%;pointer-events:none;z-index:9998;transform:translate(-50%,-50%);transition:width .25s,height .25s,border-color .25s,opacity .2s;}
body.hovering #cur-ring{width:60px;height:60px;border-color:var(--accent2);}
body.clicking #cur{transform:translate(-50%,-50%) scale(.6);}

/* ── THREE.JS CANVAS ── */
#bg{position:fixed;inset:0;z-index:0;pointer-events:none;}

/* ── NAV ── */
nav{position:fixed;top:0;left:0;right:0;z-index:500;padding:1.3rem 3rem;display:flex;justify-content:space-between;align-items:center;transition:all .35s;border-bottom:1px solid transparent;}
nav.scrolled{background:rgba(6,6,10,.88);backdrop-filter:blur(18px);border-color:var(--border);}
.logo{font-family:'Bebas Neue',sans-serif;font-size:1.9rem;letter-spacing:.06em;color:var(--text);text-decoration:none;position:relative;}
.logo::after{content:'';position:absolute;bottom:-3px;left:0;width:0;height:2px;background:var(--accent);transition:width .3s;}
.logo:hover::after{width:100%;}
.logo span{color:var(--accent);}
.nav-links{display:flex;gap:2.5rem;list-style:none;}
.nav-links a{font-size:.7rem;letter-spacing:.18em;text-transform:uppercase;color:var(--muted);text-decoration:none;transition:color .2s;position:relative;}
.nav-links a::after{content:'';position:absolute;bottom:-4px;left:0;width:0;height:1px;background:var(--accent);transition:width .25s;}
.nav-links a:hover{color:var(--text);}
.nav-links a:hover::after{width:100%;}
.nav-cta{padding:.5rem 1.2rem!important;border:1px solid rgba(255,45,0,.4)!important;color:var(--accent)!important;}
.nav-cta:hover{background:var(--accent)!important;color:#fff!important;}
.nav-cta::after{display:none!important;}

/* ── HERO ── */
.hero{min-height:100vh;display:flex;flex-direction:column;justify-content:flex-end;padding:0 3rem 4rem;position:relative;z-index:10;}
.hero-badge{display:inline-flex;align-items:center;gap:.6rem;font-family:'Space Mono',monospace;font-size:.62rem;letter-spacing:.2em;text-transform:uppercase;color:var(--accent);border:1px solid rgba(255,45,0,.35);padding:.35rem .9rem;margin-bottom:1.5rem;opacity:0;animation:fadeUp .7s .2s ease forwards;}
.live-dot{width:7px;height:7px;border-radius:50%;background:var(--accent3);animation:pulse 1.4s infinite;}
@keyframes pulse{0%,100%{opacity:1;box-shadow:0 0 0 0 rgba(0,217,126,.5);}50%{opacity:.8;box-shadow:0 0 0 6px rgba(0,217,126,0);}}
.hero-h{font-family:'Bebas Neue',sans-serif;font-size:clamp(5rem,13vw,14rem);line-height:.86;letter-spacing:-.01em;opacity:0;animation:fadeUp .9s .35s ease forwards;}
.hero-h .ol{color:transparent;-webkit-text-stroke:1.5px rgba(240,236,229,.2);}
.hero-h .red{color:var(--accent);}
.hero-sub-row{display:flex;align-items:flex-end;justify-content:space-between;gap:2rem;margin-top:2.5rem;opacity:0;animation:fadeUp .9s .5s ease forwards;}
.hero-desc{font-size:.95rem;line-height:1.85;color:var(--muted);max-width:400px;}
.hero-actions{display:flex;gap:1rem;flex-shrink:0;}
.btn-red{display:inline-flex;align-items:center;gap:.75rem;padding:.9rem 2rem;background:var(--accent);color:#fff;font-size:.72rem;font-weight:500;letter-spacing:.13em;text-transform:uppercase;text-decoration:none;transition:all .2s;}
.btn-red:hover{background:#d42500;transform:translateY(-2px);}
.btn-ghost{display:inline-flex;align-items:center;gap:.75rem;padding:.9rem 2rem;background:transparent;color:var(--text);font-size:.72rem;font-weight:500;letter-spacing:.13em;text-transform:uppercase;text-decoration:none;border:1px solid var(--border);transition:all .2s;}
.btn-ghost:hover{border-color:var(--text);transform:translateY(-2px);}
@keyframes fadeUp{from{opacity:0;transform:translateY(30px);}to{opacity:1;transform:translateY(0);}}

/* ── SCROLL INDICATOR ── */
.scroll-hint{position:absolute;bottom:2rem;left:50%;transform:translateX(-50%);display:flex;flex-direction:column;align-items:center;gap:.5rem;opacity:0;animation:fadeUp .8s .9s ease forwards;}
.scroll-hint span{font-family:'Space Mono',monospace;font-size:.55rem;letter-spacing:.2em;text-transform:uppercase;color:var(--muted);}
.scroll-line{width:1px;height:40px;background:linear-gradient(to bottom,var(--accent),transparent);animation:scrollPulse 1.8s infinite;}
@keyframes scrollPulse{0%{transform:scaleY(0);transform-origin:top;}50%{transform:scaleY(1);transform-origin:top;}51%{transform:scaleY(1);transform-origin:bottom;}100%{transform:scaleY(0);transform-origin:bottom;}}

/* ── TICKER ── */
.ticker{border-top:1px solid var(--border);border-bottom:1px solid var(--border);padding:.85rem 0;overflow:hidden;z-index:10;position:relative;background:var(--bg);}
.ticker-track{display:flex;animation:tick 28s linear infinite;white-space:nowrap;}
.ti{display:inline-flex;align-items:center;gap:1rem;padding:0 2rem;font-size:.66rem;letter-spacing:.18em;text-transform:uppercase;color:var(--muted);flex-shrink:0;}
.ti .dot{color:var(--accent);font-size:.9rem;}
@keyframes tick{from{transform:translateX(0);}to{transform:translateX(-50%);}}

/* ── SECTION COMMON ── */
.section{padding:6rem 3rem;position:relative;z-index:10;}
.section-border{border-top:1px solid var(--border);}
.sec-eyebrow{font-family:'Space Mono',monospace;font-size:.62rem;letter-spacing:.22em;color:var(--accent);text-transform:uppercase;display:flex;align-items:center;gap:1rem;margin-bottom:3.5rem;}
.sec-eyebrow::after{content:'';height:1px;width:50px;background:rgba(255,45,0,.35);}

/* ── WORKS — INSTAGRAM EMBED ── */
.works-section{padding:6rem 3rem;position:relative;z-index:10;border-top:1px solid var(--border);}
.ig-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:1.5rem;margin-bottom:2.5rem;}
.ig-post{
  aspect-ratio:1;position:relative;overflow:hidden;background:var(--card);
  border:1px solid var(--border);cursor:none;
  transition:transform .4s cubic-bezier(.25,.46,.45,.94),border-color .3s;
  transform-style:preserve-3d;
}
.ig-post:hover{transform:perspective(600px) rotateX(-3deg) rotateY(3deg) scale(1.03);border-color:rgba(255,45,0,.3);}
.ig-post:nth-child(even):hover{transform:perspective(600px) rotateX(-3deg) rotateY(-3deg) scale(1.03);}
.ig-post iframe{width:100%;height:100%;border:none;pointer-events:none;}
.ig-cover{
  position:absolute;inset:0;z-index:2;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  background:rgba(6,6,10,.7);
  transition:opacity .35s;
}
.ig-post:hover .ig-cover{opacity:0;pointer-events:none;}
.ig-play{width:56px;height:56px;border:2px solid var(--accent);border-radius:50%;display:flex;align-items:center;justify-content:center;margin-bottom:.8rem;}
.ig-play::after{content:'';width:0;height:0;border-style:solid;border-width:10px 0 10px 18px;border-color:transparent transparent transparent var(--accent);margin-left:4px;}
.ig-cover-label{font-family:'Space Mono',monospace;font-size:.58rem;letter-spacing:.15em;text-transform:uppercase;color:rgba(240,236,229,.5);}
.ig-post-info{position:absolute;bottom:0;left:0;right:0;padding:.9rem 1rem;background:linear-gradient(transparent,rgba(6,6,10,.9));z-index:3;opacity:0;transform:translateY(6px);transition:all .3s;}
.ig-post:hover .ig-post-info{opacity:1;transform:translateY(0);}
.ig-post-cat{font-family:'Space Mono',monospace;font-size:.55rem;letter-spacing:.18em;color:var(--accent);text-transform:uppercase;margin-bottom:.2rem;}
.ig-post-title{font-family:'Bebas Neue',sans-serif;font-size:1.2rem;color:var(--text);letter-spacing:.04em;}

/* ── DRAGGABLE REEL STRIP ── */
.reel-strip-wrap{padding:4rem 0;border-top:1px solid var(--border);position:relative;z-index:10;overflow:hidden;}
.reel-strip-label{font-family:'Space Mono',monospace;font-size:.62rem;letter-spacing:.22em;color:var(--accent);text-transform:uppercase;padding:0 3rem;margin-bottom:1.5rem;display:flex;align-items:center;gap:.8rem;}
.reel-strip-label::after{content:'← drag to explore →';font-size:.55rem;color:var(--muted);letter-spacing:.12em;}
.reel-strip{display:flex;gap:1.2rem;padding:0 3rem 1rem;overflow-x:auto;cursor:grab;user-select:none;scroll-behavior:auto;-ms-overflow-style:none;scrollbar-width:none;}
.reel-strip::-webkit-scrollbar{display:none;}
.reel-strip.dragging{cursor:grabbing;}
.reel-item{flex-shrink:0;width:200px;height:340px;background:var(--card);border:1px solid var(--border);position:relative;overflow:hidden;transition:transform .3s,border-color .3s;}
.reel-item:hover{transform:translateY(-6px);border-color:rgba(255,45,0,.3);}
.reel-item-bg{width:100%;height:75%;display:flex;align-items:center;justify-content:center;font-family:'Bebas Neue',sans-serif;font-size:2.5rem;letter-spacing:.1em;position:relative;overflow:hidden;}
.reel-item-info{padding:.8rem;height:25%;}
.reel-item-tag{font-family:'Space Mono',monospace;font-size:.5rem;letter-spacing:.15em;text-transform:uppercase;color:var(--accent);margin-bottom:.25rem;}
.reel-item-name{font-size:.78rem;font-weight:500;color:var(--text);line-height:1.3;}

/* ── SERVICES (3D tilt) ── */
.services-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:1.5rem;}
.svc{background:var(--card);border:1px solid var(--border);padding:2rem;position:relative;overflow:hidden;transform-style:preserve-3d;transition:border-color .3s;cursor:none;}
.svc::before{content:'';position:absolute;inset:0;background:radial-gradient(circle at var(--mx,50%) var(--my,50%),rgba(255,45,0,.08),transparent 60%);opacity:0;transition:opacity .3s;pointer-events:none;}
.svc:hover::before{opacity:1;}
.svc:hover{border-color:rgba(255,45,0,.22);}
.svc-n{font-family:'Bebas Neue',sans-serif;font-size:4rem;color:rgba(255,45,0,.1);line-height:1;margin-bottom:.5rem;}
.svc-icon{width:44px;height:44px;background:rgba(255,45,0,.07);display:flex;align-items:center;justify-content:center;margin-bottom:1.4rem;}
.svc-icon svg{width:22px;height:22px;stroke:var(--accent);fill:none;stroke-width:1.5;stroke-linecap:round;stroke-linejoin:round;}
.svc-title{font-family:'Bebas Neue',sans-serif;font-size:1.7rem;letter-spacing:.04em;margin-bottom:.6rem;}
.svc-desc{font-size:.8rem;line-height:1.75;color:var(--muted);}
.svc-tags{display:flex;flex-wrap:wrap;gap:.4rem;margin-top:1.2rem;}
.tag{font-family:'Space Mono',monospace;font-size:.56rem;letter-spacing:.1em;text-transform:uppercase;padding:.22rem .55rem;background:rgba(255,45,0,.05);border:1px solid rgba(255,45,0,.14);color:rgba(240,236,229,.45);}

/* ── ABOUT ── */
.about-grid{display:grid;grid-template-columns:1.2fr 1fr;gap:5rem;align-items:start;}
.about-h{font-family:'Bebas Neue',sans-serif;font-size:clamp(3rem,5vw,5.5rem);line-height:.9;margin-bottom:1.8rem;}
.about-h span{color:var(--accent);}
.about-p{font-size:.9rem;line-height:1.92;color:var(--muted);margin-bottom:1rem;}
.about-p.highlight{color:rgba(240,236,229,.55);font-style:italic;border-left:2px solid var(--accent);padding-left:1rem;margin-top:1.4rem;}
.skill-bars{margin-top:2.5rem;}
.sb{margin-bottom:1.3rem;}
.sb-top{display:flex;justify-content:space-between;margin-bottom:.4rem;}
.sb-name{font-size:.72rem;letter-spacing:.09em;text-transform:uppercase;color:var(--text);}
.sb-pct{font-family:'Space Mono',monospace;font-size:.62rem;color:var(--accent);}
.sb-track{height:2px;background:rgba(255,255,255,.05);}
.sb-fill{height:100%;background:linear-gradient(90deg,var(--accent),rgba(255,45,0,.3));transform-origin:left;animation:grow 1.6s ease both;}
@keyframes grow{from{transform:scaleX(0);}to{transform:scaleX(1);}}

/* profile card */
.profile-card{background:var(--card);border:1px solid var(--border);padding:2.2rem;position:sticky;top:7rem;transform-style:preserve-3d;transition:transform .05s linear;}
.av{width:68px;height:68px;border-radius:50%;background:var(--accent);display:flex;align-items:center;justify-content:center;font-family:'Bebas Neue',sans-serif;font-size:1.6rem;color:#fff;letter-spacing:.05em;margin-bottom:1.3rem;box-shadow:0 0 0 5px rgba(255,45,0,.1);}
.pname{font-family:'Bebas Neue',sans-serif;font-size:1.7rem;letter-spacing:.05em;margin-bottom:.2rem;}
.prole{font-size:.68rem;letter-spacing:.14em;color:var(--muted);text-transform:uppercase;margin-bottom:1.8rem;}
.pstat{display:flex;justify-content:space-between;align-items:center;padding:.8rem 0;border-bottom:1px solid var(--border);}
.pstat:last-of-type{border-bottom:none;}
.pslbl{font-size:.66rem;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);}
.psval{font-family:'Bebas Neue',sans-serif;font-size:1.05rem;letter-spacing:.04em;}
.psval.live{color:var(--accent3);}
.pc-ig{display:flex;align-items:center;justify-content:center;gap:.7rem;width:100%;padding:.85rem;margin-top:1.4rem;border:1px solid var(--accent);color:var(--accent);font-size:.68rem;letter-spacing:.14em;text-transform:uppercase;text-decoration:none;transition:all .2s;}
.pc-ig:hover{background:var(--accent);color:#fff;}
.pc-mail{display:flex;align-items:center;justify-content:center;gap:.7rem;width:100%;padding:.85rem;margin-top:.5rem;border:1px solid var(--border);color:var(--muted);font-size:.68rem;letter-spacing:.14em;text-transform:uppercase;text-decoration:none;transition:all .2s;}
.pc-mail:hover{border-color:var(--text);color:var(--text);}

/* ── STATS BAR ── */
.stats-bar{display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:var(--border);border:1px solid var(--border);margin-top:3rem;}
.stat-item{background:var(--card);padding:2rem 1.5rem;text-align:center;}
.stat-num{font-family:'Bebas Neue',sans-serif;font-size:3rem;color:var(--accent);line-height:1;letter-spacing:.02em;}
.stat-lbl{font-size:.7rem;letter-spacing:.1em;text-transform:uppercase;color:var(--muted);margin-top:.4rem;}

/* ── FEATURED ── */
.feat-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:1.2rem;margin-bottom:2rem;}
.fc{background:var(--card);border:1px solid var(--border);padding:1.6rem;position:relative;overflow:hidden;transform-style:preserve-3d;transition:border-color .3s,transform .05s linear;cursor:none;}
.fc::before{content:'';position:absolute;inset:0;background:radial-gradient(circle at var(--mx,50%) var(--my,50%),rgba(255,45,0,.07),transparent 65%);opacity:0;transition:opacity .3s;pointer-events:none;}
.fc:hover::before{opacity:1;}
.fc:hover{border-color:rgba(255,45,0,.22);}
.fc.wide{grid-column:span 2;}
.fc-icon{font-size:1.5rem;margin-bottom:.9rem;display:block;}
.fc-title{font-family:'Bebas Neue',sans-serif;font-size:1.25rem;letter-spacing:.04em;margin-bottom:.5rem;}
.fc-desc{font-size:.78rem;line-height:1.78;color:var(--muted);}
.stat-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:1px;background:var(--border);}
.sg-item{background:var(--card);padding:1.5rem;display:flex;align-items:flex-start;gap:.9rem;}
.sg-icon{font-size:1.1rem;margin-top:.1rem;}
.sg-text strong{display:block;font-size:.74rem;font-weight:500;color:var(--text);letter-spacing:.04em;margin-bottom:.25rem;}
.sg-text span{font-size:.76rem;line-height:1.6;color:var(--muted);}

/* ── CONTACT ── */
.contact-wrap{text-align:center;padding:8rem 3rem;}
.contact-h{font-family:'Bebas Neue',sans-serif;font-size:clamp(5rem,13vw,13rem);line-height:.88;letter-spacing:-.02em;margin-bottom:1.5rem;}
.contact-h .ol{color:transparent;-webkit-text-stroke:1px rgba(240,236,229,.15);}
.contact-sub{font-size:.95rem;line-height:1.82;color:var(--muted);max-width:420px;margin:0 auto 1.5rem;}
.contact-pills{display:flex;justify-content:center;gap:1.5rem;margin-bottom:3rem;flex-wrap:wrap;}
.cpill{display:flex;align-items:center;gap:.6rem;font-size:.8rem;color:var(--muted);padding:.5rem 1rem;border:1px solid var(--border);}
.cpill svg{width:14px;height:14px;stroke:var(--accent);fill:none;stroke-width:1.5;stroke-linecap:round;stroke-linejoin:round;}
.contact-btns{display:flex;justify-content:center;gap:1rem;flex-wrap:wrap;}

/* ── FOOTER ── */
footer{border-top:1px solid var(--border);padding:1.8rem 3rem;display:flex;justify-content:space-between;align-items:center;font-size:.68rem;letter-spacing:.1em;color:var(--muted);text-transform:uppercase;position:relative;z-index:10;}
footer a{color:var(--muted);text-decoration:none;transition:color .2s;}
footer a:hover{color:var(--text);}

/* ── REVEAL ── */
.rv{opacity:0;transform:translateY(28px);transition:opacity .7s ease,transform .7s ease;}
.rv.on{opacity:1;transform:translateY(0);}
.rv2{opacity:0;transform:translateY(28px);transition:opacity .7s .12s ease,transform .7s .12s ease;}
.rv2.on{opacity:1;transform:translateY(0);}

/* ── MOBILE ── */
@media(max-width:768px){
  nav{padding:1.1rem 1.5rem;}
  .hero{padding:0 1.5rem 4rem;}
  .hero-h{font-size:4.2rem;}
  .hero-sub-row{flex-direction:column;align-items:flex-start;}
  .ig-grid{grid-template-columns:1fr 1fr;}
  .services-grid{grid-template-columns:1fr;}
  .about-grid{grid-template-columns:1fr;gap:3rem;}
  .profile-card{position:static;}
  .stats-bar{grid-template-columns:repeat(2,1fr);}
  .feat-grid{grid-template-columns:1fr;}
  .fc.wide{grid-column:span 1;}
  .stat-grid{grid-template-columns:1fr;}
  .contact-h{font-size:4rem;}
  footer{flex-direction:column;gap:.8rem;text-align:center;}
  .section,.works-section,.reel-strip-wrap{padding:4rem 1.5rem;}
  body{cursor:auto;}
  #cur,#cur-ring{display:none;}
}
</style>
</head>
<body>

<canvas id="bg"></canvas>
<div id="cur"></div>
<div id="cur-ring"></div>

<!-- ── NAV ── -->
<nav id="nav">
  <a href="#" class="logo">DN<span>.</span></a>
  <ul class="nav-links">
    <li><a href="#works">Works</a></li>
    <li><a href="#services">Services</a></li>
    <li><a href="#about">About</a></li>
    <li><a href="#contact" class="nav-cta">Hire Me</a></li>
  </ul>
</nav>

<!-- ── HERO ── -->
<section class="hero" id="home">
  <div class="hero-badge"><span class="live-dot"></span>Open to Freelance Work</div>
  <h1 class="hero-h">
    VISUAL<br>
    <span class="red">EDITOR</span><br>
    <span class="ol">&amp; CREATOR</span>
  </h1>
  <div class="hero-sub-row">
    <p class="hero-desc">Cinematic storytelling, motion graphics &amp; high-impact social content. Based in India — working globally.</p>
    <div class="hero-actions">
      <a href="#works" class="btn-red">
        View Works
        <svg width="15" height="15" viewBox="0 0 16 16" fill="none"><path d="M3 8H13M13 8L9 4M13 8L9 12" stroke="white" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </a>
      <a href="mailto:gdhanush447@gmail.com" class="btn-ghost">Get in Touch</a>
    </div>
  </div>
  <div class="scroll-hint">
    <div class="scroll-line"></div>
    <span>Scroll</span>
  </div>
</section>

<!-- ── TICKER ── -->
<div class="ticker">
  <div class="ticker-track" id="tickerTrack">
    <span class="ti">Video Editing <span class="dot">✦</span></span>
    <span class="ti">Poster Design <span class="dot">✦</span></span>
    <span class="ti">Motion Graphics <span class="dot">✦</span></span>
    <span class="ti">CapCut <span class="dot">✦</span></span>
    <span class="ti">After Effects <span class="dot">✦</span></span>
    <span class="ti">Premiere Pro <span class="dot">✦</span></span>
    <span class="ti">Blender 3D <span class="dot">✦</span></span>
    <span class="ti">Color Grading <span class="dot">✦</span></span>
    <span class="ti">Reels &amp; Shorts <span class="dot">✦</span></span>
    <span class="ti">Cinematic Edits <span class="dot">✦</span></span>
    <span class="ti">Visual Storytelling <span class="dot">✦</span></span>
    <span class="ti">Video Editing <span class="dot">✦</span></span>
    <span class="ti">Poster Design <span class="dot">✦</span></span>
    <span class="ti">Motion Graphics <span class="dot">✦</span></span>
    <span class="ti">CapCut <span class="dot">✦</span></span>
    <span class="ti">After Effects <span class="dot">✦</span></span>
    <span class="ti">Premiere Pro <span class="dot">✦</span></span>
    <span class="ti">Blender 3D <span class="dot">✦</span></span>
    <span class="ti">Color Grading <span class="dot">✦</span></span>
    <span class="ti">Reels &amp; Shorts <span class="dot">✦</span></span>
    <span class="ti">Cinematic Edits <span class="dot">✦</span></span>
    <span class="ti">Visual Storytelling <span class="dot">✦</span></span>
  </div>
</div>

<!-- ── WORKS ── -->
<section class="works-section" id="works">
  <div class="sec-eyebrow rv">Best Works — @dnus6h</div>

  <!-- Instagram embed grid -->
  <div class="ig-grid rv">

    <!-- Post 1 -->
    <div class="ig-post">
      <div class="ig-cover">
        <div class="ig-play"></div>
        <span class="ig-cover-label">Hover to preview</span>
      </div>
      <div class="ig-post-info">
        <div class="ig-post-cat">Video Edit</div>
        <div class="ig-post-title">Cinematic Reel</div>
      </div>
      <blockquote class="instagram-media" data-instgrm-permalink="https://www.instagram.com/dnus6h/" data-instgrm-version="14" style="background:#fff;border:0;margin:0;max-width:100%;min-width:100%;padding:0;width:100%;height:100%;"></blockquote>
    </div>

    <!-- Post 2 -->
    <div class="ig-post">
      <div class="ig-cover">
        <div class="ig-play"></div>
        <span class="ig-cover-label">Hover to preview</span>
      </div>
      <div class="ig-post-info">
        <div class="ig-post-cat">Motion Graphics</div>
        <div class="ig-post-title">Title Sequence</div>
      </div>
      <div style="width:100%;height:100%;background:#0a0a0a;display:flex;align-items:center;justify-content:center;flex-direction:column;gap:1rem;">
        <div style="width:60px;height:60px;border:2px solid #ff2d00;border-radius:50%;display:flex;align-items:center;justify-content:center;">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#ff2d00" stroke-width="1.5"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1" fill="#ff2d00" stroke="none"/></svg>
        </div>
        <a href="https://www.instagram.com/dnus6h/" target="_blank" style="font-family:'Space Mono',monospace;font-size:.6rem;letter-spacing:.15em;color:#ff2d00;text-decoration:none;text-transform:uppercase;">View @dnus6h</a>
      </div>
    </div>

    <!-- Post 3 -->
    <div class="ig-post">
      <div class="ig-cover">
        <div class="ig-play"></div>
        <span class="ig-cover-label">Hover to preview</span>
      </div>
      <div class="ig-post-info">
        <div class="ig-post-cat">Poster Design</div>
        <div class="ig-post-title">Visual Poster</div>
      </div>
      <div style="width:100%;height:100%;background:#f0ead8;display:flex;flex-direction:column;padding:1.5rem;justify-content:space-between;">
        <div style="font-family:'Bebas Neue',sans-serif;font-size:2.5rem;color:#111;line-height:.9;">POSTER<br>DESIGN</div>
        <div>
          <div style="width:32px;height:3px;background:#ff2d00;margin-bottom:.7rem;"></div>
          <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:4px;">
            <div style="height:22px;background:#222;"></div>
            <div style="height:22px;background:#ccc5b5;"></div>
            <div style="height:22px;background:#222;"></div>
          </div>
        </div>
      </div>
    </div>

  </div>

  <!-- CTA to full IG -->
  <div style="display:flex;align-items:center;justify-content:space-between;padding:.8rem 0 0;" class="rv2">
    <p style="font-size:.8rem;color:var(--muted);">More works on Instagram ↗</p>
    <a href="https://www.instagram.com/dnus6h/" target="_blank" class="btn-ghost" style="font-size:.68rem;padding:.7rem 1.5rem;">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor" stroke="none"/></svg>
      @dnus6h
    </a>
  </div>
</section>

<!-- ── DRAGGABLE REEL STRIP ── -->
<div class="reel-strip-wrap">
  <div class="reel-strip-label">Project Types</div>
  <div class="reel-strip" id="reelStrip">

    <div class="reel-item">
      <div class="reel-item-bg" style="background:#0a0a0a;">
        <div style="position:absolute;inset:0;display:flex;align-items:center;justify-content:center;">
          <div style="width:50px;height:50px;border:1.5px solid #ff2d00;border-radius:50%;display:flex;align-items:center;justify-content:center;"><div style="width:0;height:0;border-style:solid;border-width:8px 0 8px 14px;border-color:transparent transparent transparent #ff2d00;margin-left:3px;"></div></div>
        </div>
        <span style="position:absolute;bottom:.8rem;left:.8rem;font-family:'Space Mono',monospace;font-size:.5rem;letter-spacing:.12em;color:rgba(255,255,255,.3);text-transform:uppercase;">Cinematic</span>
      </div>
      <div class="reel-item-info">
        <div class="reel-item-tag">Video</div>
        <div class="reel-item-name">Cinematic Edit</div>
      </div>
    </div>

    <div class="reel-item">
      <div class="reel-item-bg" style="background:#f0ead8;">
        <div style="padding:1rem;width:100%;display:flex;flex-direction:column;justify-content:flex-end;height:100%;">
          <div style="font-family:'Bebas Neue',sans-serif;font-size:2rem;color:#111;line-height:.9;">TYPE<br>WORK</div>
          <div style="width:24px;height:2px;background:#ff2d00;margin-top:.5rem;"></div>
        </div>
      </div>
      <div class="reel-item-info">
        <div class="reel-item-tag">Design</div>
        <div class="reel-item-name">Poster Design</div>
      </div>
    </div>

    <div class="reel-item">
      <div class="reel-item-bg" style="background:#04080f;overflow:hidden;">
        <div style="position:absolute;inset:0;background-image:linear-gradient(rgba(255,194,0,.06) 1px,transparent 1px),linear-gradient(90deg,rgba(255,194,0,.06) 1px,transparent 1px);background-size:30px 30px;animation:gm 8s linear infinite;"></div>
        <span style="position:relative;z-index:1;font-family:'Bebas Neue',sans-serif;font-size:2rem;color:#ffc200;text-shadow:0 0 30px rgba(255,194,0,.5);">MOTION</span>
      </div>
      <div class="reel-item-info">
        <div class="reel-item-tag">Motion</div>
        <div class="reel-item-name">Motion Graphics</div>
      </div>
    </div>

    <div class="reel-item">
      <div class="reel-item-bg" style="background:#ff2d00;">
        <div style="padding:1rem;width:100%;height:100%;display:flex;flex-direction:column;justify-content:flex-end;">
          <div style="font-family:'Bebas Neue',sans-serif;font-size:1.6rem;color:rgba(255,255,255,.2);line-height:1;margin-bottom:.3rem;">05</div>
          <div style="font-family:'Bebas Neue',sans-serif;font-size:1.8rem;color:#fff;line-height:1;">BRAND<br>VIDEO</div>
        </div>
      </div>
      <div class="reel-item-info">
        <div class="reel-item-tag">Brand</div>
        <div class="reel-item-name">Promo Video</div>
      </div>
    </div>

    <div class="reel-item">
      <div class="reel-item-bg" style="background:#060a14;overflow:hidden;position:relative;">
        <div style="position:absolute;inset:0;display:flex;align-items:center;justify-content:center;">
          <div style="position:absolute;width:100px;height:100px;border:1px solid rgba(255,194,0,.3);border-radius:50%;animation:rsp 5s linear infinite;"></div>
          <div style="position:absolute;width:60px;height:60px;border:1px solid rgba(255,194,0,.5);border-radius:50%;animation:rsp 3s linear infinite reverse;"></div>
          <div style="width:8px;height:8px;background:#ffc200;border-radius:50%;box-shadow:0 0 15px rgba(255,194,0,.7);"></div>
        </div>
        <span style="position:absolute;bottom:.8rem;left:.8rem;font-family:'Space Mono',monospace;font-size:.5rem;letter-spacing:.12em;color:rgba(255,194,0,.5);text-transform:uppercase;">Blender</span>
      </div>
      <div class="reel-item-info">
        <div class="reel-item-tag">3D</div>
        <div class="reel-item-name">3D / Blender</div>
      </div>
    </div>

    <div class="reel-item">
      <div class="reel-item-bg" style="background:#0a0a0a;position:relative;overflow:hidden;">
        <div style="position:absolute;inset:0;background:repeating-linear-gradient(0deg,rgba(255,45,0,.04) 0px,rgba(255,45,0,.04) 1px,transparent 1px,transparent 4px);"></div>
        <div style="position:relative;z-index:1;padding:1rem;height:100%;display:flex;flex-direction:column;justify-content:flex-end;">
          <div style="font-family:'Bebas Neue',sans-serif;font-size:1.8rem;color:#f0ece5;line-height:.9;">COLOR<br>GRADE</div>
        </div>
      </div>
      <div class="reel-item-info">
        <div class="reel-item-tag">Grade</div>
        <div class="reel-item-name">Color Grading</div>
      </div>
    </div>

    <div class="reel-item">
      <div class="reel-item-bg" style="background:#060610;position:relative;overflow:hidden;">
        <div style="position:absolute;inset:0;display:flex;align-items:center;justify-content:center;">
          <div style="text-align:center;">
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="#ff2d00" stroke-width="1" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1.5" fill="#ff2d00" stroke="none"/></svg>
            <div style="font-family:'Space Mono',monospace;font-size:.5rem;letter-spacing:.15em;color:rgba(255,45,0,.7);text-transform:uppercase;margin-top:.5rem;">Reels</div>
          </div>
        </div>
      </div>
      <div class="reel-item-info">
        <div class="reel-item-tag">Social</div>
        <div class="reel-item-name">Instagram Reels</div>
      </div>
    </div>

  </div>
</div>

<!-- ── SERVICES ── -->
<section class="section section-border" id="services">
  <div class="sec-eyebrow rv">What I Do</div>
  <div class="services-grid rv">

    <div class="svc" data-tilt>
      <div class="svc-n">01</div>
      <div class="svc-icon"><svg viewBox="0 0 24 24"><polygon points="5,3 19,12 5,21"/></svg></div>
      <div class="svc-title">Video Editing</div>
      <div class="svc-desc">Precision cuts, smooth transitions, and cinematic grades. From short-form reels to full productions — every second counts.</div>
      <div class="svc-tags"><span class="tag">CapCut</span><span class="tag">Premiere Pro</span><span class="tag">DaVinci</span></div>
    </div>

    <div class="svc" data-tilt>
      <div class="svc-n">02</div>
      <div class="svc-icon"><svg viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="1"/><path d="M3 9h18M9 21V9"/></svg></div>
      <div class="svc-title">Poster Design</div>
      <div class="svc-desc">Bold typographic posters, event flyers, and campaign visuals with a raw editorial edge. Every layout is intentional.</div>
      <div class="svc-tags"><span class="tag">Photoshop</span><span class="tag">Illustrator</span></div>
    </div>

    <div class="svc" data-tilt>
      <div class="svc-n">03</div>
      <div class="svc-icon"><svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="9"/><path d="M12 3v9l5 3"/></svg></div>
      <div class="svc-title">Motion Graphics</div>
      <div class="svc-desc">Kinetic titles, animated logos, and motion loops. Building visual worlds that move with purpose and rhythm.</div>
      <div class="svc-tags"><span class="tag">After Effects</span><span class="tag">CapCut</span></div>
    </div>

    <div class="svc" data-tilt>
      <div class="svc-n">04</div>
      <div class="svc-icon"><svg viewBox="0 0 24 24"><path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/></svg></div>
      <div class="svc-title">3D Design</div>
      <div class="svc-desc">Exploring depth and dimension with Blender — adding 3D elements, renders, and animations to creative projects.</div>
      <div class="svc-tags"><span class="tag">Blender</span><span class="tag">3D Modelling</span></div>
    </div>

    <div class="svc" data-tilt style="grid-column:span 2;">
      <div class="svc-n">05</div>
      <div class="svc-icon"><svg viewBox="0 0 24 24"><path d="M22 12h-4l-3 9L9 3l-3 9H2"/></svg></div>
      <div class="svc-title">Color Grading &amp; Brand Videos</div>
      <div class="svc-desc">Professional colour grading, mood creation, and exposure correction — transforming raw footage into cinematic experiences. Plus full promotional and brand video edits crafted to showcase products and services memorably.</div>
      <div class="svc-tags"><span class="tag">Premiere Pro</span><span class="tag">DaVinci</span><span class="tag">Lumetri</span><span class="tag">Brand Identity</span></div>
    </div>

  </div>
</section>

<!-- ── ABOUT ── -->
<section class="section section-border" id="about">
  <div class="sec-eyebrow rv">About Me</div>
  <div class="about-grid">
    <div class="rv">
      <h2 class="about-h">CRAFT<br>MEETS<br><span>VISION</span></h2>
      <p class="about-p">I'm Dhanush, a creative video editor specialising in cinematic storytelling, high impact social media content, and engaging visual experiences. My goal is simple: transform raw footage into content that captures attention, increases audience engagement, and leaves a lasting impression.</p>
      <p class="about-p">I combine precise editing, professional colour grading, smooth transitions, and strong storytelling to create visually appealing videos that keep viewers watching. Every frame is carefully crafted to match the client's vision while maintaining a high standard of quality and creativity.</p>
      <p class="about-p">Whether it's short-form content, promotional videos, travel films, cinematic edits, or brand-focused projects, I deliver results that help creators, businesses, and brands stand out in a crowded digital space. Great editing is not just about making videos look good — it's about creating content that connects with people, builds audiences, and drives impact.</p>
      <p class="about-p highlight">If you're looking for an editor who values creativity, attention to detail, and delivering work that exceeds expectations, you're in the right place. Let's create something unforgettable.</p>
      <div class="skill-bars">
        <div class="sb"><div class="sb-top"><span class="sb-name">Video Editing</span><span class="sb-pct">95%</span></div><div class="sb-track"><div class="sb-fill" style="width:95%;animation-delay:.1s;"></div></div></div>
        <div class="sb"><div class="sb-top"><span class="sb-name">Poster Design</span><span class="sb-pct">90%</span></div><div class="sb-track"><div class="sb-fill" style="width:90%;animation-delay:.25s;"></div></div></div>
        <div class="sb"><div class="sb-top"><span class="sb-name">Motion Graphics</span><span class="sb-pct">88%</span></div><div class="sb-track"><div class="sb-fill" style="width:88%;animation-delay:.4s;"></div></div></div>
        <div class="sb"><div class="sb-top"><span class="sb-name">Color Grading</span><span class="sb-pct">85%</span></div><div class="sb-track"><div class="sb-fill" style="width:85%;animation-delay:.55s;"></div></div></div>
        <div class="sb"><div class="sb-top"><span class="sb-name">After Effects</span><span class="sb-pct">82%</span></div><div class="sb-track"><div class="sb-fill" style="width:82%;animation-delay:.7s;"></div></div></div>
        <div class="sb"><div class="sb-top"><span class="sb-name">3D / Blender</span><span class="sb-pct">55%</span></div><div class="sb-track"><div class="sb-fill" style="width:55%;animation-delay:.85s;"></div></div></div>
      </div>
    </div>
    <div class="rv2">
      <div class="profile-card" id="profileCard">
        <div class="av">DN</div>
        <div class="pname">DHANUSH</div>
        <div class="prole">Visual Editor · Designer · Creator</div>
        <div class="pstat"><span class="pslbl">Handle</span><span class="psval">@dnus6h</span></div>
        <div class="pstat"><span class="pslbl">Specialty</span><span class="psval">Video &amp; Motion</span></div>
        <div class="pstat"><span class="pslbl">Location</span><span class="psval">India</span></div>
        <div class="pstat"><span class="pslbl">Tools</span><span class="psval">CC · AE · Pr · Blender</span></div>
        <div class="pstat"><span class="pslbl">Status</span><span class="psval live">● Available</span></div>
        <a href="https://www.instagram.com/dnus6h/" target="_blank" class="pc-ig">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor" stroke="none"/></svg>
          @dnus6h on Instagram
        </a>
        <a href="mailto:gdhanush447@gmail.com" class="pc-mail">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M2 8l10 7 10-7"/></svg>
          gdhanush447@gmail.com
        </a>
      </div>
    </div>
  </div>

  <!-- Stats -->
  <div class="stats-bar rv" style="margin-top:4rem;">
    <div class="stat-item"><div class="stat-num">100+</div><div class="stat-lbl">Hours Editing</div></div>
    <div class="stat-item"><div class="stat-num">50+</div><div class="stat-lbl">Projects Done</div></div>
    <div class="stat-item"><div class="stat-num">5+</div><div class="stat-lbl">Platforms</div></div>
    <div class="stat-item"><div class="stat-num">∞</div><div class="stat-lbl">Ideas</div></div>
  </div>
</section>

<!-- ── FEATURED PROJECTS ── -->
<section class="section section-border" id="featured">
  <div class="sec-eyebrow rv">Featured Projects</div>
  <div class="feat-grid rv">
    <div class="fc" data-tilt><span class="fc-icon">🎬</span><div class="fc-title">Cinematic Edits</div><div class="fc-desc">High-quality cinematic videos with professional colour grading, smooth transitions, sound design, and storytelling techniques that create an immersive viewing experience.</div></div>
    <div class="fc" data-tilt><span class="fc-icon">📱</span><div class="fc-title">Social Media Content</div><div class="fc-desc">Short-form content optimised for Instagram Reels, YouTube Shorts, and TikTok, designed to maximise engagement, retention, and shareability.</div></div>
    <div class="fc" data-tilt><span class="fc-icon">🎨</span><div class="fc-title">Color Grading</div><div class="fc-desc">Transforming raw footage with cinematic colour grading, mood creation, exposure correction, and visual consistency.</div></div>
    <div class="fc" data-tilt><span class="fc-icon">✨</span><div class="fc-title">Motion Graphics &amp; Effects</div><div class="fc-desc">Creative text animations, visual effects, transitions, and motion graphics that enhance storytelling and make content more dynamic.</div></div>
    <div class="fc wide" data-tilt><span class="fc-icon">🚀</span><div class="fc-title">Promotional &amp; Brand Videos</div><div class="fc-desc">Professional edits crafted to showcase products, services, and brands in a compelling and memorable way — built to convert viewers into customers and help brands stand out.</div></div>
    <div class="fc" data-tilt><span class="fc-icon">🧊</span><div class="fc-title">3D Design — Blender</div><div class="fc-desc">Exploring 3D modelling, rendering, and animation in Blender — adding depth, dimension, and immersive visuals to creative projects.</div></div>
  </div>
  <div class="stat-grid rv">
    <div class="sg-item"><span class="sg-icon">📈</span><div class="sg-text"><strong>100+ Hours of Editing Experience</strong><span>Dedicated time behind the timeline crafting high-quality content.</span></div></div>
    <div class="sg-item"><span class="sg-icon">🎬</span><div class="sg-text"><strong>Multiple Projects Completed</strong><span>Cinematic &amp; social media projects delivered and published.</span></div></div>
    <div class="sg-item"><span class="sg-icon">📱</span><div class="sg-text"><strong>Short-Form Platform Expert</strong><span>Content edited for Instagram Reels &amp; short-form platforms.</span></div></div>
    <div class="sg-item"><span class="sg-icon">⚡</span><div class="sg-text"><strong>Fast Turnaround</strong><span>Quick delivery without compromising on quality or attention to detail.</span></div></div>
  </div>
</section>

<!-- ── CONTACT ── -->
<section class="contact-wrap section-border" id="contact">
  <div class="sec-eyebrow rv" style="justify-content:center;">Let's Create</div>
  <h2 class="contact-h rv">LET'S<br><span class="ol">MAKE IT</span></h2>
  <p class="contact-sub rv">Got a project? I'm open to video edits, poster designs, motion work and creative collabs. Let's build something unforgettable.</p>
  <div class="contact-pills rv">
    <div class="cpill">
      <svg viewBox="0 0 24 24"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M2 8l10 7 10-7"/></svg>
      gdhanush447@gmail.com
    </div>
    <div class="cpill">
      <svg viewBox="0 0 24 24"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1.5" fill="var(--accent)" stroke="none"/></svg>
      @dnus6h
    </div>
  </div>
  <div class="contact-btns rv">
    <a href="https://www.instagram.com/dnus6h/" target="_blank" class="btn-red">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="2" width="20" height="20" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r="1" fill="currentColor" stroke="none"/></svg>
      DM on Instagram
    </a>
    <a href="mailto:gdhanush447@gmail.com" class="btn-ghost">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><rect x="2" y="4" width="20" height="16" rx="2"/><path d="M2 8l10 7 10-7"/></svg>
      gdhanush447@gmail.com
    </a>
  </div>
</section>

<footer>
  <div>© 2025 dnus6h — Dhanush</div>
  <div>Visual Artist · India</div>
  <div><a href="https://www.instagram.com/dnus6h/" target="_blank">@dnus6h</a> · <a href="mailto:gdhanush447@gmail.com">gdhanush447@gmail.com</a></div>
</footer>

<!-- Instagram embed script -->
<script async src="https://www.instagram.com/embed.js"></script>

<!-- Three.js -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
// ── THREE.JS ──────────────────────────────────────────────────────
const canvas = document.getElementById('bg');
const renderer = new THREE.WebGLRenderer({canvas, antialias:true, alpha:true});
renderer.setPixelRatio(Math.min(devicePixelRatio,2));
renderer.setSize(innerWidth, innerHeight);

const scene = new THREE.Scene();
const cam = new THREE.PerspectiveCamera(60, innerWidth/innerHeight, 0.1, 100);
cam.position.z = 5;

// particles
const N = 2000, ppos = new Float32Array(N*3), pcol = new Float32Array(N*3);
for(let i=0;i<N;i++){
  ppos[i*3]=(Math.random()-.5)*22; ppos[i*3+1]=(Math.random()-.5)*22; ppos[i*3+2]=(Math.random()-.5)*14;
  const hot=Math.random()>.87;
  pcol[i*3]=hot?1:.07; pcol[i*3+1]=hot?.18:.07; pcol[i*3+2]=hot?0:.1;
}
const pgeo = new THREE.BufferGeometry();
pgeo.setAttribute('position',new THREE.BufferAttribute(ppos,3));
pgeo.setAttribute('color',new THREE.BufferAttribute(pcol,3));
const pts = new THREE.Points(pgeo, new THREE.PointsMaterial({size:.035,vertexColors:true,transparent:true,opacity:.65}));
scene.add(pts);

// wireframes
const icoA = new THREE.Mesh(new THREE.IcosahedronGeometry(2.5,1), new THREE.MeshBasicMaterial({color:0xff2d00,wireframe:true,transparent:true,opacity:.035}));
const icoB = new THREE.Mesh(new THREE.IcosahedronGeometry(4.2,1), new THREE.MeshBasicMaterial({color:0xffc200,wireframe:true,transparent:true,opacity:.018}));
scene.add(icoA,icoB);

let mx=0,my=0,t=0;
document.addEventListener('mousemove',e=>{mx=(e.clientX/innerWidth-.5)*2;my=-(e.clientY/innerHeight-.5)*2;});
(function loop(){
  requestAnimationFrame(loop); t+=.004;
  pts.rotation.y=t*.07+mx*.09; pts.rotation.x=t*.025+my*.04;
  icoA.rotation.x=t*.11; icoA.rotation.y=t*.16+mx*.12;
  icoB.rotation.x=-t*.06+my*.07; icoB.rotation.y=-t*.1;
  cam.position.x+=(mx*.35-cam.position.x)*.04;
  cam.position.y+=(my*.28-cam.position.y)*.04;
  renderer.render(scene,cam);
})();
addEventListener('resize',()=>{cam.aspect=innerWidth/innerHeight;cam.updateProjectionMatrix();renderer.setSize(innerWidth,innerHeight);});

// ── CURSOR ───────────────────────────────────────────────────────
const cur=document.getElementById('cur'), curR=document.getElementById('cur-ring');
let cx=0,cy=0,rx=0,ry=0;
addEventListener('mousemove',e=>{cx=e.clientX;cy=e.clientY;});
addEventListener('mousedown',()=>document.body.classList.add('clicking'));
addEventListener('mouseup',()=>document.body.classList.remove('clicking'));
document.querySelectorAll('a,button,[data-tilt],.svc,.fc,.ig-post,.reel-item').forEach(el=>{
  el.addEventListener('mouseenter',()=>document.body.classList.add('hovering'));
  el.addEventListener('mouseleave',()=>document.body.classList.remove('hovering'));
});
(function curLoop(){
  cur.style.left=cx+'px'; cur.style.top=cy+'px';
  rx+=(cx-rx)*.12; ry+=(cy-ry)*.12;
  curR.style.left=rx+'px'; curR.style.top=ry+'px';
  requestAnimationFrame(curLoop);
})();

// ── NAV SCROLL ───────────────────────────────────────────────────
const nav=document.getElementById('nav');
addEventListener('scroll',()=>nav.classList.toggle('scrolled',scrollY>60));

// ── SCROLL REVEAL ────────────────────────────────────────────────
const revObs=new IntersectionObserver(es=>es.forEach(e=>{if(e.isIntersecting)e.target.classList.add('on');}),{threshold:.08});
document.querySelectorAll('.rv,.rv2').forEach(el=>revObs.observe(el));

// ── 3D TILT (services, feat cards) ───────────────────────────────
document.querySelectorAll('[data-tilt],.svc,.fc').forEach(el=>{
  el.addEventListener('mousemove',e=>{
    const r=el.getBoundingClientRect();
    const x=(e.clientX-r.left)/r.width, y=(e.clientY-r.top)/r.height;
    el.style.transform=`perspective(900px) rotateX(${(y-.5)*14}deg) rotateY(${(x-.5)*-14}deg) translateZ(8px)`;
    el.style.setProperty('--mx',(x*100)+'%'); el.style.setProperty('--my',(y*100)+'%');
  });
  el.addEventListener('mouseleave',()=>{el.style.transform='perspective(900px) rotateX(0) rotateY(0) translateZ(0)';});
});

// ── PROFILE CARD TILT ────────────────────────────────────────────
const pc=document.getElementById('profileCard');
if(pc){
  pc.addEventListener('mousemove',e=>{const r=pc.getBoundingClientRect();const x=(e.clientX-r.left)/r.width,y=(e.clientY-r.top)/r.height;pc.style.transform=`perspective(800px) rotateX(${(y-.5)*10}deg) rotateY(${(x-.5)*-10}deg)`;});
  pc.addEventListener('mouseleave',()=>pc.style.transform='');
}

// ── DRAGGABLE REEL STRIP ─────────────────────────────────────────
const strip=document.getElementById('reelStrip');
let isDown=false, startX, scrollLeft;
strip.addEventListener('mousedown',e=>{isDown=true;strip.classList.add('dragging');startX=e.pageX-strip.offsetLeft;scrollLeft=strip.scrollLeft;});
strip.addEventListener('mouseleave',()=>{isDown=false;strip.classList.remove('dragging');});
strip.addEventListener('mouseup',()=>{isDown=false;strip.classList.remove('dragging');});
strip.addEventListener('mousemove',e=>{if(!isDown)return;e.preventDefault();const x=e.pageX-strip.offsetLeft;strip.scrollLeft=scrollLeft-(x-startX)*1.4;});
// touch
strip.addEventListener('touchstart',e=>{startX=e.touches[0].pageX-strip.offsetLeft;scrollLeft=strip.scrollLeft;},{passive:true});
strip.addEventListener('touchmove',e=>{const x=e.touches[0].pageX-strip.offsetLeft;strip.scrollLeft=scrollLeft-(x-startX);},{passive:true});

// ── IG COVER: click to reveal ────────────────────────────────────
document.querySelectorAll('.ig-post').forEach(post=>{
  const cover=post.querySelector('.ig-cover');
  if(cover) cover.addEventListener('click',()=>{cover.style.opacity='0';cover.style.pointerEvents='none';});
});
</script>
</body>
</html>
