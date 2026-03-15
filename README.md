<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>TradeLite — Invest with Confidence</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=Fraunces:ital,wght@0,400;0,700;0,900;1,400;1,700&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --saffron: #FF6B00;
      --saffron-light: #FF8C38;
      --saffron-pale: rgba(255,107,0,0.08);
      --saffron-glow: rgba(255,107,0,0.15);
      --jade: #00875A;
      --jade-light: #00A86B;
      --jade-pale: rgba(0,135,90,0.08);
      --navy: #0A1628;
      --navy2: #0F1E35;
      --navy3: #162540;
      --card: #111D2E;
      --card2: #0D1826;
      --border: rgba(255,255,255,0.07);
      --border2: rgba(255,255,255,0.04);
      --text: #F0F4FF;
      --muted: #7A8FAD;
      --muted2: #3D526B;
      --red: #FF4D6A;
      --gold: #F5C842;
    }

    html { scroll-behavior: smooth; }

    body {
      font-family: 'Plus Jakarta Sans', sans-serif;
      background: var(--navy);
      color: var(--text);
      overflow-x: hidden;
      line-height: 1.6;
    }

    ::-webkit-scrollbar { width: 4px; }
    ::-webkit-scrollbar-track { background: var(--navy); }
    ::-webkit-scrollbar-thumb { background: var(--muted2); border-radius: 4px; }

    /* ── NOISE & GRID OVERLAYS ── */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.035'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 9998;
    }

    /* ── ANIMATIONS ── */
    @keyframes fadeUp   { from{opacity:0;transform:translateY(24px)} to{opacity:1;transform:translateY(0)} }
    @keyframes fadeIn   { from{opacity:0} to{opacity:1} }
    @keyframes ticker   { 0%{transform:translateX(0)} 100%{transform:translateX(-50%)} }
    @keyframes blink    { 0%,100%{opacity:1} 50%{opacity:0.2} }
    @keyframes chartDraw{ from{stroke-dashoffset:800} to{stroke-dashoffset:0} }
    @keyframes floatUp  { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-8px)} }
    @keyframes shimmer  { 0%{background-position:-200% center} 100%{background-position:200% center} }
    @keyframes growBar  { from{transform:scaleY(0)} to{transform:scaleY(1)} }
    @keyframes ripple   { 0%{transform:scale(1);opacity:0.6} 100%{transform:scale(2.2);opacity:0} }

    /* ── NAV ── */
    nav {
      position: fixed; top:0; left:0; right:0; z-index:200;
      display:flex; align-items:center; justify-content:space-between;
      padding:0 5%; height:68px;
      background:rgba(10,22,40,0.88);
      backdrop-filter:blur(24px);
      border-bottom:1px solid var(--border);
    }
    .logo-wrap { display:flex; align-items:center; gap:0.6rem; }
    .logo-icon {
      width:34px; height:34px;
      background:linear-gradient(135deg,var(--saffron),#FF4500);
      border-radius:8px;
      display:flex; align-items:center; justify-content:center;
      font-size:0.9rem; font-weight:800; color:#fff;
      box-shadow:0 0 16px var(--saffron-glow);
    }
    .logo-text {
      font-family:'Fraunces',serif; font-weight:900; font-size:1.3rem;
      letter-spacing:-0.5px; color:var(--text);
    }
    .logo-text span { color:var(--saffron); }
    .nav-links { display:flex; gap:2rem; list-style:none; }
    .nav-links a { color:var(--muted); text-decoration:none; font-size:0.875rem; font-weight:500; transition:color .2s; }
    .nav-links a:hover { color:var(--text); }
    .nav-badge {
      background:rgba(255,107,0,0.12); border:1px solid rgba(255,107,0,0.25);
      color:var(--saffron); font-size:0.68rem; padding:0.1rem 0.45rem;
      border-radius:4px; font-weight:600; margin-left:0.3rem; vertical-align:middle;
    }
    .nav-actions { display:flex; gap:0.75rem; align-items:center; }
    .btn-outline {
      border:1px solid var(--border); background:transparent; color:var(--text);
      padding:0.5rem 1.15rem; border-radius:8px; font-family:'Plus Jakarta Sans',sans-serif;
      font-size:0.85rem; font-weight:500; cursor:pointer; transition:all .2s;
    }
    .btn-outline:hover { border-color:var(--muted); background:rgba(255,255,255,0.04); }
    .btn-saffron {
      background:linear-gradient(135deg,var(--saffron),#FF4500);
      color:#fff; border:none; padding:0.5rem 1.3rem; border-radius:8px;
      font-family:'Plus Jakarta Sans',sans-serif; font-size:0.85rem; font-weight:600;
      cursor:pointer; transition:all .2s;
      box-shadow:0 4px 14px rgba(255,107,0,0.3);
    }
    .btn-saffron:hover { transform:translateY(-1px); box-shadow:0 6px 20px rgba(255,107,0,0.4); }
    .hamburger { display:none; flex-direction:column; gap:5px; cursor:pointer; }
    .hamburger span { display:block; width:22px; height:2px; background:var(--text); border-radius:2px; }

    /* ── TICKER ── */
    .ticker-bar {
      position:fixed; top:68px; left:0; right:0; z-index:199;
      height:30px; overflow:hidden; background:var(--navy2);
      border-bottom:1px solid var(--border2);
      display:flex; align-items:center;
    }
    .ticker-scroll { display:flex; animation:ticker 28s linear infinite; white-space:nowrap; }
    .tick { display:inline-flex; align-items:center; gap:0.35rem; padding:0 1.2rem; font-size:0.72rem; }
    .tick .ts { color:var(--text); font-weight:600; }
    .tick .tp { color:var(--muted); }
    .tick .tu { color:var(--jade-light); }
    .tick .td { color:var(--red); }
    .nse-tag { font-size:0.65rem; color:var(--muted2); margin-right:1rem; padding-left:1rem; border-left:1px solid var(--border); }

    /* ── HERO ── */
    .hero {
      min-height:100vh; padding:130px 5% 80px;
      position:relative; overflow:hidden;
      display:flex; align-items:center;
    }
    .hero-mesh {
      position:absolute; inset:0; pointer-events:none;
      background:
        radial-gradient(ellipse 60% 50% at 70% 40%, rgba(255,107,0,0.06) 0%, transparent 65%),
        radial-gradient(ellipse 40% 40% at 20% 70%, rgba(0,135,90,0.05) 0%, transparent 60%),
        radial-gradient(ellipse 80% 60% at 50% 100%, rgba(10,22,40,0.8) 0%, transparent 80%);
    }
    .hero-grid-lines {
      position:absolute; inset:0; pointer-events:none;
      background-image:
        linear-gradient(rgba(255,255,255,0.015) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.015) 1px, transparent 1px);
      background-size:60px 60px;
      mask-image:radial-gradient(ellipse 80% 80% at 50% 50%, black 30%, transparent 80%);
    }
    .hero-inner { max-width:1200px; margin:0 auto; width:100%; display:grid; grid-template-columns:1fr 1fr; gap:4rem; align-items:center; position:relative; }

    .hero-eyebrow {
      display:inline-flex; align-items:center; gap:0.5rem;
      background:rgba(255,107,0,0.08); border:1px solid rgba(255,107,0,0.2);
      border-radius:999px; padding:0.3rem 1rem; font-size:0.75rem;
      color:var(--saffron); margin-bottom:1.5rem;
      animation:fadeIn 0.5s ease both;
    }
    .blink-dot { width:6px;height:6px;border-radius:50%;background:var(--saffron);animation:blink 1.4s ease-in-out infinite; }
    .hero-h1 {
      font-family:'Fraunces',serif; font-size:clamp(2.4rem,4.5vw,3.8rem);
      font-weight:900; line-height:1.05; letter-spacing:-1.5px;
      margin-bottom:1.4rem;
      animation:fadeUp 0.6s ease 0.1s both;
    }
    .hero-h1 em { font-style:italic; color:var(--saffron); }
    .hero-h1 .green-word { color:var(--jade-light); }
    .hero-p {
      color:var(--muted); font-size:1rem; line-height:1.75; max-width:460px;
      font-weight:400; margin-bottom:2rem;
      animation:fadeUp 0.6s ease 0.2s both;
    }
    .hero-actions { display:flex; gap:1rem; flex-wrap:wrap; margin-bottom:2.5rem; animation:fadeUp 0.6s ease 0.3s both; }
    .btn-hero-primary {
      background:linear-gradient(135deg,var(--saffron),#E55A00);
      color:#fff; border:none; padding:0.9rem 2rem; border-radius:10px;
      font-family:'Plus Jakarta Sans',sans-serif; font-size:0.95rem; font-weight:600;
      cursor:pointer; transition:all .25s;
      box-shadow:0 6px 24px rgba(255,107,0,0.35);
      position:relative; overflow:hidden;
    }
    .btn-hero-primary:hover { transform:translateY(-2px); box-shadow:0 10px 30px rgba(255,107,0,0.45); }
    .btn-hero-secondary {
      background:rgba(255,255,255,0.05); border:1px solid var(--border);
      color:var(--text); padding:0.9rem 2rem; border-radius:10px;
      font-family:'Plus Jakarta Sans',sans-serif; font-size:0.95rem; font-weight:500;
      cursor:pointer; transition:all .2s;
    }
    .btn-hero-secondary:hover { background:rgba(255,255,255,0.09); border-color:var(--muted2); }
    .hero-trust { display:flex; align-items:center; gap:1.5rem; animation:fadeUp 0.6s ease 0.4s both; }
    .trust-avatars { display:flex; }
    .trust-av {
      width:32px;height:32px;border-radius:50%;border:2px solid var(--navy);
      background:linear-gradient(135deg,var(--saffron),#FF4500);
      display:flex;align-items:center;justify-content:center;font-size:0.7rem;font-weight:700;
      margin-left:-8px; color:#fff;
    }
    .trust-av:first-child { margin-left:0; }
    .trust-text { font-size:0.82rem; color:var(--muted); }
    .trust-text strong { color:var(--text); }
    .hero-stats-row { display:flex; gap:2rem; margin-top:2.5rem; animation:fadeUp 0.6s ease 0.5s both; }
    .hstat .hnum { font-family:'Fraunces',serif; font-size:1.6rem; font-weight:700; }
    .hstat .hlbl { font-size:0.73rem; color:var(--muted); margin-top:0.1rem; }
    .hstat .hnum.saffron { color:var(--saffron); }
    .hstat .hnum.jade { color:var(--jade-light); }

    /* ── HERO APP MOCKUP ── */
    .app-mockup {
      animation:fadeUp 0.8s ease 0.3s both;
      position:relative;
    }
    .app-mockup::before {
      content:'';
      position:absolute;
      top:-20%; left:-10%; right:-10%; bottom:-10%;
      background:radial-gradient(ellipse, rgba(255,107,0,0.07) 0%, transparent 65%);
      pointer-events:none;
    }
    .phone-frame {
      background:var(--card); border:1px solid var(--border);
      border-radius:28px; padding:1.5rem;
      box-shadow:0 40px 80px rgba(0,0,0,0.5), 0 0 0 1px rgba(255,255,255,0.04);
      position:relative; overflow:hidden;
      animation:floatUp 5s ease-in-out infinite;
    }
    .phone-frame::before {
      content:'';
      position:absolute; inset:0;
      background:linear-gradient(135deg,rgba(255,107,0,0.03) 0%,transparent 50%);
      pointer-events:none;
    }
    .pf-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:1.2rem; }
    .pf-greeting { font-size:0.75rem; color:var(--muted); }
    .pf-name { font-size:0.95rem; font-weight:700; margin-top:0.1rem; }
    .pf-notif {
      width:32px;height:32px;border-radius:50%;
      background:rgba(255,255,255,0.06); border:1px solid var(--border);
      display:flex;align-items:center;justify-content:center;font-size:0.85rem;
    }
    .pf-portfolio-card {
      background:linear-gradient(135deg,var(--saffron) 0%,#E05500 100%);
      border-radius:16px; padding:1.25rem; margin-bottom:1rem;
      position:relative; overflow:hidden;
    }
    .pf-portfolio-card::after {
      content:'';
      position:absolute; top:-30%;right:-20%;
      width:140px;height:140px;border-radius:50%;
      background:rgba(255,255,255,0.08);
    }
    .pfc-label { font-size:0.7rem; color:rgba(255,255,255,0.7); margin-bottom:0.3rem; }
    .pfc-value { font-family:'Fraunces',serif; font-size:1.9rem; font-weight:900; color:#fff; line-height:1; }
    .pfc-change { font-size:0.75rem; color:rgba(255,255,255,0.85); margin-top:0.35rem; }
    .pfc-bar { margin-top:1rem; }
    .pfc-bar-track { height:3px; background:rgba(255,255,255,0.25); border-radius:3px; }
    .pfc-bar-fill { height:3px; background:#fff; border-radius:3px; width:67%; }
    .pfc-bar-lbl { display:flex; justify-content:space-between; font-size:0.65rem; color:rgba(255,255,255,0.7); margin-top:0.3rem; }

    .pf-section-title { font-size:0.72rem; color:var(--muted); font-weight:600; text-transform:uppercase; letter-spacing:1px; margin-bottom:0.75rem; }
    .pf-stocks { display:flex; flex-direction:column; gap:0.5rem; }
    .pf-stock-row {
      display:flex; align-items:center; justify-content:space-between;
      padding:0.6rem 0.75rem; background:var(--card2);
      border:1px solid var(--border2); border-radius:10px;
    }
    .pfs-left { display:flex; align-items:center; gap:0.6rem; }
    .pfs-logo {
      width:28px;height:28px;border-radius:7px;
      display:flex;align-items:center;justify-content:center;font-size:0.65rem;font-weight:700;
    }
    .pfs-sym { font-size:0.8rem; font-weight:700; }
    .pfs-name { font-size:0.65rem; color:var(--muted); margin-top:0.05rem; }
    .pfs-right { text-align:right; }
    .pfs-price { font-size:0.82rem; font-weight:600; }
    .pfs-chg { font-size:0.68rem; margin-top:0.05rem; }
    .pfs-chg.up { color:var(--jade-light); }
    .pfs-chg.dn { color:var(--red); }

    .pf-bottom-nav {
      display:flex; justify-content:space-around; margin-top:1rem;
      padding-top:1rem; border-top:1px solid var(--border2);
    }
    .pbn-item { display:flex; flex-direction:column; align-items:center; gap:0.2rem; font-size:0.62rem; color:var(--muted); cursor:pointer; }
    .pbn-item.active { color:var(--saffron); }
    .pbn-icon { font-size:1rem; }

    /* floating cards */
    .float-card {
      position:absolute;
      background:var(--card); border:1px solid var(--border);
      border-radius:12px; padding:0.75rem 1rem;
      box-shadow:0 8px 24px rgba(0,0,0,0.4);
      font-size:0.75rem;
    }
    .float-card-1 { top:-16px; right:-20px; }
    .float-card-2 { bottom:40px; left:-24px; }
    .fc-label { color:var(--muted); font-size:0.65rem; margin-bottom:0.15rem; }
    .fc-val { font-weight:700; font-size:0.9rem; }
    .fc-val.jade { color:var(--jade-light); }
    .fc-val.saffron { color:var(--saffron); }

    /* ── SEBI COMPLIANCE BAND ── */
    .compliance-band {
      background:var(--navy2); border-top:1px solid var(--border); border-bottom:1px solid var(--border);
      padding:1.2rem 5%;
    }
    .compliance-inner { max-width:1200px; margin:0 auto; display:flex; align-items:center; justify-content:space-between; flex-wrap:wrap; gap:1rem; }
    .comp-item { display:flex; align-items:center; gap:0.6rem; }
    .comp-icon { width:32px;height:32px;border-radius:8px;background:var(--jade-pale);display:flex;align-items:center;justify-content:center;font-size:0.9rem; }
    .comp-text .ct { font-size:0.8rem; font-weight:600; }
    .comp-text .cs { font-size:0.7rem; color:var(--muted); }
    .comp-divider { width:1px;height:28px;background:var(--border); }

    /* ── SECTIONS ── */
    section { padding:90px 5%; }
    .sec-inner { max-width:1200px; margin:0 auto; }
    .sec-chip {
      display:inline-flex; align-items:center; gap:0.4rem;
      font-size:0.7rem; font-weight:700; text-transform:uppercase; letter-spacing:2px;
      color:var(--saffron); margin-bottom:1rem;
    }
    .sec-chip-jade { color:var(--jade-light); }
    .sec-h2 {
      font-family:'Fraunces',serif; font-size:clamp(1.8rem,3vw,2.8rem);
      font-weight:900; letter-spacing:-1px; line-height:1.08; margin-bottom:0.9rem;
    }
    .sec-p { color:var(--muted); font-size:0.95rem; line-height:1.75; max-width:520px; }

    /* ── FEATURES ── */
    .features-section { background:var(--navy2); }
    .features-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:1.25rem; margin-top:3rem; }
    .feat-card {
      background:var(--card); border:1px solid var(--border);
      border-radius:18px; padding:1.75rem;
      transition:border-color .25s, transform .25s;
      position:relative; overflow:hidden;
    }
    .feat-card::before {
      content:'';
      position:absolute; top:0; left:0; right:0; height:2px;
      background:linear-gradient(90deg,transparent,var(--saffron),transparent);
      opacity:0; transition:opacity .3s;
    }
    .feat-card:hover { transform:translateY(-5px); border-color:rgba(255,107,0,0.2); }
    .feat-card:hover::before { opacity:1; }
    .feat-icon-wrap {
      width:48px;height:48px;border-radius:12px;
      display:flex;align-items:center;justify-content:center;font-size:1.4rem;
      margin-bottom:1.25rem;
    }
    .feat-icon-saffron { background:rgba(255,107,0,0.1); }
    .feat-icon-jade { background:rgba(0,135,90,0.1); }
    .feat-icon-gold { background:rgba(245,200,66,0.1); }
    .feat-title { font-family:'Fraunces',serif; font-size:1.05rem; font-weight:700; margin-bottom:0.6rem; }
    .feat-desc { font-size:0.855rem; color:var(--muted); line-height:1.7; }
    .feat-tag {
      display:inline-block; margin-top:1rem;
      font-size:0.68rem; font-weight:700;
      background:rgba(255,107,0,0.08); color:var(--saffron);
      padding:0.2rem 0.6rem; border-radius:4px; letter-spacing:0.5px;
    }
    .feat-tag.jade { background:rgba(0,135,90,0.08); color:var(--jade-light); }

    /* ── EDUCATION SECTION ── */
    .edu-grid { display:grid; grid-template-columns:1fr 1fr; gap:4rem; align-items:center; }
    .edu-visual { position:relative; }
    .edu-card {
      background:var(--card); border:1px solid var(--border); border-radius:20px; overflow:hidden;
    }
    .edu-card-header {
      background:linear-gradient(135deg,var(--jade) 0%,#005C40 100%);
      padding:1.5rem;
    }
    .ech-title { font-family:'Fraunces',serif; font-size:1.1rem; font-weight:700; color:#fff; margin-bottom:0.3rem; }
    .ech-sub { font-size:0.78rem; color:rgba(255,255,255,0.7); }
    .edu-modules { padding:1.25rem; display:flex; flex-direction:column; gap:0.6rem; }
    .edu-module {
      display:flex; align-items:center; gap:0.85rem;
      padding:0.75rem; border-radius:10px; background:var(--card2);
      border:1px solid var(--border2); cursor:default;
      transition:border-color .2s;
    }
    .edu-module:hover { border-color:rgba(0,135,90,0.3); }
    .em-icon {
      width:36px;height:36px;border-radius:8px;
      display:flex;align-items:center;justify-content:center;font-size:0.95rem;
      flex-shrink:0;
    }
    .em-text .et { font-size:0.82rem; font-weight:600; }
    .em-text .es { font-size:0.7rem; color:var(--muted); margin-top:0.1rem; }
    .em-progress { margin-left:auto; }
    .em-prog-ring {
      width:28px;height:28px;border-radius:50%;
      background:conic-gradient(var(--jade-light) var(--p), rgba(255,255,255,0.1) 0);
      display:flex;align-items:center;justify-content:center;font-size:0.58rem;font-weight:700;
    }
    .em-done {
      width:20px;height:20px;border-radius:50%;
      background:var(--jade-pale);border:1px solid var(--jade);
      display:flex;align-items:center;justify-content:center;font-size:0.65rem;color:var(--jade-light);
    }
    .edu-float {
      position:absolute; right:-24px; top:50%;
      transform:translateY(-50%);
      background:var(--card); border:1px solid var(--border);
      border-radius:14px; padding:1rem 1.2rem;
      box-shadow:0 12px 32px rgba(0,0,0,0.4);
    }
    .ef-q { font-size:0.78rem; font-weight:600; margin-bottom:0.7rem; max-width:160px; line-height:1.4; }
    .ef-opts { display:flex; flex-direction:column; gap:0.4rem; }
    .ef-opt {
      background:var(--card2); border:1px solid var(--border2);
      border-radius:7px; padding:0.45rem 0.75rem; font-size:0.72rem; cursor:pointer; transition:all .15s;
    }
    .ef-opt:hover { border-color:var(--saffron); }
    .ef-opt.correct { border-color:var(--jade); background:var(--jade-pale); color:var(--jade-light); }

    /* ── RISK SECTION ── */
    .risk-section { background:var(--navy2); }
    .risk-grid { display:grid; grid-template-columns:1fr 1fr; gap:4rem; align-items:center; }
    .risk-visual { display:flex; flex-direction:column; gap:1rem; }
    .risk-meter-card {
      background:var(--card); border:1px solid var(--border); border-radius:18px; padding:1.5rem;
    }
    .rm-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:1.25rem; }
    .rm-title { font-size:0.875rem; font-weight:700; }
    .rm-profile-badge {
      background:rgba(255,107,0,0.1); border:1px solid rgba(255,107,0,0.2);
      color:var(--saffron); font-size:0.68rem; font-weight:700;
      padding:0.2rem 0.6rem; border-radius:6px;
    }
    .rm-gauge { position:relative; height:90px; margin-bottom:1rem; }
    .rm-labels { display:flex; justify-content:space-between; font-size:0.65rem; color:var(--muted); }
    .risk-bars { display:flex; flex-direction:column; gap:0.6rem; }
    .rb-row { display:flex; align-items:center; gap:0.75rem; }
    .rb-label { font-size:0.75rem; color:var(--muted); width:80px; flex-shrink:0; }
    .rb-track { flex:1; height:6px; background:rgba(255,255,255,0.06); border-radius:6px; overflow:hidden; }
    .rb-fill { height:100%; border-radius:6px; transform-origin:left; animation:growBar 1.5s cubic-bezier(0.34,1.56,0.64,1) both; }
    .rb-val { font-size:0.72rem; font-weight:600; width:36px; text-align:right; }

    .portfolio-breakdown {
      background:var(--card); border:1px solid var(--border); border-radius:18px; padding:1.5rem;
    }
    .pb-title { font-size:0.875rem; font-weight:700; margin-bottom:1.1rem; }
    .pb-items { display:flex; flex-direction:column; gap:0.6rem; }
    .pb-item { display:flex; align-items:center; gap:0.75rem; }
    .pb-dot { width:10px;height:10px;border-radius:50%;flex-shrink:0; }
    .pb-name { font-size:0.8rem; flex:1; }
    .pb-bar-wrap { width:120px; height:5px; background:rgba(255,255,255,0.06); border-radius:5px; overflow:hidden; }
    .pb-bar { height:100%; border-radius:5px; }
    .pb-pct { font-size:0.75rem; color:var(--muted); width:36px; text-align:right; }

    /* ── HOW IT WORKS ── */
    .how-row { display:flex; gap:0; margin-top:3rem; position:relative; }
    .how-row::before {
      content:'';
      position:absolute; top:28px; left:calc(14% + 28px); right:calc(14% + 28px);
      height:1px; background:linear-gradient(90deg, var(--saffron), var(--jade-light));
      opacity:0.25;
    }
    .how-step {
      flex:1; display:flex; flex-direction:column; align-items:center; text-align:center; padding:0 1rem;
    }
    .how-num-wrap { position:relative; margin-bottom:1.25rem; }
    .how-num {
      width:56px;height:56px;border-radius:50%;
      background:var(--card); border:2px solid var(--border);
      display:flex;align-items:center;justify-content:center;
      font-family:'Fraunces',serif; font-size:1.2rem; font-weight:900;
      color:var(--saffron); position:relative; z-index:1;
      transition:all .3s;
    }
    .how-step:hover .how-num {
      border-color:var(--saffron);
      box-shadow:0 0 0 6px var(--saffron-pale);
    }
    .how-title { font-weight:700; font-size:0.9rem; margin-bottom:0.5rem; }
    .how-desc { font-size:0.8rem; color:var(--muted); line-height:1.65; }
    .how-emoji { font-size:1.4rem; margin-bottom:0.5rem; }

    /* ── MARKETS TABLE ── */
    .markets-section { background:var(--navy2); }
    .mkt-tabs { display:flex; gap:0.5rem; margin:2rem 0 1.5rem; flex-wrap:wrap; }
    .mkt-tab {
      background:transparent; border:1px solid var(--border);
      color:var(--muted); padding:0.45rem 1.1rem; border-radius:8px;
      font-size:0.82rem; font-weight:500; cursor:pointer;
      font-family:'Plus Jakarta Sans',sans-serif; transition:all .15s;
    }
    .mkt-tab.active { background:var(--saffron); color:#fff; border-color:var(--saffron); font-weight:600; }
    .mkt-tab:hover:not(.active) { border-color:var(--muted2); color:var(--text); }
    .mkt-table { width:100%; border-collapse:collapse; }
    .mkt-table th {
      text-align:left; font-size:0.7rem; color:var(--muted2);
      text-transform:uppercase; letter-spacing:1px;
      padding:0.6rem 0.9rem; border-bottom:1px solid var(--border);
      font-weight:600;
    }
    .mkt-table td { padding:0.9rem 0.9rem; font-size:0.875rem; border-bottom:1px solid var(--border2); }
    .mkt-table tr:hover td { background:rgba(255,255,255,0.015); }
    .td-logo {
      width:28px;height:28px;border-radius:7px;
      display:inline-flex;align-items:center;justify-content:center;
      font-size:0.62rem;font-weight:800;margin-right:0.65rem;vertical-align:middle;
    }
    .td-sym { font-weight:700; }
    .td-name { color:var(--muted); font-size:0.76rem; }
    .td-up { color:var(--jade-light); font-weight:600; }
    .td-dn { color:var(--red); font-weight:600; }
    .sebi-note {
      margin-top:1.5rem; padding:0.75rem 1rem;
      background:rgba(0,135,90,0.06); border:1px solid rgba(0,135,90,0.15);
      border-radius:10px; font-size:0.75rem; color:var(--muted);
      display:flex; align-items:center; gap:0.5rem;
    }

    /* ── TESTIMONIALS ── */
    .test-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:1.25rem; margin-top:3rem; }
    .test-card {
      background:var(--card); border:1px solid var(--border);
      border-radius:18px; padding:1.75rem;
      transition:transform .25s, border-color .25s;
    }
    .test-card:hover { transform:translateY(-4px); border-color:rgba(255,107,0,0.15); }
    .test-stars { color:var(--gold); font-size:0.82rem; margin-bottom:1rem; letter-spacing:2px; }
    .test-q { font-size:0.88rem; color:var(--muted); line-height:1.72; margin-bottom:1.25rem; font-style:italic; }
    .test-author { display:flex; align-items:center; gap:0.75rem; }
    .ta-av {
      width:38px;height:38px;border-radius:50%;
      display:flex;align-items:center;justify-content:center;
      font-size:0.8rem;font-weight:700;color:#fff;flex-shrink:0;
    }
    .ta-name { font-size:0.85rem; font-weight:600; }
    .ta-role { font-size:0.72rem; color:var(--muted); margin-top:0.1rem; }
    .ta-city { font-size:0.68rem; color:var(--muted2); }

    /* ── CTA ── */
    .cta-section {
      text-align:center; padding:100px 5%;
      position:relative; overflow:hidden;
    }
    .cta-bg {
      position:absolute; inset:0; pointer-events:none;
      background:
        radial-gradient(ellipse 60% 60% at 50% 50%, rgba(255,107,0,0.07) 0%, transparent 70%),
        radial-gradient(ellipse 40% 40% at 30% 80%, rgba(0,135,90,0.05) 0%, transparent 60%);
    }
    .cta-h2 {
      font-family:'Fraunces',serif; font-size:clamp(2rem,4vw,3.2rem);
      font-weight:900; letter-spacing:-1.5px; margin-bottom:1rem;
      position:relative;
    }
    .cta-h2 em { font-style:italic; color:var(--saffron); }
    .cta-p { color:var(--muted); font-size:1rem; margin-bottom:2.5rem; position:relative; }
    .cta-form { display:flex; gap:0.75rem; max-width:420px; margin:0 auto; position:relative; flex-wrap:wrap; justify-content:center; }
    .cta-inp {
      flex:1; min-width:200px;
      background:var(--card); border:1px solid var(--border);
      color:var(--text); padding:0.875rem 1.2rem; border-radius:10px;
      font-family:'Plus Jakarta Sans',sans-serif; font-size:0.9rem; outline:none;
      transition:border-color .2s;
    }
    .cta-inp:focus { border-color:var(--saffron); }
    .cta-inp::placeholder { color:var(--muted2); }
    .cta-disclaimer { font-size:0.7rem; color:var(--muted2); margin-top:1rem; position:relative; }
    .sebi-chip {
      display:inline-flex;align-items:center;gap:0.4rem;
      background:rgba(0,135,90,0.1);border:1px solid rgba(0,135,90,0.2);
      color:var(--jade-light);font-size:0.7rem;font-weight:600;
      padding:0.25rem 0.75rem;border-radius:999px;margin-bottom:1.5rem;
      position:relative;
    }

    /* ── FOOTER ── */
    footer {
      background:var(--navy2); border-top:1px solid var(--border);
      padding:3.5rem 5% 2rem;
    }
    .footer-inner {
      max-width:1200px; margin:0 auto;
      display:grid; grid-template-columns:2fr 1fr 1fr 1fr; gap:3rem;
      padding-bottom:2.5rem; border-bottom:1px solid var(--border);
    }
    .footer-brand .logo-text { display:block; font-family:'Fraunces',serif; font-size:1.3rem; font-weight:900; margin-bottom:0.75rem; }
    .footer-brand .logo-text span { color:var(--saffron); }
    .footer-brand p { font-size:0.82rem; color:var(--muted); line-height:1.65; max-width:240px; }
    .footer-sebi { margin-top:1rem; font-size:0.72rem; color:var(--muted2); line-height:1.6; max-width:240px; }
    .footer-col h4 { font-size:0.75rem; font-weight:700; text-transform:uppercase; letter-spacing:1.5px; color:var(--muted2); margin-bottom:1.1rem; }
    .footer-col a { display:block; color:var(--muted); text-decoration:none; font-size:0.83rem; margin-bottom:0.65rem; transition:color .2s; }
    .footer-col a:hover { color:var(--text); }
    .footer-bottom {
      max-width:1200px; margin:1.5rem auto 0;
      display:flex; justify-content:space-between; align-items:center;
      font-size:0.73rem; color:var(--muted2); flex-wrap:wrap; gap:0.5rem;
    }

    /* ── RESPONSIVE ── */
    @media(max-width:1024px){
      .hero-inner{grid-template-columns:1fr;gap:3rem;}
      .phone-frame{max-width:420px;margin:0 auto;}
      .edu-grid,.risk-grid{grid-template-columns:1fr;gap:2.5rem;}
      .edu-float{display:none;}
      .features-grid{grid-template-columns:repeat(2,1fr);}
      .test-grid{grid-template-columns:repeat(2,1fr);}
      .footer-inner{grid-template-columns:1fr 1fr;gap:2rem;}
      .how-row::before{display:none;}
    }
    @media(max-width:768px){
      .nav-links{display:none;}
      .hamburger{display:flex;}
      .features-grid{grid-template-columns:1fr;}
      .how-row{flex-direction:column;gap:2rem;}
      .test-grid{grid-template-columns:1fr;}
      .mkt-tabs{gap:0.4rem;}
      .compliance-inner{flex-direction:column;align-items:flex-start;}
      .comp-divider{display:none;}
      .footer-inner{grid-template-columns:1fr 1fr;}
      .hero-actions{flex-direction:column;}
    }
    @media(max-width:480px){
      .footer-inner{grid-template-columns:1fr;}
      .footer-bottom{flex-direction:column;text-align:center;}
      .hero-stats-row{gap:1rem;flex-wrap:wrap;}
      .cta-form{flex-direction:column;}
    }
  </style>
</head>
<body>
<div id="root"></div>
<script type="text/babel">
const { useState } = React;

const NSE_TICKERS = [
  {sym:'RELIANCE',p:'2,948.55',chg:'+1.42%',up:true},
  {sym:'TCS',p:'3,812.20',chg:'+0.78%',up:true},
  {sym:'INFY',p:'1,456.75',chg:'-0.34%',up:false},
  {sym:'HDFCBANK',p:'1,678.40',chg:'+0.91%',up:true},
  {sym:'SBIN',p:'812.60',chg:'+2.15%',up:true},
  {sym:'WIPRO',p:'487.30',chg:'-0.55%',up:false},
  {sym:'BAJFINANCE',p:'7,124.00',chg:'+1.88%',up:true},
  {sym:'NIFTY50',p:'22,456.80',chg:'+0.62%',up:true},
  {sym:'SENSEX',p:'73,812.40',chg:'+0.54%',up:true},
  {sym:'ICICIBANK',p:'1,089.15',chg:'-0.12%',up:false},
];

const MARKET_DATA = {
  'Nifty 50':[
    {sym:'RELIANCE',name:'Reliance Industries',p:'₹2,948.55',chg:'+1.42%',mktcap:'₹19.97L Cr',vol:'4.2M',up:true,logo:'RI',bg:'#c0392b'},
    {sym:'TCS',name:'Tata Consultancy Svcs',p:'₹3,812.20',chg:'+0.78%',mktcap:'₹13.87L Cr',vol:'2.8M',up:true,logo:'TC',bg:'#1a6b9e'},
    {sym:'HDFCBANK',name:'HDFC Bank Ltd',p:'₹1,678.40',chg:'+0.91%',mktcap:'₹12.69L Cr',vol:'6.1M',up:true,logo:'HB',bg:'#1a4e8c'},
    {sym:'INFY',name:'Infosys Limited',p:'₹1,456.75',chg:'-0.34%',mktcap:'₹6.06L Cr',vol:'5.3M',up:false,logo:'IF',bg:'#006DB7'},
    {sym:'SBIN',name:'State Bank of India',p:'₹812.60',chg:'+2.15%',mktcap:'₹7.27L Cr',vol:'18.4M',up:true,logo:'SB',bg:'#1a3b5e'},
  ],
  'Mid Cap':[
    {sym:'BAJFINANCE',name:'Bajaj Finance Ltd',p:'₹7,124.00',chg:'+1.88%',mktcap:'₹4.32L Cr',vol:'1.2M',up:true,logo:'BF',bg:'#7b3fa0'},
    {sym:'HCLTECH',name:'HCL Technologies',p:'₹1,312.40',chg:'-0.43%',mktcap:'₹3.57L Cr',vol:'3.4M',up:false,logo:'HC',bg:'#0096DB'},
    {sym:'TATAMOTORS',name:'Tata Motors Ltd',p:'₹978.35',chg:'+3.21%',mktcap:'₹3.61L Cr',vol:'11.8M',up:true,logo:'TM',bg:'#003880'},
    {sym:'WIPRO',name:'Wipro Limited',p:'₹487.30',chg:'-0.55%',mktcap:'₹2.54L Cr',vol:'7.2M',up:false,logo:'WP',bg:'#341f6e'},
  ],
  'IPOs':[
    {sym:'IXIGO',name:'Le Travenues Tech',p:'₹196.80',chg:'+4.32%',mktcap:'₹12,400 Cr',vol:'8.4M',up:true,logo:'IX',bg:'#e8503a'},
    {sym:'JYOTIRDA',name:'Jyoti CNC Automation',p:'₹1,248.60',chg:'+2.11%',mktcap:'₹9,820 Cr',vol:'1.1M',up:true,logo:'JC',bg:'#2e7d32'},
    {sym:'DIABECON',name:'Diabecon Pharma',p:'₹312.40',chg:'-1.22%',mktcap:'₹2,180 Cr',vol:'2.3M',up:false,logo:'DP',bg:'#546e7a'},
  ],
};

const FEATURES = [
  {icon:'🎓',title:'Learn Before You Invest',desc:'Every stock page has a built-in explainer — P/E ratio, what the company does, risk level. Investing with understanding, not guesswork.',tag:'Education First',tagType:'saffron',ic:'feat-icon-saffron'},
  {icon:'🛡️',title:'Smart Risk Assessment',desc:'Our risk engine analyses your portfolio concentration, sector exposure, and volatility — then gives you a plain-English risk score.',tag:'Portfolio Safety',tagType:'jade',ic:'feat-icon-jade'},
  {icon:'📊',title:'Simplified Charts',desc:'No jargon-heavy candlestick chaos. Clean, annotated charts that tell you what is actually happening with your investment.',tag:'Clarity',tagType:'saffron',ic:'feat-icon-saffron'},
  {icon:'🧮',title:'SIP Calculator',desc:'Plan monthly SIPs with goal-based projections. Enter your target — retirement, car, home — and we map the path.',tag:'Goal Planning',tagType:'jade',ic:'feat-icon-jade'},
  {icon:'🔔',title:'Smart Alerts',desc:'Plain-language alerts. Not "MACD crossover" — but "TCS just dropped 5% in a day, here is why it might matter for you."',tag:'No Jargon',tagType:'saffron',ic:'feat-icon-saffron'},
  {icon:'🇮🇳',title:'NSE & BSE Compliant',desc:'Fully SEBI-registered, compliant with PMLA, Aadhaar-based KYC, and UPI payment rails. Built for India, by design.',tag:'SEBI Registered',tagType:'jade',ic:'feat-icon-jade'},
];

const TESTIMONIALS = [
  {name:'Priya Sharma',role:'IT Professional',city:'Bengaluru',text:'I always thought the stock market was only for finance people. TradeLite\'s learn-as-you-go modules gave me the confidence to make my first investment in 2 weeks.',av:'PS',bg:'#8B3A8A'},
  {name:'Arjun Mehta',role:'College Student',city:'Pune',text:'The risk score feature is incredible. It told me I was too heavily concentrated in tech stocks before I even realised it. Saved me from a bad decision.',av:'AM',bg:'#2C5F8A'},
  {name:'Divya Nair',role:'Homemaker',city:'Chennai',text:'Mera pehla investment tha aur mujhe bilkul bhi darr nahi laga. Interface itna simple hai ki main apne phone par easily track karti hoon.',av:'DN',bg:'#5C8A2C'},
  {name:'Rohan Verma',role:'Small Business Owner',city:'Delhi',text:'The SIP goal planner showed me exactly how much to invest monthly to reach my home down payment target. That clarity is what was missing from other apps.',av:'RV',bg:'#8A5C2C'},
  {name:'Meera Pillai',role:'School Teacher',city:'Kochi',text:'As someone with zero finance background, TradeLite is the first platform that did not make me feel stupid. The glossary tooltips on every screen are brilliant.',av:'MP',bg:'#2C6A8A'},
  {name:'Kartik Bose',role:'Startup Founder',city:'Hyderabad',text:'I used many apps — Zerodha, Groww — but TradeLite is the only one that actively warns me when I am making an emotional trade. The guardrails are next-level.',av:'KB',bg:'#6A2C8A'},
];

function AppMockup() {
  const stocks = [
    {sym:'RELIANCE',name:'Reliance Inds',p:'₹2,948',chg:'+1.42%',up:true,logo:'RI',bg:'#c0392b'},
    {sym:'TCS',name:'Tata Consult.',p:'₹3,812',chg:'+0.78%',up:true,logo:'TC',bg:'#1a6b9e'},
    {sym:'INFY',name:'Infosys Ltd',p:'₹1,456',chg:'-0.34%',up:false,logo:'IF',bg:'#006DB7'},
    {sym:'SBIN',name:'State Bank',p:'₹812',chg:'+2.15%',up:true,logo:'SB',bg:'#1a3b5e'},
  ];
  return (
    <div className="app-mockup">
      <div className="float-card float-card-1">
        <div className="fc-label">Today's Gain</div>
        <div className="fc-val jade">+₹1,842.50</div>
      </div>
      <div className="phone-frame">
        <div className="pf-header">
          <div>
            <div className="pf-greeting">Namaste 🙏</div>
            <div className="pf-name">Arjun Mehta</div>
          </div>
          <div className="pf-notif">🔔</div>
        </div>
        <div className="pf-portfolio-card">
          <div className="pfc-label">Portfolio Value</div>
          <div className="pfc-value">₹84,312</div>
          <div className="pfc-change">▲ +₹1,842.50 today (+2.23%)</div>
          <div className="pfc-bar">
            <div className="pfc-bar-track"><div className="pfc-bar-fill"/></div>
            <div className="pfc-bar-lbl"><span>Goal: ₹1,25,000</span><span>67%</span></div>
          </div>
        </div>
        <div className="pf-section-title">Your Holdings</div>
        <div className="pf-stocks">
          {stocks.map(s => (
            <div className="pf-stock-row" key={s.sym}>
              <div className="pfs-left">
                <div className="pfs-logo" style={{background:s.bg}}>{s.logo}</div>
                <div>
                  <div className="pfs-sym">{s.sym}</div>
                  <div className="pfs-name">{s.name}</div>
                </div>
              </div>
              <div className="pfs-right">
                <div className="pfs-price">{s.p}</div>
                <div className={`pfs-chg ${s.up?'up':'dn'}`}>{s.chg}</div>
              </div>
            </div>
          ))}
        </div>
        <div className="pf-bottom-nav">
          {[['🏠','Home'],['📈','Markets'],['🎓','Learn'],['👤','Profile']].map(([ic,lb],i)=>(
            <div className={`pbn-item ${i===0?'active':''}`} key={lb}>
              <div className="pbn-icon">{ic}</div>
              <div>{lb}</div>
            </div>
          ))}
        </div>
      </div>
      <div className="float-card float-card-2">
        <div className="fc-label">Risk Score</div>
        <div className="fc-val saffron">Moderate ●</div>
      </div>
    </div>
  );
}

function EduSection() {
  const [selected, setSelected] = useState(null);
  const modules = [
    {icon:'📘',bg:'rgba(255,107,0,0.1)',title:'What is a Stock?',sub:'5 min · Beginner',prog:100,done:true},
    {icon:'📊',bg:'rgba(0,135,90,0.1)',title:'Reading a Balance Sheet',sub:'8 min · Beginner',prog:0.6,done:false,pct:'60%'},
    {icon:'🔢',bg:'rgba(245,200,66,0.1)',title:'Understanding P/E Ratio',sub:'6 min · Beginner',prog:0,done:false,pct:'0%'},
    {icon:'🛡️',bg:'rgba(255,107,0,0.1)',title:'Diversification Basics',sub:'7 min · Intermediate',prog:0,done:false,pct:'0%'},
  ];
  return (
    <section id="learn">
      <div className="sec-inner">
        <div className="edu-grid">
          <div>
            <div className="sec-chip sec-chip-jade">📚 Education First</div>
            <h2 className="sec-h2">Learn investing<br/><em style={{fontStyle:'italic',color:'var(--saffron)'}}>as you trade.</em></h2>
            <p className="sec-p" style={{marginBottom:'1.5rem'}}>
              TradeLite embeds bite-sized learning modules directly into the trading experience. Before you buy any stock, you'll understand what you're buying and why.
            </p>
            <p className="sec-p">
              From "What is a dividend?" to understanding quarterly results — our curriculum is built for first-time investors in the Indian market, in Hindi and English.
            </p>
            <div style={{marginTop:'2rem',display:'flex',gap:'1rem',flexWrap:'wrap'}}>
              <div style={{padding:'1rem 1.25rem',background:'var(--card)',border:'1px solid var(--border)',borderRadius:'12px'}}>
                <div style={{fontFamily:'Fraunces,serif',fontWeight:700,fontSize:'1.4rem',color:'var(--saffron)'}}>40+</div>
                <div style={{fontSize:'0.75rem',color:'var(--muted)',marginTop:'0.1rem'}}>Free Modules</div>
              </div>
              <div style={{padding:'1rem 1.25rem',background:'var(--card)',border:'1px solid var(--border)',borderRadius:'12px'}}>
                <div style={{fontFamily:'Fraunces,serif',fontWeight:700,fontSize:'1.4rem',color:'var(--jade-light)'}}>Hindi</div>
                <div style={{fontSize:'0.75rem',color:'var(--muted)',marginTop:'0.1rem'}}>& English</div>
              </div>
              <div style={{padding:'1rem 1.25rem',background:'var(--card)',border:'1px solid var(--border)',borderRadius:'12px'}}>
                <div style={{fontFamily:'Fraunces,serif',fontWeight:700,fontSize:'1.4rem',color:'var(--gold)'}}>Quiz</div>
                <div style={{fontSize:'0.75rem',color:'var(--muted)',marginTop:'0.1rem'}}>Based Learning</div>
              </div>
            </div>
          </div>
          <div className="edu-visual">
            <div className="edu-card">
              <div className="edu-card-header">
                <div className="ech-title">📚 Your Learning Path</div>
                <div className="ech-sub">Personalised for new investors</div>
              </div>
              <div className="edu-modules">
                {modules.map((m,i) => (
                  <div className="edu-module" key={i}>
                    <div className="em-icon" style={{background:m.bg}}>{m.icon}</div>
                    <div className="em-text">
                      <div className="et">{m.title}</div>
                      <div className="es">{m.sub}</div>
                    </div>
                    <div className="em-progress">
                      {m.done
                        ? <div className="em-done">✓</div>
                        : <div className="em-prog-ring" style={{'--p':`${Math.round(m.prog*100)}%`}}>{m.pct}</div>
                      }
                    </div>
                  </div>
                ))}
              </div>
            </div>
            <div className="edu-float">
              <div className="ef-q">What does "Market Cap" mean?</div>
              <div className="ef-opts">
                <div className="ef-opt">The company's profit</div>
                <div className="ef-opt correct">Total shares × price ✓</div>
                <div className="ef-opt">Its bank balance</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  );
}

function RiskSection() {
  const bars = [
    {label:'Equity Stocks',pct:68,color:'var(--saffron)',delay:'0.1s'},
    {label:'Index ETFs',pct:20,color:'var(--jade-light)',delay:'0.2s'},
    {label:'Debt Funds',pct:8,color:'var(--gold)',delay:'0.3s'},
    {label:'Cash',pct:4,color:'var(--muted)',delay:'0.4s'},
  ];
  const breakdown = [
    {name:'Technology',pct:'34%',w:34,color:'#FF6B00'},
    {name:'Banking',pct:'28%',w:28,color:'#00875A'},
    {name:'Energy',pct:'18%',w:18,color:'#F5C842'},
    {name:'Consumer',pct:'12%',w:12,color:'#6B8CFF'},
    {name:'Others',pct:'8%',w:8,color:'#4B5563'},
  ];
  return (
    <section className="risk-section" id="risk">
      <div className="sec-inner">
        <div className="risk-grid">
          <div className="risk-visual">
            <div className="risk-meter-card">
              <div className="rm-header">
                <div className="rm-title">Portfolio Risk Meter</div>
                <div className="rm-profile-badge">Moderate Risk</div>
              </div>
              <div className="rm-gauge">
                <svg viewBox="0 0 280 100" style={{width:'100%',height:'100%'}}>
                  <defs>
                    <linearGradient id="gaugeg" x1="0%" y1="0%" x2="100%" y2="0%">
                      <stop offset="0%" stopColor="#00875A"/>
                      <stop offset="50%" stopColor="#F5C842"/>
                      <stop offset="100%" stopColor="#FF4D6A"/>
                    </linearGradient>
                  </defs>
                  <path d="M20,90 A120,120 0 0,1 260,90" fill="none" stroke="rgba(255,255,255,0.06)" strokeWidth="16" strokeLinecap="round"/>
                  <path d="M20,90 A120,120 0 0,1 260,90" fill="none" stroke="url(#gaugeg)" strokeWidth="16" strokeLinecap="round" strokeDasharray="377" strokeDashoffset="94" style={{transition:'stroke-dashoffset 1s ease'}}/>
                  <circle cx="176" cy="48" r="6" fill="#F5C842"/>
                  <line x1="140" y1="90" x2="176" y2="48" stroke="#F5C842" strokeWidth="2.5" strokeLinecap="round"/>
                  <circle cx="140" cy="90" r="5" fill="var(--card)" stroke="rgba(255,255,255,0.2)" strokeWidth="1.5"/>
                  <text x="20" y="108" fill="var(--muted)" fontSize="10" fontFamily="Plus Jakarta Sans">Low</text>
                  <text x="116" y="22" fill="var(--muted)" fontSize="10" fontFamily="Plus Jakarta Sans" textAnchor="middle">Moderate</text>
                  <text x="252" y="108" fill="var(--muted)" fontSize="10" fontFamily="Plus Jakarta Sans" textAnchor="end">High</text>
                </svg>
              </div>
              <div className="risk-bars">
                {bars.map((b,i) => (
                  <div className="rb-row" key={i}>
                    <div className="rb-label">{b.label}</div>
                    <div className="rb-track">
                      <div className="rb-fill" style={{width:`${b.pct}%`,background:b.color,animationDelay:b.delay}}/>
                    </div>
                    <div className="rb-val" style={{color:b.color}}>{b.pct}%</div>
                  </div>
                ))}
              </div>
            </div>
            <div className="portfolio-breakdown">
              <div className="pb-title">Sector Concentration</div>
              <div className="pb-items">
                {breakdown.map((b,i)=>(
                  <div className="pb-item" key={i}>
                    <div className="pb-dot" style={{background:b.color}}/>
                    <div className="pb-name">{b.name}</div>
                    <div className="pb-bar-wrap"><div className="pb-bar" style={{width:`${b.w/34*100}%`,background:b.color}}/></div>
                    <div className="pb-pct">{b.pct}</div>
                  </div>
                ))}
              </div>
            </div>
          </div>
          <div>
            <div className="sec-chip">🛡️ Risk Intelligence</div>
            <h2 className="sec-h2">Know your risk<br/>before it knows you.</h2>
            <p className="sec-p" style={{marginBottom:'1.5rem'}}>
              TradeLite's risk engine continuously monitors your portfolio. It flags when you're over-concentrated in a single sector, when your volatility exceeds your stated risk appetite, and when you might be making an emotional trade.
            </p>
            <p className="sec-p" style={{marginBottom:'2rem'}}>
              Informed decisions, not impulsive ones. That's the TradeLite promise.
            </p>
            <div style={{display:'flex',flexDirection:'column',gap:'0.75rem'}}>
              {[
                {icon:'⚠️',t:'Concentration Alert',d:'You have 34% in Tech — consider diversifying'},
                {icon:'📉',t:'Volatility Warning',d:'TATAMOTORS up 8% in 2 days — momentum risk'},
                {icon:'✅',t:'Risk Score Updated',d:'Portfolio risk: Moderate. Well balanced.'},
              ].map((a,i)=>(
                <div key={i} style={{display:'flex',alignItems:'flex-start',gap:'0.75rem',padding:'0.85rem 1rem',background:'var(--card)',border:'1px solid var(--border)',borderRadius:'12px'}}>
                  <span style={{fontSize:'1rem',marginTop:'0.1rem'}}>{a.icon}</span>
                  <div>
                    <div style={{fontSize:'0.82rem',fontWeight:600}}>{a.t}</div>
                    <div style={{fontSize:'0.75rem',color:'var(--muted)',marginTop:'0.15rem'}}>{a.d}</div>
                  </div>
                </div>
              ))}
            </div>
          </div>
        </div>
      </div>
    </section>
  );
}

function MarketsSection() {
  const [tab, setTab] = useState('Nifty 50');
  const rows = MARKET_DATA[tab] || [];
  return (
    <section className="markets-section" id="markets">
      <div className="sec-inner">
        <div className="sec-chip">📈 Indian Markets</div>
        <h2 className="sec-h2">NSE &amp; BSE, live.</h2>
        <p className="sec-p">Real-time quotes from the National Stock Exchange and BSE. Track Nifty 50, Mid-Caps, and upcoming IPOs — all in one place.</p>
        <div className="mkt-tabs">
          {Object.keys(MARKET_DATA).map(t=>(
            <button key={t} className={`mkt-tab ${tab===t?'active':''}`} onClick={()=>setTab(t)}>{t}</button>
          ))}
        </div>
        <div style={{overflowX:'auto'}}>
          <table className="mkt-table">
            <thead>
              <tr>
                <th>Symbol</th>
                <th>Company</th>
                <th>Price</th>
                <th>Change</th>
                <th>Mkt Cap</th>
                <th>Volume</th>
                <th>Exchange</th>
              </tr>
            </thead>
            <tbody>
              {rows.map(r=>(
                <tr key={r.sym}>
                  <td>
                    <span className="td-logo" style={{background:r.bg}}>{r.logo}</span>
                    <span className="td-sym">{r.sym}</span>
                  </td>
                  <td><span className="td-name">{r.name}</span></td>
                  <td style={{fontWeight:600}}>{r.p}</td>
                  <td><span className={r.up?'td-up':'td-dn'}>{r.chg}</span></td>
                  <td style={{color:'var(--muted)',fontSize:'0.8rem'}}>{r.mktcap}</td>
                  <td style={{color:'var(--muted)',fontSize:'0.8rem'}}>{r.vol}</td>
                  <td style={{fontSize:'0.75rem'}}><span style={{background:'rgba(0,135,90,0.1)',color:'var(--jade-light)',padding:'0.15rem 0.5rem',borderRadius:'4px',fontWeight:600}}>NSE</span></td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
        <div className="sebi-note">
          <span>🛡️</span>
          <span>All market data is sourced via SEBI-approved data vendors. TradeLite is registered with SEBI as an Investment Adviser (IA) and complies with all NSE/BSE regulatory guidelines.</span>
        </div>
      </div>
    </section>
  );
}

function App() {
  const [email, setEmail] = useState('');

  return (
    <>
      {/* NAV */}
      <nav>
        <div className="logo-wrap">
          <div className="logo-icon">TL</div>
          <span className="logo-text">Trade<span>Lite</span></span>
        </div>
        <ul className="nav-links">
          <li><a href="#features">Features</a></li>
          <li><a href="#learn">Learn <span className="nav-badge">NEW</span></a></li>
          <li><a href="#risk">Risk Tools</a></li>
          <li><a href="#markets">Markets</a></li>
          <li><a href="#pricing">Pricing</a></li>
        </ul>
        <div className="nav-actions">
          <button className="btn-outline">Log In</button>
          <button className="btn-saffron">Start Free →</button>
          <div className="hamburger"><span/><span/><span/></div>
        </div>
      </nav>

      {/* TICKER */}
      <div className="ticker-bar">
        <div className="ticker-scroll">
          {[...NSE_TICKERS,...NSE_TICKERS].map((t,i)=>(
            <span className="tick" key={i}>
              <span className="nse-tag" style={{display:i===0?'inline':'none'}}>NSE LIVE</span>
              <span className="ts">{t.sym}</span>
              <span className="tp">₹{t.p}</span>
              <span className={t.up?'tu':'td'}>{t.chg}</span>
            </span>
          ))}
        </div>
      </div>

      {/* HERO */}
      <section className="hero" id="hero">
        <div className="hero-mesh"/>
        <div className="hero-grid-lines"/>
        <div className="hero-inner">
          <div>
            <div className="hero-eyebrow"><div className="blink-dot"/> Built for India's New Investors</div>
            <h1 className="hero-h1">
              Invest with<br/>
              <em>confidence,</em><br/>
              not <span className="green-word">confusion.</span>
            </h1>
            <p className="hero-p">
              TradeLite is India's education-first trading platform. Trade NSE & BSE stocks with a simplified interface, built-in risk assessment, and bite-sized learning — designed for first-time investors.
            </p>
            <div className="hero-actions">
              <button className="btn-hero-primary">Open Free Account →</button>
              <button className="btn-hero-secondary">Watch Demo ▶</button>
            </div>
            <div className="hero-trust">
              <div className="trust-avatars">
                {['AR','PR','MK','SV','RN'].map((a,i)=>(
                  <div className="trust-av" key={i} style={{background:`hsl(${i*60+20},60%,38%)`}}>{a}</div>
                ))}
              </div>
              <div className="trust-text"><strong>2.4 lakh+ investors</strong> already growing wealth</div>
            </div>
            <div className="hero-stats-row">
              <div className="hstat"><div className="hnum saffron">₹0</div><div className="hlbl">Commission Fees</div></div>
              <div className="hstat"><div className="hnum jade">40+</div><div className="hlbl">Learn Modules</div></div>
              <div className="hstat"><div className="hnum">SEBI</div><div className="hlbl">Registered &amp; Safe</div></div>
            </div>
          </div>
          <AppMockup/>
        </div>
      </section>

      {/* COMPLIANCE BAND */}
      <div className="compliance-band">
        <div className="compliance-inner">
          {[
            {ic:'🛡️',t:'SEBI Registered',s:'Investment Adviser IA-0001234'},
            {ic:'🏦',t:'PMLA Compliant',s:'Anti-Money Laundering verified'},
            {ic:'🪪',t:'Aadhaar e-KYC',s:'Instant verification via DigiLocker'},
            {ic:'💳',t:'UPI Payments',s:'Instant fund transfer via NPCI UPI'},
            {ic:'🔒',t:'256-bit Encryption',s:'Bank-grade data protection'},
          ].map((c,i)=>(
            <React.Fragment key={i}>
              {i>0 && <div className="comp-divider"/>}
              <div className="comp-item">
                <div className="comp-icon">{c.ic}</div>
                <div className="comp-text"><div className="ct">{c.t}</div><div className="cs">{c.s}</div></div>
              </div>
            </React.Fragment>
          ))}
        </div>
      </div>

      {/* FEATURES */}
      <section className="features-section" id="features">
        <div className="sec-inner">
          <div className="sec-chip">✨ Platform Features</div>
          <h2 className="sec-h2">Everything a new investor<br/>actually needs.</h2>
          <p className="sec-p">No overwhelming dashboards. No jargon walls. Just the tools that help you learn, decide, and grow — steadily.</p>
          <div className="features-grid">
            {FEATURES.map((f,i)=>(
              <div className="feat-card" key={i}>
                <div className={`feat-icon-wrap ${f.ic}`}>{f.icon}</div>
                <div className="feat-title">{f.title}</div>
                <div className="feat-desc">{f.desc}</div>
                <span className={`feat-tag ${f.tagType}`}>{f.tag}</span>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* EDUCATION */}
      <EduSection/>

      {/* RISK */}
      <RiskSection/>

      {/* HOW IT WORKS */}
      <section id="how">
        <div className="sec-inner">
          <div className="sec-chip">🚀 Getting Started</div>
          <h2 className="sec-h2">Invested in 4 easy steps.</h2>
          <p className="sec-p">From sign-up to your first stock purchase — it takes under 15 minutes. No paperwork, no branch visits.</p>
          <div className="how-row">
            {[
              {n:'1',emoji:'📱',t:'Download & Sign Up',d:'Install the app or sign up on web. Just your mobile number to begin.'},
              {n:'2',emoji:'🪪',t:'Complete eKYC',d:'Aadhaar-based instant KYC via DigiLocker. Paperless & secure in under 3 minutes.'},
              {n:'3',emoji:'🎓',t:'Take a Starter Quiz',d:'Tell us your goals and risk comfort. We build your personalised learning path and portfolio template.'},
              {n:'4',emoji:'📈',t:'Make Your First Trade',d:'Buy your first share — as little as ₹1 with fractional shares. Real stocks, real learning.'},
            ].map((s,i)=>(
              <div className="how-step" key={i}>
                <div className="how-num-wrap">
                  <div className="how-num">{s.n}</div>
                </div>
                <div className="how-emoji">{s.emoji}</div>
                <div className="how-title">{s.t}</div>
                <div className="how-desc">{s.d}</div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* MARKETS */}
      <MarketsSection/>

      {/* TESTIMONIALS */}
      <section id="testimonials">
        <div className="sec-inner">
          <div className="sec-chip">💬 Real Investors</div>
          <h2 className="sec-h2">From Mumbai to Mysore,<br/>investors trust TradeLite.</h2>
          <div className="test-grid">
            {TESTIMONIALS.map((t,i)=>(
              <div className="test-card" key={i}>
                <div className="test-stars">★★★★★</div>
                <div className="test-q">"{t.text}"</div>
                <div className="test-author">
                  <div className="ta-av" style={{background:t.bg}}>{t.av}</div>
                  <div>
                    <div className="ta-name">{t.name}</div>
                    <div className="ta-role">{t.role}</div>
                    <div className="ta-city">📍 {t.city}</div>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      </section>

      {/* CTA */}
      <section className="cta-section">
        <div className="cta-bg"/>
        <div className="sebi-chip">🛡️ SEBI Registered · Safe · Secure</div>
        <h2 className="cta-h2">Your first investment<br/>starts <em>today.</em></h2>
        <p className="cta-p">Join 2.4 lakh Indians building wealth with confidence. No experience needed — TradeLite teaches you as you trade.</p>
        <div className="cta-form">
          <input className="cta-inp" type="email" placeholder="Enter your email" value={email} onChange={e=>setEmail(e.target.value)}/>
          <button className="btn-saffron" style={{padding:'0.875rem 1.5rem',fontSize:'0.9rem',borderRadius:'10px'}}>Get Started →</button>
        </div>
        <div className="cta-disclaimer">Free forever plan available · No credit card · eKYC in 3 minutes · SEBI IA-0001234</div>
      </section>

      {/* FOOTER */}
      <footer>
        <div className="footer-inner">
          <div className="footer-brand">
            <span className="logo-text">Trade<span>Lite</span></span>
            <p>India's education-first trading platform. Empowering new investors to participate in the equity market with confidence, clarity, and safety.</p>
            <div className="footer-sebi">
              SEBI Registered Investment Adviser · Reg. No. IA-0001234<br/>
              NSE Member · BSE Member · AMFI Registered<br/>
              CIN: U74999MH2024PTC001234
            </div>
          </div>
          <div className="footer-col">
            <h4>Platform</h4>
            <a href="#">Stock Trading</a>
            <a href="#">Mutual Funds</a>
            <a href="#">SIP Planner</a>
            <a href="#">IPO Access</a>
            <a href="#">Portfolio Tracker</a>
          </div>
          <div className="footer-col">
            <h4>Learn</h4>
            <a href="#">Beginner Guide</a>
            <a href="#">Video Lessons</a>
            <a href="#">Market Glossary</a>
            <a href="#">Blog</a>
            <a href="#">Webinars</a>
          </div>
          <div className="footer-col">
            <h4>Company</h4>
            <a href="#">About Us</a>
            <a href="#">Careers</a>
            <a href="#">Press</a>
            <a href="#">Help Centre</a>
            <a href="#">Grievance</a>
          </div>
        </div>
        <div className="footer-bottom">
          <span>© 2026 TradeLite Technologies Pvt. Ltd. All rights reserved.</span>
          <span>Investments are subject to market risks. Please read all scheme-related documents carefully.</span>
        </div>
      </footer>
    </>
  );
}

ReactDOM.createRoot(document.getElementById('root')).render(<App/>);
</script>
</body>
</html>
