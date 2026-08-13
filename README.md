# m728pwjwry-alt.github.io
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Specimen Label Maker</title>
<link href="https://fonts.googleapis.com/css2?family=Archivo+Narrow:wght@400;500;600&family=Bricolage+Grotesque:opsz,wght@12..96,500;12..96,600;12..96,700&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500;600&family=Spectral:ital,wght@0,400;0,600;1,400&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
/* ─────────────────────────────────────────────────────────────
   Light table. The sheet proof is the lit object; the interface
   around it stays dim so the paper is what your eye lands on.
   ───────────────────────────────────────────────────────────── */
:root{
  --void:#080B0F;
  --ground:#0C1015;
  --raise:#141A22;
  --raise-2:#1B2531;
  --edge:rgba(255,255,255,.075);
  --edge-lit:rgba(255,255,255,.14);
  --ink:#E9EEF4;
  --dim:#7E8C9C;
  --faint:#55616F;
  --amazonite:#79D4C6;
  --amazonite-dim:rgba(121,212,198,.16);
  --amber:#E3B341;
  --r-lg:16px; --r-md:11px; --r-sm:8px;
  --shadow:0 1px 2px rgba(0,0,0,.4), 0 8px 28px -12px rgba(0,0,0,.7);
}
*{box-sizing:border-box;}
html,body{margin:0;padding:0;}
body{
  background:
    radial-gradient(1100px 620px at 68% -8%, rgba(121,212,198,.055), transparent 62%),
    var(--ground);
  color:var(--ink);
  font-family:Inter,system-ui,-apple-system,sans-serif;
  font-size:14px;line-height:1.55;letter-spacing:-.005em;
  -webkit-font-smoothing:antialiased;
}
h1,h2,h3{font-family:'Bricolage Grotesque',Inter,sans-serif;font-weight:600;letter-spacing:-.02em;}

/* ── top bar ── */
.topbar{
  display:flex;align-items:center;gap:14px;flex-wrap:wrap;
  padding:14px 22px;position:sticky;top:0;z-index:30;
  background:rgba(12,16,21,.78);backdrop-filter:blur(18px) saturate(140%);
  border-bottom:1px solid var(--edge);
}
.topbar h1{font-size:17px;margin:0;}
.topbar .spec{
  font-family:'JetBrains Mono',ui-monospace,monospace;font-size:11px;color:var(--dim);
  background:var(--raise);border:1px solid var(--edge);border-radius:999px;padding:4px 11px;
}
.topbar .grow{flex:1;}

button,.act{font:inherit;}
button.act,a.act{
  font-size:13px;font-weight:550;letter-spacing:-.01em;
  border:1px solid transparent;border-radius:var(--r-sm);
  padding:9px 16px;cursor:pointer;text-decoration:none;display:inline-flex;align-items:center;gap:7px;
  transition:background .16s ease,border-color .16s ease,transform .1s ease;
}
button.act{background:var(--amazonite);color:#07110F;}
button.act:hover{background:#8FE0D3;}
a.act{background:var(--amber);color:#140F02;}
a.act:hover{background:#EFC45C;}
button.ghost{background:var(--raise);color:var(--dim);border-color:var(--edge);}
button.ghost:hover{background:var(--raise-2);color:var(--ink);border-color:var(--edge-lit);}
button:active,a.act:active{transform:translateY(1px);}
button:focus-visible,a:focus-visible,select:focus-visible,input:focus-visible,textarea:focus-visible{
  outline:2px solid var(--amazonite);outline-offset:2px;
}

/* ── layout ── */
.wrap{display:grid;grid-template-columns:392px 1fr;align-items:start;}
@media (max-width:1040px){.wrap{grid-template-columns:1fr;}}
.panel{padding:22px 22px 80px;border-right:1px solid var(--edge);}
@media (max-width:1040px){.panel{border-right:none;border-bottom:1px solid var(--edge);}}
.stage{padding:26px 24px 40px;}

/* ── form ── */
.field{margin-bottom:26px;}
.legend{
  display:flex;align-items:center;gap:8px;
  font-family:'JetBrains Mono',ui-monospace,monospace;font-size:10px;letter-spacing:.14em;
  text-transform:uppercase;color:var(--faint);margin-bottom:10px;
}
.legend::before{content:"";width:14px;height:1px;background:var(--amazonite);opacity:.6;}
.hint{font-size:12.5px;color:var(--dim);margin:9px 0 0;line-height:1.5;}
.hint b{color:var(--ink);font-weight:550;}

textarea,select,input[type=number]{
  width:100%;background:var(--raise);color:var(--ink);
  border:1px solid var(--edge);border-radius:var(--r-sm);
  padding:11px 13px;font-family:inherit;font-size:13.5px;
  transition:border-color .16s ease,background .16s ease;
}
input[type=number],textarea{font-family:'JetBrains Mono',ui-monospace,monospace;font-size:12.5px;}
textarea{min-height:154px;resize:vertical;white-space:pre;overflow-x:auto;line-height:1.75;}
textarea:focus,select:focus,input:focus{border-color:var(--amazonite);background:var(--raise-2);outline:none;}
select{
  appearance:none;cursor:pointer;
  background-image:linear-gradient(45deg,transparent 50%,var(--dim) 50%),linear-gradient(135deg,var(--dim) 50%,transparent 50%);
  background-position:calc(100% - 17px) 50%,calc(100% - 12px) 50%;
  background-size:5px 5px,5px 5px;background-repeat:no-repeat;padding-right:34px;
}
.check{display:flex;gap:10px;align-items:flex-start;margin-top:12px;font-size:13px;cursor:pointer;}
.check input{margin-top:3px;accent-color:var(--amazonite);width:15px;height:15px;}
.nums{display:grid;grid-template-columns:1fr 1fr;gap:11px;}
.nums label{
  font-family:'JetBrains Mono',ui-monospace,monospace;font-size:9.5px;color:var(--faint);
  display:block;margin-bottom:5px;text-transform:uppercase;letter-spacing:.1em;
}

/* ── position picker ── */
.poswrap{display:grid;gap:9px;}
.pos{
  display:flex;gap:13px;align-items:center;position:relative;
  background:var(--raise);border:1px solid var(--edge);border-radius:var(--r-md);
  padding:12px 13px;cursor:pointer;
  transition:border-color .16s ease,background .16s ease,transform .12s ease;
}
.pos:hover{border-color:var(--edge-lit);transform:translateY(-1px);}
.pos.on{
  border-color:var(--amazonite);background:linear-gradient(180deg,var(--amazonite-dim),transparent 70%),var(--raise-2);
}
.pos.on::after{
  content:"";position:absolute;right:12px;top:12px;width:7px;height:7px;border-radius:50%;
  background:var(--amazonite);box-shadow:0 0 0 3px rgba(121,212,198,.18);
}
.pos svg{flex:0 0 74px;}
.pos .t{font-size:13.5px;font-weight:550;letter-spacing:-.01em;}
.pos .d{font-size:11.5px;color:var(--dim);line-height:1.45;margin-top:3px;}

/* ── cards ── */
.card{
  background:linear-gradient(180deg,rgba(255,255,255,.028),transparent 42%),var(--raise);
  border:1px solid var(--edge);border-radius:var(--r-lg);box-shadow:var(--shadow);
  padding:20px;display:inline-block;margin:0 22px 24px 0;vertical-align:top;
}
.cap{
  font-family:'JetBrains Mono',ui-monospace,monospace;font-size:9.5px;letter-spacing:.14em;
  text-transform:uppercase;color:var(--faint);margin-bottom:14px;
}

/* signature: the proof sits on a lit table */
.lightbox{
  position:relative;padding:26px;border-radius:var(--r-lg);
  background:radial-gradient(120% 90% at 50% 0%, rgba(121,212,198,.10), transparent 58%), var(--void);
  border:1px solid var(--edge);
}
.lightbox .sheetscale,.lightbox .zoomwrap{
  box-shadow:0 0 0 1px rgba(255,255,255,.10), 0 18px 60px -20px rgba(121,212,198,.32), 0 24px 60px -30px #000;
  border-radius:3px;
}
.zoomwrap{overflow:hidden;background:#fff;}

.planlist{
  font-family:'JetBrains Mono',ui-monospace,monospace;font-size:12px;color:var(--dim);
  background:var(--raise);border:1px solid var(--edge);border-radius:var(--r-lg);
  padding:18px 20px;line-height:2;max-width:740px;box-shadow:var(--shadow);
}
.planlist b{color:var(--ink);font-weight:500;}
.planlist .n{
  color:var(--void);background:var(--amber);border-radius:4px;
  padding:1px 6px;margin-right:5px;font-weight:600;font-size:11px;
}

/* ── help overlay ── */
.sheetHelp{position:fixed;inset:0;z-index:1000;background:var(--ground);display:none;flex-direction:column;}
.sheetHelp.open{display:flex;animation:rise .22s ease;}
@keyframes rise{from{opacity:0;transform:translateY(10px);}to{opacity:1;transform:none;}}
.sheetHelp .hbar{
  display:flex;align-items:center;gap:14px;padding:15px 22px;
  border-bottom:1px solid var(--edge);background:rgba(12,16,21,.8);backdrop-filter:blur(18px);
}
.sheetHelp .hbar h2{font-size:17px;margin:0;}
.sheetHelp .body{flex:1;overflow-y:auto;-webkit-overflow-scrolling:touch;padding:24px 22px 70px;max-width:680px;margin:0 auto;width:100%;}
.sheetHelp h3{
  font-family:'JetBrains Mono',ui-monospace,monospace;font-size:10px;letter-spacing:.14em;
  text-transform:uppercase;color:var(--amazonite);margin:30px 0 11px;font-weight:500;
  display:flex;align-items:center;gap:9px;
}
.sheetHelp h3::after{content:"";flex:1;height:1px;background:var(--edge);}
.sheetHelp h3:first-child{margin-top:0;}
.sheetHelp p{font-size:13.5px;line-height:1.65;margin:0 0 12px;color:var(--ink);}
.sheetHelp .r{display:flex;gap:13px;padding:11px 0;border-bottom:1px solid var(--edge);font-size:13px;line-height:1.55;align-items:flex-start;}
.sheetHelp .r:last-child{border-bottom:none;}
.sheetHelp .k{
  flex:0 0 100px;font-family:'JetBrains Mono',ui-monospace,monospace;font-size:10.5px;color:var(--ink);
  background:var(--raise);border:1px solid var(--edge);border-radius:6px;padding:5px 7px;text-align:center;
}
.sheetHelp .warn{
  background:linear-gradient(180deg,rgba(227,179,65,.10),transparent),var(--raise);
  border:1px solid rgba(227,179,65,.28);border-left:2px solid var(--amber);
  border-radius:var(--r-md);padding:13px 15px;font-size:12.5px;line-height:1.6;margin:12px 0 0;color:#EFE3C4;
}
.sheetHelp code{font-family:'JetBrains Mono',ui-monospace,monospace;font-size:12px;background:var(--raise);border:1px solid var(--edge);padding:2px 6px;border-radius:5px;}
.sheetHelp .diag{display:flex;gap:15px;align-items:center;padding:12px 0;border-bottom:1px solid var(--edge);}
.sheetHelp .diag:last-child{border-bottom:none;}

/* ── credit ── */
.credit{border-top:1px solid var(--edge);margin-top:38px;padding:26px 0 20px;max-width:740px;}
.credit .by{font-family:'Bricolage Grotesque',Inter,sans-serif;font-size:16px;font-weight:600;color:var(--ink);margin:0 0 4px;letter-spacing:-.02em;}
.credit .biz{font-size:12.5px;color:var(--dim);margin:0 0 16px;line-height:1.55;}
.socials{display:flex;gap:10px;flex-wrap:wrap;}
.socials a{
  display:inline-flex;align-items:center;gap:9px;text-decoration:none;
  background:var(--raise);border:1px solid var(--edge);border-radius:999px;
  padding:9px 15px;color:var(--ink);font-size:13px;font-weight:500;
  transition:border-color .16s ease,background .16s ease,transform .12s ease;
}
.socials a:hover{border-color:var(--edge-lit);background:var(--raise-2);transform:translateY(-1px);}
.socials .h{font-family:'JetBrains Mono',ui-monospace,monospace;font-size:10.5px;color:var(--faint);}
@media (max-width:520px){.socials a{flex:1 1 100%;justify-content:center;}}

/* ── the printable sheet (unchanged geometry) ── */
.sheet{width:215.9mm;height:279.4mm;background:#fff;position:relative;overflow:hidden;}
.cell{position:absolute;overflow:hidden;}
.tick{position:absolute;background:#000;}
.lab{position:absolute;left:0;top:0;color:#000;display:flex;flex-direction:column;
  align-items:center;justify-content:center;text-align:center;padding:1.3mm 1.4mm;}
.lab.dbl{border:.25mm solid #14343f;box-shadow:inset 0 0 0 .5mm #fff, inset 0 0 0 .62mm #14343f;}
.lab.sgl{border:.25mm solid #14343f;}
.lab .sp{font-family:'Spectral',Georgia,serif;font-weight:600;line-height:1.06;}
.lab .loc{font-family:'Archivo Narrow','Arial Narrow',sans-serif;font-weight:500;line-height:1.2;margin-top:.7mm;}
.lab .note{font-family:'Archivo Narrow','Arial Narrow',sans-serif;line-height:1.15;margin-top:.9mm;color:#2a2a2a;}
.lab .rule{height:.18mm;background:#14343f;margin:.55mm 0 .1mm;opacity:.75;}
.foldline{position:absolute;left:0;right:0;border-top:.2mm dashed #9aa4ad;}
.hidezone{position:absolute;left:0;right:0;bottom:0;
  background:repeating-linear-gradient(45deg,rgba(121,212,198,.18) 0 2px,transparent 2px 5px);
  border-top:.2mm dashed rgba(121,212,198,.6);pointer-events:none;}

@page{size:letter;margin:0;}
@media print{
  body{background:#fff;}
  .topbar,.panel,.card .cap,.planlist,.zoomcard,.sheetHelp,.credit{display:none !important;}
  .stage{padding:0;}
  .card{border:none;padding:0;margin:0;background:#fff;display:block;box-shadow:none;}
  .lightbox{padding:0;background:#fff;border:none;}
  .lightbox .sheetscale{box-shadow:none;}
  .sheetscale{width:auto !important;height:auto !important;overflow:visible !important;}
  .sheetscale .sheet{transform:none !important;}
  .hidezone{display:none !important;}
}
/* ── phones ───────────────────────────────────────────────
   Desktop keeps the two-column workbench. Below 760 the page
   becomes a single column, cards go full width, and anything
   that cannot shrink scrolls inside its own box rather than
   dragging the whole page sideways. */
@media (max-width:760px){
  /* Mobile-only layout: desktop geometry and print/PDF sizing are untouched. */
  html,body{
    width:100%;
    max-width:100%;
    overflow-x:hidden;
    overscroll-behavior-x:none;
  }
  body{min-width:0;}
  .wrap,.panel,.stage,.card,.lightbox,.credit,.socials{min-width:0;max-width:100%;}
  .stage{overflow:hidden;}
  .topbar{padding:12px 14px;gap:9px;}
  .topbar h1{font-size:16px;width:100%;}
  .topbar .spec{order:5;font-size:10px;}
  .topbar .grow{display:none;}
  button.act,a.act{flex:1 1 auto;justify-content:center;padding:11px 14px;}
  button.ghost#helpBtn{flex:0 0 46px;}

  .panel{padding:18px 14px 30px;}
  .stage{padding:18px 14px 40px;}
  .nums{grid-template-columns:1fr 1fr;gap:9px;}

  .card{
    display:block;width:100%;margin:0 0 18px 0;padding:14px;
    border-radius:var(--r-md);
  }
  .lightbox{
    padding:14px;
    width:100%;
    max-width:100%;
    overflow:hidden;
  }
  .lightbox .sheetscale,
  .lightbox .zoomwrap{
    display:block;
    max-width:100%;
    min-width:0;
  }
  .zoomwrap{overflow:hidden;}

  .planlist{
    padding:14px;font-size:11.5px;line-height:1.95;border-radius:var(--r-md);
    overflow-x:auto;-webkit-overflow-scrolling:touch;
  }

  .pos{padding:11px 12px;gap:11px;}
  .pos svg{flex:0 0 58px;width:58px;height:32px;}
  .pos .d{font-size:11px;}

  .credit{margin-top:26px;padding:22px 0 10px;}
  .socials{flex-direction:column;gap:8px;}
  .socials a{
    width:100%;
    max-width:100%;
    min-width:0;
    justify-content:flex-start;
    border-radius:var(--r-md);
    padding:12px 14px;
    overflow:hidden;
  }
  .socials a > svg{flex:0 0 16px;}
  .socials .h{margin-left:auto;font-size:10px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;max-width:46%;}

  .sheetHelp .body{padding:18px 14px 60px;}
  .sheetHelp .k{flex:0 0 78px;font-size:9.5px;}
  .sheetHelp .r{gap:10px;}
  .sheetHelp .diag{gap:11px;}
  .sheetHelp .diag svg{flex:0 0 58px;}
}

@media (max-width:400px){
  .socials .h{display:none;}
  .nums{grid-template-columns:1fr;}
}

@media (prefers-reduced-motion:reduce){*{transition:none !important;animation:none !important;}}
</style>
</head>
<body>

<div class="topbar">
  <h1>Specimen Label Maker</h1>
  <span class="spec" id="spec">—</span>
  <span class="grow"></span>
  <button class="act" id="pdfbtn">Build PDF</button>
  <a class="act" id="pdflink" hidden rel="noopener">Open PDF</a>
  <button class="act ghost" onclick="window.print()">Print (desktop)</button>
  <button class="act ghost" id="helpBtn" style="font-weight:700;padding:9px 15px">?</button>
</div>

<div class="sheetHelp" id="help">
  <div class="hbar">
    <h2>How this works</h2>
    <span class="grow" style="flex:1"></span>
    <button class="act ghost" id="helpClose">Close</button>
  </div>
  <div class="body">

    <p>This prints specimen labels — species, locality, provenance — sized to fit
    a specific mineral box, laid out many to a sheet with cutting guides. You pick
    the box, type your specimens, and it works out the label size, how many fit on
    a page, and exactly where to cut.</p>

    <h3>The short version</h3>
    <p>Pick your box. Pick where the label sits. Type one specimen per line.
    Tap <b>Build PDF</b>, then <b>Open PDF</b>, and print at 100% scale on cardstock.
    Cut along the tick marks using the plan at the bottom of the page.</p>

    <h3>1 · Choose the box</h3>
    <p>The dropdown covers perky boxes, gem boxes, micro mounts and the fold-up
    paperboard range. Selecting one fills in its measurements, and <b>you can edit
    any of them</b> — suppliers vary, and some do not publish a height at all.
    Where a height is my estimate the hint says so. Measure yours and correct it.</p>
    <p>Nothing on your list? Choose <b>Custom</b> at the bottom and enter the three numbers.</p>

    <h3>2 · Choose where the label sits</h3>
      <div class="diag"><svg viewBox="0 0 74 40" width="74" height="40">
      <path d="M8 12 L8 32 L66 32 L66 12" fill="none" stroke="#8A97A4" stroke-width="2"/>
      <rect x="12" y="26" width="50" height="5" fill="#C9A227"/>
      <ellipse cx="37" cy="22" rx="13" ry="7" fill="#3C4855"/>
    </svg><div><b>Flat in the bottom</b><br><span style="color:var(--ink-dim);font-size:12.5px">Label lies on the box floor and the specimen sits on it. Sized to the full footprint.</span></div></div>
      <div class="diag"><svg viewBox="0 0 74 40" width="74" height="40">
      <path d="M8 12 L8 32 L66 32 L66 12" fill="none" stroke="#8A97A4" stroke-width="2"/>
      <rect x="12" y="4" width="46" height="24" fill="#C9A227"/>
      <rect x="12" y="28" width="26" height="4" fill="#8A6D1A"/>
      <ellipse cx="44" cy="27" rx="11" ry="5" fill="#3C4855"/>
      <line x1="12" y1="28" x2="58" y2="28" stroke="#0E1216" stroke-width="1" stroke-dasharray="2,2"/>
    </svg><div><b>Standing at the back</b><br><span style="color:var(--ink-dim);font-size:12.5px">One fold. The tab lies flat under the specimen and holds the label down; the panel rises past the rim so it still reads when a specimen is cradled in padding.</span></div></div>
      <div class="diag"><svg viewBox="0 0 74 40" width="74" height="40">
      <path d="M8 12 L8 32 L66 32 L66 12" fill="none" stroke="#8A97A4" stroke-width="2"/>
      <rect x="12" y="13" width="50" height="17" fill="#C9A227"/>
      <ellipse cx="40" cy="28" rx="12" ry="5" fill="#3C4855"/>
    </svg><div><b>Flush to the back wall</b><br><span style="color:var(--ink-dim);font-size:12.5px">Height capped at box height so nothing rises above the rim. Good when boxes stack or go into a flat.</span></div></div>
    <p>Standing labels add two controls. <b>Fold tab</b> is how much lies flat on the
    floor of the box — the specimen pins it down. <b>Above rim</b> is how far the panel
    rises past the edge of the box. Raise it if you pad boxes with plastic and the
    specimen sits high, so the label still reads over the top.</p>

    <h3>3 · Type your specimens</h3>
    <p>One label per line, fields separated by a vertical bar:</p>
    <p><code>Wavellite | Mauldin Mountain | Montgomery Co., AR, USA | ex. J. Baird</code></p>
    <p>Leave a field empty to drop that line. Long species names shrink and wrap on
    their own, and type scales with the label — a five inch label carries much larger
    text than a one inch one.</p>

    <h3>4 · Layout options</h3>
    <div class="r"><span class="k">Fill order</span><span>One entry per column means each strip you cut is a clean stack of one species — no sorting afterwards. Cycling spreads them across the sheet instead.</span></div>
    <div class="r"><span class="k">Clearance</span><span>How much smaller than the box the label is cut, so it drops in without buckling. 1.5 mm suits most boxes.</span></div>
    <div class="r"><span class="k">Sheet edge</span><span>How much of the page to keep clear. Most laser printers cannot print within about 4 mm of the paper edge — raise this if the outer cut marks come out faint. It may cost a row.</span></div>
    <div class="r"><span class="k">Guides</span><span>Ticks put marks in the margins so no printed line ever crosses a label. Full lines are easier to sight along if your cutter is awkward, but leave a faint trace at the cut.</span></div>
    <div class="r"><span class="k">Border</span><span>Double rule, single hairline, or none. The border is inset slightly so a cut that wanders by a fraction of a millimetre does not shave it.</span></div>

    <h3>5 · Print</h3>
    <p>Tap <b>Build PDF</b>, then the <b>Open PDF</b> button that appears. Share it to
    your printer and set <b>scale to 100%</b> — not "fit to page", which silently shrinks
    everything and throws off every measurement.</p>
    <div class="warn"><b>Check the calibration bar on your first proof.</b> A 100 mm line
    prints in the side margin. Measure it with a ruler. If it reads 100 mm the sheet
    printed at true size and the cut plan is good. If it reads short, your printer
    rescaled the page — fix that before you spend cardstock.</div>

    <h3>6 · Cut</h3>
    <p>The plan under the sheet preview gives exact blade positions measured from the
    left and top edges of the paper. Trim the waste strips first, then step across at
    the label width. Stack the strips, trim top and bottom, then cut across.</p>
    <p>Standing labels get an extra step telling you where to score and fold — the
    dashed line printed on the sheet.</p>

    <h3>Paper</h3>
    <p>Cardstock around 65 to 110 lb index works well. Anything past roughly 200 gsm
    needs the manual feed slot on most desktop lasers, one sheet at a time, and the
    rear cover open so the sheet travels straight through instead of curling. Use
    laser-rated stock — inkjet coatings can melt onto a fuser.</p>

    <h3>Who made this</h3>
    <p>Justin Baird, Hot Springs, Arkansas. I dig and sell mineral specimens, and I
    built this because I needed it. Links to my Facebook, Instagram and TikTok are at
    the bottom of the main page. It is free — pass it on.</p>

    <h3>Nothing is stored</h3>
    <p>Everything happens in your browser. No account, no upload, no server holding
    your specimen data. Closing the page clears it, so keep your specimen list
    somewhere of your own if you want it again.</p>

  </div>
</div>

<div class="wrap">
  <div class="panel">

    <div class="field">
      <span class="legend">Box</span>
      <select id="box"></select>
      <div class="nums" style="margin-top:10px">
        <div><label for="bw">Width mm</label><input type="number" id="bw" step="0.1" min="8"></div>
        <div><label for="bl">Depth front-back mm</label><input type="number" id="bl" step="0.1" min="8"></div>
        <div><label for="bd">Height mm</label><input type="number" id="bd" step="0.1" min="4"></div>
      </div>
      <p class="hint" id="boxhint"></p>
    </div>

    <div class="field">
      <span class="legend">Where the label sits</span>
      <div class="poswrap" id="poswrap"></div>
    </div>

    <div class="field" id="dims">
      <span class="legend">Fit</span>
      <div class="nums">
        <div><label for="clear">Clearance mm</label><input type="number" id="clear" value="1.5" step="0.5" min="0" max="8"></div>
        <div id="tabwrap"><label for="tab">Fold tab mm</label><input type="number" id="tab" value="12" step="1" min="4" max="60"></div>
        <div id="rimwrap"><label for="rim">Above rim mm</label><input type="number" id="rim" value="15" step="1" min="0" max="60"></div>
        <div><label for="edge">Sheet edge mm</label><input type="number" id="edge" value="4.6" step="0.5" min="3" max="30"></div>
      </div>
      <p class="hint" id="dimhint"></p>
      <p class="hint"><b>Sheet edge</b> is how much of the page to keep clear. Most
      laser printers cannot print within about 4 mm of the paper edge, so raise this
      if the outermost cut ticks come out faint or missing. Raising it pulls the grid
      in and may cost you a row or a column.</p>
    </div>

    <div class="field">
      <span class="legend">Specimen data</span>
      <textarea id="data" spellcheck="false"></textarea>
      <p class="hint">One label per line, fields split by <b>|</b><br>
      <span style="color:var(--celestite)">Species | Locality | Region | Footer</span></p>
    </div>

    <div class="field">
      <span class="legend">Layout</span>
      <select id="fill">
        <option value="col">One entry per column — each strip is a sorted stack</option>
        <option value="cycle">Cycle entries across the sheet</option>
      </select>
      <label class="check"><input type="checkbox" id="repeat" checked><span>Repeat entries to fill the sheet</span></label>
      <select id="border" style="margin-top:10px">
        <option value="dbl">Double rule border</option>
        <option value="sgl">Single hairline border</option>
        <option value="none">No border</option>
      </select>
      <label class="check"><input type="checkbox" id="divider" checked><span>Hairline under species name</span></label>
      <select id="guides" style="margin-top:10px">
        <option value="ticks">Edge ticks — labels stay clean</option>
        <option value="lines">Full guide lines</option>
        <option value="none">No cutting guides</option>
      </select>
    </div>

    <div class="field">
      <span class="legend">Printing</span>
      <p class="hint">
        Tap <b>Build PDF</b> then <b>Open PDF</b>, share to your printer, and set
        <b>scale to 100%</b>. A 100 mm bar prints in the margin — measure it on the
        first proof to confirm nothing was rescaled.
      </p>
    </div>
  </div>

  <div class="stage">
    <div class="card zoomcard">
      <div class="cap">First label, actual proportions</div>
      <div class="lightbox"><div class="zoomwrap" id="zoomwrap"><div id="zoom"></div></div></div>
      <div class="cap" style="margin:12px 0 0" id="zoomcap"></div>
    </div>

    <div class="card">
      <div class="cap">Sheet proof — letter, 100% scale</div>
      <div class="lightbox"><div class="sheetscale" id="sheetscale"><div class="sheet" id="sheet"></div></div></div>
    </div>

    <div class="planlist" id="plan"></div>

<div class="credit">
  <p class="by">Made by Justin Baird</p>
  <p class="biz">Rockhound and mineral dealer, Hot Springs, Arkansas. Free to use,
  and free to pass along to anyone who prints their own labels.</p>
  <div class="socials">
    <a href="https://www.facebook.com/rock.hound.357" target="_blank" rel="noopener">
      <svg viewBox="0 0 24 24" width="16" height="16" fill="#1877F2" aria-hidden="true"><path d="M24 12.07C24 5.4 18.63 0 12 0S0 5.4 0 12.07C0 18.1 4.39 23.1 10.13 24v-8.44H7.08v-3.49h3.05V9.41c0-3.02 1.79-4.69 4.53-4.69 1.31 0 2.68.24 2.68.24v2.96h-1.51c-1.49 0-1.96.93-1.96 1.89v2.26h3.33l-.53 3.49h-2.8V24C19.61 23.1 24 18.1 24 12.07z"/></svg>
      Facebook <span class="h">rock.hound.357</span></a>
    <a href="https://www.instagram.com/justin_baird_ar_rockhound" target="_blank" rel="noopener">
      <svg viewBox="0 0 24 24" width="16" height="16" fill="#E4405F" aria-hidden="true"><path d="M12 2.16c3.2 0 3.58.01 4.85.07 1.17.05 1.8.25 2.23.41.56.22.96.48 1.38.9.42.42.68.82.9 1.38.16.42.36 1.06.41 2.23.06 1.27.07 1.65.07 4.85s-.01 3.58-.07 4.85c-.05 1.17-.25 1.8-.41 2.23-.22.56-.48.96-.9 1.38-.42.42-.82.68-1.38.9-.42.16-1.06.36-2.23.41-1.27.06-1.65.07-4.85.07s-3.58-.01-4.85-.07c-1.17-.05-1.8-.25-2.23-.41-.56-.22-.96-.48-1.38-.9-.42-.42-.68-.82-.9-1.38-.16-.42-.36-1.06-.41-2.23C2.17 15.58 2.16 15.2 2.16 12s.01-3.58.07-4.85c.05-1.17.25-1.8.41-2.23.22-.56.48-.96.9-1.38.42-.42.82-.68 1.38-.9.42-.16 1.06-.36 2.23-.41C8.42 2.17 8.8 2.16 12 2.16M12 0C8.74 0 8.33.01 7.05.07 5.78.13 4.9.33 4.14.63c-.79.3-1.46.72-2.13 1.38C1.35 2.68.93 3.35.63 4.14.33 4.9.13 5.78.07 7.05.01 8.33 0 8.74 0 12s.01 3.67.07 4.95c.06 1.27.26 2.15.56 2.91.3.79.72 1.46 1.38 2.13.67.66 1.34 1.08 2.13 1.38.76.3 1.64.5 2.91.56C8.33 23.99 8.74 24 12 24s3.67-.01 4.95-.07c1.27-.06 2.15-.26 2.91-.56.79-.3 1.46-.72 2.13-1.38.66-.67 1.08-1.34 1.38-2.13.3-.76.5-1.64.56-2.91.06-1.28.07-1.69.07-4.95s-.01-3.67-.07-4.95c-.06-1.27-.26-2.15-.56-2.91-.3-.79-.72-1.46-1.38-2.13C21.32 1.35 20.65.93 19.86.63 19.1.33 18.22.13 16.95.07 15.67.01 15.26 0 12 0z"/><path d="M12 5.84A6.16 6.16 0 1 0 18.16 12 6.16 6.16 0 0 0 12 5.84zm0 10.16A4 4 0 1 1 16 12a4 4 0 0 1-4 4z"/><circle cx="18.41" cy="5.59" r="1.44"/></svg>
      Instagram <span class="h">justin_baird_ar_rockhound</span></a>
    <a href="https://www.tiktok.com/@arkansasrockhound" target="_blank" rel="noopener">
      <svg viewBox="0 0 24 24" width="16" height="16" fill="#DCE3EA" aria-hidden="true"><path d="M16.6 5.82A4.28 4.28 0 0 1 15.54 3h-3.09v12.4a2.59 2.59 0 0 1-2.59 2.5 2.59 2.59 0 1 1 .76-5.07v-3.1a5.66 5.66 0 0 0-.76-.05 5.68 5.68 0 1 0 5.68 5.68V9.01a7.35 7.35 0 0 0 4.29 1.38V7.3a4.29 4.29 0 0 1-3.23-1.48z"/></svg>
      TikTok <span class="h">@arkansasrockhound</span></a>
  </div>
</div>


  </div>
</div>

<script>
const SHEET_W = 215.9, SHEET_H = 279.4, MARGIN = 4.6, IN = 25.4;
const PT = 0.352778;

/* Box catalogue. All dimensions mm: w/l are the footprint, d is depth.
   est:true marks a depth the supplier does not publish — measure and correct it,
   which is why every dimension is editable below. */
const BOXES = [
  { id:"perky",   name:"Perky box 1¼ in (standard size)", w:31.8, l:31.8, d:31.8, perky:true },
  { id:"p28",     name:"Perky box 28 × 28 × 20 mm",       w:28,   l:28,   d:20 },
  { id:"krantz",  name:"Krantz perky 27 × 27 × 26 mm",    w:27,   l:27,   d:26 },
  { id:"p42",     name:"Perky box 42 × 42 × 33 mm",       w:42,   l:42,   d:33 },
  { id:"p5642",   name:"Perky box 56 × 42 × 33 mm",       w:56,   l:42,   d:33 },
  { id:"p2in",    name:"Perky box 2 in",                  w:50.8, l:50.8, d:38.1, est:1 },
  { id:"p267",    name:"Perky box 2.67 in",               w:67.8, l:67.8, d:50.8, est:1 },
  { id:"gem30",   name:"Premium gem box 30 mm",           w:30,   l:30,   d:12,   est:1, gem:1 },
  { id:"gem40",   name:"Premium gem box 40 mm",           w:40,   l:40,   d:12,   est:1, gem:1 },
  { id:"micro",   name:"Black micro mount box",           w:25,   l:25,   d:20,   est:1 },
  { id:"FB-6",    name:"FB-6 · 5⅛ × 5⅛ × 1½",             w:130.2, l:130.2, d:38.1 },
  { id:"FB-8",    name:"FB-8 · 5⅛ × 3¼ × 1⅜",             w:130.2, l:82.6,  d:34.9 },
  { id:"FB-12",   name:"FB-12 · 3¾ × 3¼ × 1⅜",            w:95.3,  l:82.6,  d:34.9 },
  { id:"FB-18",   name:"FB-18 · 3¼ × 2½ × 1⅛",            w:82.6,  l:63.5,  d:28.6 },
  { id:"FB-20L",  name:"FB-20L · 5⅛ × 1½ × 1",            w:130.2, l:38.1,  d:25.4 },
  { id:"FB-24",   name:"FB-24 · 2½ × 2⅜ × 1",             w:63.5,  l:60.3,  d:25.4 },
  { id:"FB-25",   name:"FB-25 · 3 × 2 × 1",               w:76.2,  l:50.8,  d:25.4 },
  { id:"FB-35",   name:"FB-35 · 2 × 2 × 1",               w:50.8,  l:50.8,  d:25.4 },
  { id:"FB-42L",  name:"FB-42L · 3¾ × 1 × ¾",             w:95.3,  l:25.4,  d:19.1 },
  { id:"FB-54",   name:"FB-54 · 1½ × 1½ × ¾",             w:38.1,  l:38.1,  d:19.1 },
  { id:"custom",  name:"Custom — enter your own",         w:50,   l:50,   d:25 },
];

const POSITIONS = [
  { id:"flat",  name:"Flat in the bottom",
    desc:"Specimen sits on the label. Uses the whole box footprint.",
    svg:`<svg viewBox="0 0 74 40" width="74" height="40">
      <path d="M8 12 L8 32 L66 32 L66 12" fill="none" stroke="#8A97A4" stroke-width="2"/>
      <rect x="12" y="26" width="50" height="5" fill="#C9A227"/>
      <ellipse cx="37" cy="22" rx="13" ry="7" fill="#3C4855"/>
    </svg>` },
  { id:"stand", name:"Standing at the back",
    desc:"Folds once. Tab lies under the specimen, panel rises past the rim so it reads over padding.",
    svg:`<svg viewBox="0 0 74 40" width="74" height="40">
      <path d="M8 12 L8 32 L66 32 L66 12" fill="none" stroke="#8A97A4" stroke-width="2"/>
      <rect x="12" y="4" width="46" height="24" fill="#C9A227"/>
      <rect x="12" y="28" width="26" height="4" fill="#8A6D1A"/>
      <ellipse cx="44" cy="27" rx="11" ry="5" fill="#3C4855"/>
      <line x1="12" y1="28" x2="58" y2="28" stroke="#0E1216" stroke-width="1" stroke-dasharray="2,2"/>
    </svg>` },
  { id:"wall",  name:"Flush to the back wall",
    desc:"Height capped at box depth so nothing rises above the rim.",
    svg:`<svg viewBox="0 0 74 40" width="74" height="40">
      <path d="M8 12 L8 32 L66 32 L66 12" fill="none" stroke="#8A97A4" stroke-width="2"/>
      <rect x="12" y="13" width="50" height="17" fill="#C9A227"/>
      <ellipse cx="40" cy="28" rx="12" ry="5" fill="#3C4855"/>
    </svg>` },
];

const SAMPLE = [
  "Brookite var. Arkansite | Moses Hill | Magnet Cove, AR, USA | Dug by: Justin Baird",
  "Wavellite | Mauldin Mountain | Montgomery Co., AR, USA | ex. Justin Baird",
  "Smithsonite | Rush Mining District | Marion Co., AR, USA | ex. Justin Baird",
].join("\n");

const $ = id => document.getElementById(id);

/* CSS millimetres are a fixed 96/25.4 px, so previews can be sized from the
   real viewport instead of a hardcoded width that overflows a phone. */
const MM = 96 / 25.4;
function avail(){
  const stage = document.querySelector(".stage");
  const w = (stage ? stage.clientWidth : window.innerWidth) - 92;  // card + lightbox padding
  return Math.max(160, Math.min(w, 780));
}
let boxId = "FB-25", posId = "stand";

/* ---------- geometry ---------- */
function geom(){
  const b = BOXES.find(x => x.id === boxId);
  const clear = Number($("clear").value) || 0;
  const tab = Number($("tab").value) || 0;
  const rim = Number($("rim").value) || 0;
  const W = Number($("bw").value) || b.w;
  const L = Number($("bl").value) || b.l;
  const D = Number($("bd").value) || b.d;

  if(b.perky){
    // The perky insert covers the lower third; only the top 20 mm shows.
    return { b, w:30, h:30, hidden:10, fold:null, note:"30 × 30 mm, top 20 mm visible" };
  }
  if(posId === "flat"){
    return { b, w:W - clear, h:L - clear, hidden:0, fold:null,
             note:"Full box footprint" };
  }
  if(posId === "wall"){
    return { b, w:W - clear, h:D - clear, hidden:0, fold:null,
             note:"Height capped at box depth — sits flush" };
  }
  // standing: tab on the floor, then the panel spans the depth and clears the rim
  return { b, w:W - clear, h:tab + D + rim, hidden:tab, fold:tab,
           note:`${tab} mm tab + ${D.toFixed(1)} mm depth + ${rim} mm above rim` };
}

function grid(g){
  const edge = Math.max(3, Math.min(30, Number($("edge").value) || MARGIN));
  const cols = Math.max(1, Math.floor((SHEET_W - 2 * edge) / g.w));
  const rows = Math.max(1, Math.floor((SHEET_H - 2 * edge) / g.h));
  const gw = cols * g.w, gh = rows * g.h;
  return { cols, rows, gw, gh, left:(SHEET_W - gw) / 2, top:(SHEET_H - gh) / 2 };
}

/* ---------- text fitting ---------- */
function parse(t){
  return t.split("\n").map(l => l.trim()).filter(Boolean).map(line => {
    const p = line.split("|").map(s => s.trim());
    return { sp:p[0] || "", loc:p[1] || "", reg:p[2] || "", note:p[3] || "" };
  });
}
function pickEntry(entries, i, cols, repeat, mode){
  if(!entries.length) return null;
  if(mode === "col"){
    const c = i % cols;
    if(!repeat && c >= entries.length) return null;
    return entries[c % entries.length];
  }
  return repeat ? entries[i % entries.length] : (entries[i] || null);
}

/* Type scales with the label. A 5-inch FB-6 label carries far larger type than
   a 1½-inch FB-54, so sizes derive from the available text height. */
function scaleFor(g){
  const textH = g.h - g.hidden;
  const k = Math.max(0.75, Math.min(2.6, Math.sqrt((g.w * textH) / (28 * 18))));
  return { sp: 10.5 * k, loc: 6.4 * k, note: 5 * k, rule: 8 * k };
}

function buildLabelEl(d, g, opts){
  const el = document.createElement("div");
  el.className = "lab" + (opts.border !== "none" ? " " + opts.border : "");
  el.style.width = g.w + "mm";
  el.style.height = (g.h - g.hidden) + "mm";
  if(!d) return el;
  const s = scaleFor(g);
  const long = d.sp.length;
  const spPt = s.sp * (long <= 12 ? 1 : long <= 18 ? .86 : long <= 26 ? .72 : .62);

  if(d.sp){
    const e = document.createElement("div");
    e.className = "sp"; e.style.fontSize = spPt + "pt"; e.textContent = d.sp;
    el.appendChild(e);
  }
  if(opts.divider && d.sp && (d.loc || d.reg)){
    const r = document.createElement("div");
    r.className = "rule"; r.style.width = s.rule + "mm";
    el.appendChild(r);
  }
  const locs = [d.loc, d.reg].filter(Boolean);
  if(locs.length){
    const e = document.createElement("div");
    e.className = "loc"; e.style.fontSize = (s.loc * (locs.length > 1 ? .95 : 1)) + "pt";
    e.innerHTML = locs.map(x => x.replace(/[&<>]/g, c => ({"&":"&amp;","<":"&lt;",">":"&gt;"}[c]))).join("<br>");
    el.appendChild(e);
  }
  if(d.note){
    const e = document.createElement("div");
    e.className = "note"; e.style.fontSize = s.note + "pt"; e.textContent = d.note;
    el.appendChild(e);
  }
  return el;
}

/* ---------- render ---------- */
function render(){
  const g = geom(), gr = grid(g);
  const opts = { border:$("border").value, divider:$("divider").checked };
  const entries = parse($("data").value);
  const repeat = $("repeat").checked, fill = $("fill").value;
  const total = gr.cols * gr.rows;

  const sheet = $("sheet");
  sheet.innerHTML = "";
  for(let i = 0; i < total; i++){
    const c = i % gr.cols, r = Math.floor(i / gr.cols);
    const cell = document.createElement("div");
    cell.className = "cell";
    cell.style.cssText = `left:${gr.left + c * g.w}mm;top:${gr.top + r * g.h}mm;width:${g.w}mm;height:${g.h}mm;`;
    cell.appendChild(buildLabelEl(pickEntry(entries, i, gr.cols, repeat, fill), g, opts));
    if(g.fold){
      const f = document.createElement("div");
      f.className = "foldline";
      f.style.top = (g.h - g.hidden) + "mm";
      cell.appendChild(f);
    }
    sheet.appendChild(cell);
  }
  drawGuides(sheet, g, gr);
  calibration(sheet, gr);

  // preview
  const zoom = $("zoom");
  zoom.innerHTML = "";
  const holder = document.createElement("div");
  holder.style.cssText = `position:relative;width:${g.w}mm;height:${g.h}mm;background:#fff;`;
  holder.appendChild(buildLabelEl(entries[0] || null, g, opts));
  if(g.hidden){
    const hz = document.createElement("div");
    hz.className = "hidezone"; hz.style.height = g.hidden + "mm";
    holder.appendChild(hz);
  }
  if(g.fold){
    const f = document.createElement("div");
    f.className = "foldline"; f.style.top = (g.h - g.hidden) + "mm";
    holder.appendChild(f);
  }
  const scale = Math.min(3, avail() / (g.w * MM), (avail() * 1.15) / (g.h * MM));
  holder.style.transform = `scale(${scale})`;
  holder.style.transformOrigin = "top left";
  zoom.appendChild(holder);
  $("zoomwrap").style.width = (g.w * MM * scale) + "px";
  $("zoomwrap").style.height = (g.h * MM * scale) + "px";
  $("zoomcap").innerHTML = `${g.w.toFixed(1)} × ${g.h.toFixed(1)} mm · ${g.note}` +
    (g.hidden ? ` · <span style="color:var(--celestite)">▨ hidden ${g.hidden} mm</span>` : "");

  // sheet proof scaling
  const sc = Math.min(1, avail() / (SHEET_W * MM));
  $("sheet").style.transform = `scale(${sc})`;
  $("sheet").style.transformOrigin = "top left";
  $("sheetscale").style.width = (SHEET_W * MM * sc) + "px";
  $("sheetscale").style.height = (SHEET_H * MM * sc) + "px";
  $("sheetscale").style.overflow = "hidden";

  $("spec").textContent = `${g.w.toFixed(1)} × ${g.h.toFixed(1)} mm · ${total} per sheet`;
  $("dimhint").textContent = g.note;
  plan(g, gr, total);
}

function drawGuides(sheet, g, gr){
  const mode = $("guides").value;
  if(mode === "none") return;
  const W = mode === "ticks" ? 0.2 : 0.12;
  const col = mode === "ticks" ? "#000" : "#9aa4ad";
  for(let c = 0; c <= gr.cols; c++){
    const x = gr.left + c * g.w;
    if(mode === "ticks"){
      [[0,3],[SHEET_H-3,3]].forEach(([y,h]) => add(`left:${x-W/2}mm;top:${y}mm;width:${W}mm;height:${h}mm;`));
    }else add(`left:${x-W/2}mm;top:0;width:${W}mm;height:${SHEET_H}mm;background:${col};`);
  }
  for(let r = 0; r <= gr.rows; r++){
    const y = gr.top + r * g.h;
    if(mode === "ticks"){
      [[0,3],[SHEET_W-3,3]].forEach(([x,w]) => add(`top:${y-W/2}mm;left:${x}mm;height:${W}mm;width:${w}mm;`));
    }else add(`top:${y-W/2}mm;left:0;height:${W}mm;width:${SHEET_W}mm;background:${col};`);
  }
  function add(css){
    const d = document.createElement("div");
    d.className = "tick"; d.style.cssText = css;
    sheet.appendChild(d);
  }
}

function calibration(sheet, gr){
  const x = 2.6;
  if(gr.left < 9) return;   // no room in the margin
  const bar = document.createElement("div");
  bar.className = "tick";
  bar.style.cssText = `left:${x}mm;top:${gr.top}mm;width:.25mm;height:100mm;`;
  sheet.appendChild(bar);
  [gr.top, gr.top + 100].forEach(y => {
    const c = document.createElement("div");
    c.className = "tick";
    c.style.cssText = `left:${x-1.2}mm;top:${y-0.125}mm;width:2.65mm;height:.25mm;`;
    sheet.appendChild(c);
  });
}

function plan(g, gr, total){
  const xs = [], ys = [];
  for(let c = 0; c <= gr.cols; c++) xs.push((gr.left + c * g.w).toFixed(1));
  for(let r = 0; r <= gr.rows; r++) ys.push((gr.top + r * g.h).toFixed(1));
  $("plan").innerHTML =
    `<span class="n">1</span> Trim side waste at <b>${xs[0]} mm</b> and <b>${xs[gr.cols]} mm</b> from the left edge.` +
    `<br><span class="n">2</span> Cut down every <b>${g.w.toFixed(1)} mm</b> — ${gr.cols} strip${gr.cols>1?"s":""}.` +
    `<br><span class="n">3</span> Stack the strips, trim top and bottom at <b>${ys[0]} mm</b> and <b>${ys[gr.rows]} mm</b>.` +
    `<br><span class="n">4</span> Cut across every <b>${g.h.toFixed(1)} mm</b> — ${total} labels total.` +
    (g.fold ? `<br><span class="n">5</span> Score and fold <b>${g.hidden} mm</b> up from the bottom edge — dashed line on the sheet.` : "") +
    `<br>&nbsp;<br>Grid <b>${gr.gw.toFixed(1)} × ${gr.gh.toFixed(1)} mm</b>, side margin <b>${gr.left.toFixed(1)} mm</b>, top and bottom <b>${gr.top.toFixed(1)} mm</b>.` +
    ((gr.left < 4.3 || gr.top < 4.3)
      ? `<br><span style="color:var(--sulfur)">Margin is inside the band most printers cannot reach — the outer cut ticks may not print. Raise the sheet edge value.</span>`
      : "");
}

/* ---------- PDF ---------- */
let pdfUrl = null;
function buildPDF(){
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({ unit:"mm", format:"letter" });
  const g = geom(), gr = grid(g);
  const entries = parse($("data").value);
  const repeat = $("repeat").checked, fill = $("fill").value;
  const border = $("border").value, divider = $("divider").checked;
  const total = gr.cols * gr.rows;
  const INSET = Math.min(0.8, g.w * .04);
  const s = scaleFor(g);

  for(let i = 0; i < total; i++){
    const d = pickEntry(entries, i, gr.cols, repeat, fill);
    const c = i % gr.cols, r = Math.floor(i / gr.cols);
    const x0 = gr.left + c * g.w, y0 = gr.top + r * g.h;
    const visH = g.h - g.hidden;

    if(g.fold){
      doc.setDrawColor(150,164,173); doc.setLineWidth(0.15);
      doc.setLineDashPattern([1.2,1.2], 0);
      doc.line(x0, y0 + visH, x0 + g.w, y0 + visH);
      doc.setLineDashPattern([], 0);
    }
    if(!d) continue;

    if(border !== "none"){
      doc.setDrawColor(20,52,63); doc.setLineWidth(0.25);
      doc.rect(x0 + INSET, y0 + INSET, g.w - INSET*2, visH - INSET*2);
      if(border === "dbl"){
        doc.setLineWidth(0.12);
        doc.rect(x0 + INSET + .6, y0 + INSET + .6, g.w - INSET*2 - 1.2, visH - INSET*2 - 1.2);
      }
    }

    const maxW = g.w - INSET*2 - 2.4, cx = x0 + g.w/2;
    const blocks = [];

    const rawLoc = [d.loc, d.reg].filter(Boolean);
    let locPt = s.loc;
    for(let k = 0; k < 7; k++){
      doc.setFont("helvetica","normal"); doc.setFontSize(locPt);
      if(rawLoc.every(t => doc.getTextWidth(t) <= maxW)) break;
      locPt *= 0.92;
    }
    let locLines = [];
    doc.setFontSize(locPt);
    rawLoc.forEach(t => { locLines = locLines.concat(doc.splitTextToSize(t, maxW)); });
    const locLh = locPt * PT * 1.2;

    doc.setFontSize(s.note);
    const noteLines = d.note ? doc.splitTextToSize(d.note, maxW) : [];
    const noteLh = s.note * PT * 1.18;

    const RULE = 1.35 * (s.rule/8), GAP_L = .45, GAP_N = .85;
    const useRule = divider && d.sp && rawLoc.length;
    const fixed = (useRule ? RULE : 0) + GAP_L + locLines.length*locLh +
                  (noteLines.length ? GAP_N + noteLines.length*noteLh : 0);
    const budget = visH - INSET*2 - 2.6 - fixed;

    let spPt = s.sp, spLines = [d.sp], spLh = 0;
    if(d.sp){
      for(let k = 0; k < 12; k++){
        doc.setFont("times","bold"); doc.setFontSize(spPt);
        spLines = doc.splitTextToSize(d.sp, maxW);
        spLh = spPt * PT * 1.08;
        if(spLines.length * spLh <= budget) break;
        spPt *= 0.9;
      }
      blocks.push({ k:"sp", lines:spLines, lh:spLh });
    }
    if(useRule) blocks.push({ k:"rule", lh:RULE });
    if(locLines.length) blocks.push({ k:"loc", lines:locLines, pt:locPt, lh:locLh, gap:GAP_L });
    if(noteLines.length) blocks.push({ k:"note", lines:noteLines, pt:s.note, lh:noteLh, gap:GAP_N });

    let h = 0;
    blocks.forEach(b => { h += (b.gap||0) + (b.lines ? b.lines.length*b.lh : b.lh); });
    let y = y0 + (visH - h)/2;

    blocks.forEach(b => {
      y += (b.gap||0);
      if(b.k === "rule"){
        doc.setDrawColor(20,52,63); doc.setLineWidth(0.18);
        doc.line(cx - s.rule/2, y + .5, cx + s.rule/2, y + .5);
        y += b.lh; return;
      }
      if(b.k === "sp"){ doc.setFont("times","bold"); doc.setFontSize(spPt); doc.setTextColor(0,0,0); }
      if(b.k === "loc"){ doc.setFont("helvetica","normal"); doc.setFontSize(b.pt); doc.setTextColor(0,0,0); }
      if(b.k === "note"){ doc.setFont("helvetica","oblique"); doc.setFontSize(b.pt); doc.setTextColor(42,42,42); }
      b.lines.forEach(line => { y += b.lh; doc.text(line, cx, y - b.lh*.24, { align:"center" }); });
    });
  }

  // guides
  const mode = $("guides").value;
  if(mode === "ticks"){
    doc.setDrawColor(0,0,0); doc.setLineWidth(0.2);
    for(let c = 0; c <= gr.cols; c++){ const x = gr.left + c*g.w; doc.line(x,0,x,3); doc.line(x,SHEET_H-3,x,SHEET_H); }
    for(let r = 0; r <= gr.rows; r++){ const y = gr.top + r*g.h; doc.line(0,y,3,y); doc.line(SHEET_W-3,y,SHEET_W,y); }
  }else if(mode === "lines"){
    doc.setDrawColor(150,158,166); doc.setLineWidth(0.12);
    for(let c = 0; c <= gr.cols; c++){ const x = gr.left + c*g.w; doc.line(x,0,x,SHEET_H); }
    for(let r = 0; r <= gr.rows; r++){ const y = gr.top + r*g.h; doc.line(0,y,SHEET_W,y); }
  }
  if(gr.left >= 9){
    const x = 2.6;
    doc.setDrawColor(0,0,0); doc.setLineWidth(0.25);
    doc.line(x, gr.top, x, gr.top + 100);
    doc.line(x-1.2, gr.top, x+1.2, gr.top);
    doc.line(x-1.2, gr.top+100, x+1.2, gr.top+100);
  }

  if(pdfUrl) URL.revokeObjectURL(pdfUrl);
  pdfUrl = URL.createObjectURL(doc.output("blob"));
  const link = $("pdflink");
  link.href = pdfUrl; link.hidden = false;
  $("pdfbtn").textContent = "Rebuild PDF";
}

/* ---------- wiring ---------- */
function buildControls(){
  $("box").innerHTML = BOXES.map(b => `<option value="${b.id}"${b.id===boxId?" selected":""}>${b.name}</option>`).join("");
  $("poswrap").innerHTML = POSITIONS.map(p =>
    `<div class="pos${p.id===posId?" on":""}" data-p="${p.id}">${p.svg}
       <div><div class="t">${p.name}</div><div class="d">${p.desc}</div></div></div>`).join("");
  $("poswrap").querySelectorAll(".pos").forEach(el => el.onclick = () => {
    posId = el.dataset.p; buildControls(); invalidate(); render();
  });
  const b = BOXES.find(x => x.id === boxId);
  let hint = b.perky
    ? "The standard perky insert fixes the label at 30 × 30 mm with the lower 10 mm hidden — position options do not apply."
    : `${b.w} × ${b.l} mm footprint, ${b.d} mm high. Edit any of these if yours measure differently.`;
  if(b.est) hint += " Height is an estimate — the supplier does not publish it, so measure yours.";
  if(b.gem) hint += " Gem boxes suspend the stone in film, so a label usually goes behind rather than under it.";
  $("boxhint").textContent = hint;
  const perky = !!b.perky, stand = posId === "stand" && !perky;
  $("poswrap").style.opacity = perky ? .4 : 1;
  $("poswrap").style.pointerEvents = perky ? "none" : "auto";
  $("tabwrap").style.display = stand ? "" : "none";
  $("rimwrap").style.display = stand ? "" : "none";
}
function invalidate(){
  const l = $("pdflink");
  if(!l.hidden){ l.hidden = true; $("pdfbtn").textContent = "Build PDF"; }
}
function loadDims(){
  const b = BOXES.find(x => x.id === boxId);
  $("bw").value = b.w; $("bl").value = b.l; $("bd").value = b.d;
}
$("data").value = SAMPLE;
loadDims();
["box","bw","bl","bd","clear","tab","rim","edge","data","fill","repeat","border","divider","guides"].forEach(id => {
  $(id).addEventListener("input", () => {
    if(id === "box"){
      boxId = $("box").value;
      loadDims(); // Selecting a box loads its catalog dimensions; fields remain editable afterward.
    }
    buildControls(); invalidate(); render();
  });
  $(id).addEventListener("change", () => {
    if(id === "box"){
      boxId = $("box").value;
      loadDims(); // Selecting a box loads its catalog dimensions; fields remain editable afterward.
    }
    buildControls(); invalidate(); render();
  });
});
$("pdfbtn").onclick = () => {
  if(!window.jspdf){ $("pdfbtn").textContent = "PDF library didn't load"; return; }
  buildPDF();
};
document.getElementById("helpBtn").onclick = () => document.getElementById("help").classList.add("open");
document.getElementById("helpClose").onclick = () => document.getElementById("help").classList.remove("open");

let resizeTimer = null;
window.addEventListener("resize", () => {
  clearTimeout(resizeTimer);
  resizeTimer = setTimeout(render, 140);
});

buildControls();
render();
</script>
</body>
</html>
