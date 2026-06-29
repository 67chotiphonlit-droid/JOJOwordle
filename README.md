<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>JoJo-dle</title>
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;500;600&display=swap" rel="stylesheet"/>
<style>
*{box-sizing:border-box;margin:0;padding:0}
:root{
  --gold:#D4A017;
  --dark:#0D0C0E;
  --surface:#1A1820;
  --surface2:#242130;
  --border:#3A3648;
  --text:#F0EEF8;
  --muted:#8A869E;
  --correct:#1D9E75;
  --correct-bg:#0A3D2E;
  --correct-text:#5DCAA5;
  --partial:#BA7517;
  --partial-bg:#3D2800;
  --partial-text:#EF9F27;
  --wrong:#4A4760;
  --wrong-text:#8A869E;
}
body{background:var(--dark);color:var(--text);font-family:'Inter',sans-serif;min-height:100vh;overflow-x:hidden}

header{border-bottom:1px solid var(--border);padding:0 2rem}
.header-inner{max-width:960px;margin:0 auto;display:flex;align-items:center;justify-content:space-between;height:64px}
.logo{font-family:'Bebas Neue',sans-serif;font-size:32px;letter-spacing:3px;color:var(--gold);line-height:1}
.logo span{color:var(--text);font-size:18px;letter-spacing:1px;margin-left:4px;vertical-align:middle;font-family:'Inter',sans-serif;font-weight:500}
.tries-badge{font-size:13px;color:var(--muted)}
.tries-badge strong{color:var(--text);font-weight:600}

.hero{padding:2rem 2rem 1.5rem;background:repeating-linear-gradient(135deg,transparent,transparent 20px,rgba(212,160,23,0.04) 20px,rgba(212,160,23,0.04) 21px)}
.hero-inner{max-width:960px;margin:0 auto}
.hero h2{font-family:'Bebas Neue',sans-serif;font-size:clamp(26px,5vw,48px);letter-spacing:4px;line-height:1.05;color:var(--text)}
.hero h2 em{color:var(--gold);font-style:normal}
.hero p{margin-top:6px;font-size:14px;color:var(--muted);max-width:520px;line-height:1.6}
.legend{display:flex;gap:16px;margin-top:14px;flex-wrap:wrap}
.leg{display:flex;align-items:center;gap:7px;font-size:12px;color:var(--muted)}
.leg-box{width:13px;height:13px;border-radius:3px;flex-shrink:0}

/* YESTERDAY BANNER */
#yesterday-banner{max-width:960px;margin:1rem auto 0;padding:0 2rem}
.yday-card{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:14px 16px;display:flex;align-items:center;gap:14px}
.yday-img{width:54px;height:54px;border-radius:8px;object-fit:cover;border:1px solid var(--border);background:var(--surface2);flex-shrink:0}
.yday-img.placeholder{display:flex;align-items:center;justify-content:center;font-size:22px}
.yday-info{flex:1}
.yday-label{font-size:11px;font-weight:600;letter-spacing:.8px;text-transform:uppercase;color:var(--muted);margin-bottom:3px}
.yday-name{font-size:16px;font-weight:600;color:var(--gold)}
.yday-sub{font-size:12px;color:var(--muted);margin-top:2px}

main{max-width:960px;margin:0 auto;padding:1.25rem 2rem 4rem}

.input-area{position:relative;margin-bottom:1.25rem}
.search-row{display:flex;gap:8px}
#search{flex:1;padding:12px 16px;font-size:15px;background:var(--surface);border:1px solid var(--border);border-radius:8px;color:var(--text);outline:none;transition:border-color .15s}
#search:focus{border-color:var(--gold)}
#search::placeholder{color:var(--muted)}
#guess-btn{padding:12px 24px;font-size:14px;font-weight:600;background:var(--gold);color:var(--dark);border:none;border-radius:8px;cursor:pointer;letter-spacing:.5px;transition:opacity .15s,transform .1s;white-space:nowrap}
#guess-btn:hover{opacity:.9}
#guess-btn:active{transform:scale(.97)}
#guess-btn:disabled{opacity:.4;cursor:default}

#dropdown{position:absolute;top:calc(100% + 4px);left:0;right:90px;background:var(--surface2);border:1px solid var(--border);border-radius:8px;max-height:280px;overflow-y:auto;display:none;z-index:100;box-shadow:0 8px 24px rgba(0,0,0,.5)}
.dd-item{padding:8px 12px;cursor:pointer;font-size:13px;color:var(--text);display:flex;align-items:center;gap:10px}
.dd-item:hover,.dd-item.selected{background:rgba(212,160,23,.15);color:var(--gold)}
.dd-thumb{width:40px;height:40px;border-radius:6px;object-fit:cover;background:var(--surface);border:1px solid var(--border);flex-shrink:0;display:flex;align-items:center;justify-content:center;font-size:18px;overflow:hidden}
.dd-thumb img{width:100%;height:100%;object-fit:cover;border-radius:5px}
.dd-info{display:flex;flex-direction:column;gap:2px}
.dd-stand-label{font-size:11px;color:var(--muted);font-style:italic}
.dd-item:hover .dd-stand-label,.dd-item.selected .dd-stand-label{color:rgba(212,160,23,.7)}

#msg{padding:12px 16px;border-radius:8px;font-size:14px;font-weight:500;margin-bottom:1.25rem;display:none;line-height:1.5}
#msg.info{background:rgba(212,160,23,.12);color:var(--gold);border:1px solid rgba(212,160,23,.3)}
#msg.win{background:var(--correct-bg);color:var(--correct-text);border:1px solid var(--correct)}
#msg.lose{background:#3D0A0A;color:#F09595;border:1px solid #A32D2D}

/* GRID — 8 columns now: name, img, part, stand, stand-type, nationality, role, hair */
.grid-header,.guess-row{display:grid;grid-template-columns:56px 140px repeat(6,1fr);gap:5px;margin-bottom:5px}
.grid-header span{font-size:10px;font-weight:600;color:var(--muted);text-align:center;padding:4px 2px;letter-spacing:.7px;text-transform:uppercase}
.grid-header span:first-child,.grid-header span:nth-child(2){text-align:left}
.grid-header span:first-child{padding-left:4px}
.grid-header span:nth-child(2){padding-left:10px}

.cell{border-radius:7px;padding:8px 4px;text-align:center;font-size:11px;font-weight:600;display:flex;align-items:center;justify-content:center;min-height:56px;line-height:1.3;letter-spacing:.2px}
.cell.name{text-align:left;justify-content:flex-start;padding:8px 12px;font-size:12px;font-weight:500;background:var(--surface);border:1px solid var(--border);color:var(--text)}
.cell.portrait{background:var(--surface2);border:1px solid var(--border);padding:3px;border-radius:7px;overflow:hidden}
.cell.portrait img{width:100%;height:100%;object-fit:cover;border-radius:5px;display:block}
.cell.portrait .no-img{width:100%;height:100%;display:flex;align-items:center;justify-content:center;font-size:20px;background:var(--surface)}
.correct{background:var(--correct-bg);color:var(--correct-text);border:1px solid var(--correct)}
.wrong{background:var(--surface);color:var(--wrong-text);border:1px solid var(--border)}
.partial{background:var(--partial-bg);color:var(--partial-text);border:1px solid var(--partial)}

#share-section{display:none;margin-top:1.5rem;padding:1.25rem;background:var(--surface);border:1px solid var(--border);border-radius:10px}
#share-section h3{font-size:15px;font-weight:600;margin-bottom:10px;color:var(--text)}
#share-text{width:100%;padding:10px 12px;font-size:13px;font-family:'Inter',monospace;background:var(--surface2);border:1px solid var(--border);border-radius:6px;color:var(--muted);resize:none;height:90px;outline:none}
#copy-btn{margin-top:8px;padding:8px 18px;font-size:13px;font-weight:600;background:transparent;border:1px solid var(--gold);color:var(--gold);border-radius:6px;cursor:pointer;transition:background .15s}
#copy-btn:hover{background:rgba(212,160,23,.1)}

footer{text-align:center;padding:2rem;font-size:12px;color:var(--muted);border-top:1px solid var(--border)}

@keyframes slideIn{from{opacity:0;transform:translateY(-8px)}to{opacity:1;transform:translateY(0)}}
.guess-row{animation:slideIn .3s ease}

@media(max-width:680px){
  .header-inner,#yesterday-banner,main{padding-left:1rem;padding-right:1rem}
  header{padding:0 1rem}
  .hero{padding:1.25rem 1rem}
  .grid-header,.guess-row{grid-template-columns:44px 110px repeat(6,1fr);gap:3px}
  .cell{font-size:9px;min-height:46px;padding:5px 2px}
  .cell.name{font-size:10px;padding:5px 7px}
  .grid-header span{font-size:8px}
  #dropdown{right:0}
}
</style>
</head>
<body>

<header>
  <div class="header-inner">
    <div class="logo">JoJo<span>-dle</span></div>
    <div class="tries-badge">Daily puzzle — <strong id="date-display"></strong></div>
  </div>
</header>

<div class="hero">
  <div class="hero-inner">
    <h2>GUESS THE <em>BIZARRE</em> CHARACTER</h2>
    <p>One JoJo's Bizarre Adventure character each day. 8 tries. Use clues to narrow it down.</p>
    <div class="legend">
      <div class="leg"><div class="leg-box" style="background:var(--correct)"></div>Correct</div>
      <div class="leg"><div class="leg-box" style="background:var(--partial)"></div>Close (Part ±1)</div>
      <div class="leg"><div class="leg-box" style="background:var(--wrong)"></div>Wrong</div>
    </div>
  </div>
</div>

<div id="yesterday-banner">
  <div class="yday-card">
    <div id="yday-img-wrap"></div>
    <div class="yday-info">
      <div class="yday-label">Yesterday's answer</div>
      <div class="yday-name" id="yday-name">—</div>
      <div class="yday-sub" id="yday-sub">—</div>
    </div>
  </div>
</div>

<main>
  <div class="input-area">
    <div class="search-row">
      <input id="search" type="text" placeholder="Search by character name or stand name…" autocomplete="off"/>
      <button id="guess-btn">Guess</button>
    </div>
    <div id="dropdown"></div>
  </div>

  <div id="msg"></div>

  <div class="grid-header">
    <span></span>
    <span>Character</span>
    <span>Part</span>
    <span>Stand?</span>
    <span>Stand type</span>
    <span>Nationality</span>
    <span>Role</span>
    <span>Hair</span>
  </div>
  <div id="guesses"></div>

  <div id="share-section">
    <h3>Share your result</h3>
    <textarea id="share-text" readonly></textarea>
    <button id="copy-btn">Copy to clipboard</button>
  </div>
</main>

<footer>
  JoJo-dle — Non-profit fan project. JoJo's Bizarre Adventure &copy; Hirohiko Araki / Shueisha. No affiliation with the official franchise.
</footer>

<script>
// Stand range types: Close-Range, Long-Range, Remote, Automatic, Bound, Colony, Integrated, None
const CHARS=[
  {name:"Jonathan Joestar",part:1,stand:false,standName:null,standType:"None",nationality:"British",role:"Protagonist",hair:"Brown",img:"https://static.wikia.nocookie.net/jjba/images/1/18/Jonathan_Joestar_Infobox_Manga.png",emoji:"🎩"},
  {name:"Robert E.O. Speedwagon",part:1,stand:false,standName:null,standType:"None",nationality:"British",role:"Ally",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/a/a1/Speedwagon_Infobox_Manga.png",emoji:"🎩"},
  {name:"Dio Brando",part:1,stand:true,standName:"The World",standType:"Close-Range",nationality:"British",role:"Villain",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/2/2f/Dio_Brando_Infobox_Manga.png",emoji:"🧛"},
  {name:"Will A. Zeppeli",part:1,stand:false,standName:null,standType:"None",nationality:"Italian",role:"Ally",hair:"Brown",img:"https://static.wikia.nocookie.net/jjba/images/3/32/Will_Anthonio_Zeppeli_Infobox_Manga.png",emoji:"🎩"},
  {name:"Erina Pendleton",part:1,stand:false,standName:null,standType:"None",nationality:"British",role:"Ally",hair:"Brown",img:"https://static.wikia.nocookie.net/jjba/images/e/e0/Erina_Infobox_Manga.png",emoji:"💐"},
  {name:"Joseph Joestar",part:2,stand:true,standName:"Hermit Purple",standType:"Close-Range",nationality:"British",role:"Protagonist",hair:"Brown",img:"https://static.jojowiki.com/images/b/b7/latest/20230129074253/Johnny_Joestar_Infobox_Manga.png",emoji:"🔮"},
  {name:"Caesar Anthonio Zeppeli",part:2,stand:false,standName:null,standType:"None",nationality:"Italian",role:"Ally",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/5/52/Caesar_Infobox_Manga.png",emoji:"🫧"},
  {name:"Lisa Lisa",part:2,stand:false,standName:null,standType:"None",nationality:"American",role:"Ally",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/b/b3/Lisa_Lisa_Infobox_Manga.png",emoji:"🌹"},
  {name:"Kars",part:2,stand:false,standName:null,standType:"None",nationality:"Other",role:"Villain",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/f/f9/Kars_Infobox_Manga.png",emoji:"🌙"},
  {name:"Wamuu",part:2,stand:false,standName:null,standType:"None",nationality:"Other",role:"Villain",hair:"White",img:"https://static.wikia.nocookie.net/jjba/images/1/10/Wamuu_Infobox_Manga.png",emoji:"💨"},
  {name:"Jotaro Kujo",part:3,stand:true,standName:"Star Platinum",standType:"Close-Range",nationality:"Japanese",role:"Protagonist",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/d/d9/Jotaro_Kujo_Infobox_Manga.png",emoji:"⭐"},
  {name:"Noriaki Kakyoin",part:3,stand:true,standName:"Hierophant Green",standType:"Long-Range",nationality:"Japanese",role:"Ally",hair:"Red",img:"https://static.wikia.nocookie.net/jjba/images/1/18/Kakyoin_Infobox_Manga.png",emoji:"🕸️"},
  {name:"Jean Pierre Polnareff",part:3,stand:true,standName:"Silver Chariot",standType:"Close-Range",nationality:"French",role:"Ally",hair:"Silver",img:"https://static.wikia.nocookie.net/jjba/images/0/05/Polnareff_Infobox_Manga.png",emoji:"⚔️"},
  {name:"Muhammad Avdol",part:3,stand:true,standName:"Magician's Red",standType:"Close-Range",nationality:"Egyptian",role:"Ally",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/8/8c/Avdol_Infobox_Manga.png",emoji:"🔥"},
  {name:"Iggy",part:3,stand:true,standName:"The Fool",standType:"Close-Range",nationality:"American",role:"Ally",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/5/52/Iggy_Infobox_Manga.png",emoji:"🐶"},
  {name:"DIO",part:3,stand:true,standName:"The World",standType:"Close-Range",nationality:"British",role:"Villain",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/9/9d/DIO_Infobox_Manga.png",emoji:"🧛"},
  {name:"Josuke Higashikata",part:4,stand:true,standName:"Crazy Diamond",standType:"Close-Range",nationality:"Japanese",role:"Protagonist",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/5/5e/Josuke_Higashikata_Infobox_Manga.png",emoji:"💎"},
  {name:"Koichi Hirose",part:4,stand:true,standName:"Echoes",standType:"Close-Range",nationality:"Japanese",role:"Ally",hair:"Brown",img:"https://static.wikia.nocookie.net/jjba/images/c/c5/Koichi_Hirose_Infobox_Manga.png",emoji:"🥚"},
  {name:"Okuyasu Nijimura",part:4,stand:true,standName:"The Hand",standType:"Close-Range",nationality:"Japanese",role:"Ally",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/e/e5/Okuyasu_Infobox_Manga.png",emoji:"✋"},
  {name:"Rohan Kishibe",part:4,stand:true,standName:"Heaven's Door",standType:"Close-Range",nationality:"Japanese",role:"Neutral",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/9/98/Rohan_Kishibe_Infobox_Manga.png",emoji:"📖"},
  {name:"Yoshikage Kira",part:4,stand:true,standName:"Killer Queen",standType:"Close-Range",nationality:"Japanese",role:"Villain",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/4/4e/Yoshikage_Kira_Infobox_Manga.png",emoji:"💣"},
  {name:"Giorno Giovanna",part:5,stand:true,standName:"Gold Experience",standType:"Close-Range",nationality:"Italian",role:"Protagonist",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/5/5f/Giorno_Giovanna_Infobox_Manga.png",emoji:"🌿"},
  {name:"Bruno Bucciarati",part:5,stand:true,standName:"Sticky Fingers",standType:"Close-Range",nationality:"Italian",role:"Ally",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/4/43/Bucciarati_Infobox_Manga.png",emoji:"🤐"},
  {name:"Guido Mista",part:5,stand:true,standName:"Sex Pistols",standType:"Long-Range",nationality:"Italian",role:"Ally",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/7/7a/Mista_Infobox_Manga.png",emoji:"🔫"},
  {name:"Narancia Ghirga",part:5,stand:true,standName:"Aerosmith",standType:"Long-Range",nationality:"Italian",role:"Ally",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/f/f5/Narancia_Infobox_Manga.png",emoji:"✈️"},
  {name:"Pannacotta Fugo",part:5,stand:true,standName:"Purple Haze",standType:"Close-Range",nationality:"Italian",role:"Ally",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/1/17/Fugo_Infobox_Manga.png",emoji:"☣️"},
  {name:"Trish Una",part:5,stand:true,standName:"Spice Girl",standType:"Close-Range",nationality:"Italian",role:"Ally",hair:"Pink",img:"https://static.wikia.nocookie.net/jjba/images/6/6e/Trish_Una_Infobox_Manga.png",emoji:"💅"},
  {name:"Diavolo",part:5,stand:true,standName:"King Crimson",standType:"Close-Range",nationality:"Italian",role:"Villain",hair:"Pink",img:"https://static.wikia.nocookie.net/jjba/images/8/86/Diavolo_Infobox_Manga.png",emoji:"👁️"},
  {name:"Jolyne Cujoh",part:6,stand:true,standName:"Stone Free",standType:"Close-Range",nationality:"American",role:"Protagonist",hair:"Green",img:"https://static.wikia.nocookie.net/jjba/images/6/62/Jolyne_Cujoh_Infobox_Manga.png",emoji:"🧵"},
  {name:"Hermes Costello",part:6,stand:true,standName:"Kiss",standType:"Close-Range",nationality:"American",role:"Ally",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/5/5d/Hermes_Infobox_Manga.png",emoji:"💋"},
  {name:"Foo Fighters",part:6,stand:true,standName:"Foo Fighters",standType:"Colony",nationality:"Other",role:"Ally",hair:"Green",img:"https://static.wikia.nocookie.net/jjba/images/3/38/Foo_Fighters_Infobox_Manga.png",emoji:"🦠"},
  {name:"Enrico Pucci",part:6,stand:true,standName:"Whitesnake",standType:"Close-Range",nationality:"American",role:"Villain",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/4/43/Enrico_Pucci_Infobox_Manga.png",emoji:"🌀"},
  {name:"Johnny Joestar",part:7,stand:true,standName:"Tusk",standType:"Long-Range",nationality:"American",role:"Protagonist",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/4/48/Johnny_Joestar_Infobox_Manga.png",emoji:"🌀"},
  {name:"Gyro Zeppeli",part:7,stand:true,standName:"Ball Breaker",standType:"Long-Range",nationality:"Italian",role:"Ally",hair:"Blond",img:"images/Gyro.webp",emoji:"🎱"},
  {name:"Diego Brando",part:7,stand:true,standName:"Scary Monsters",standType:"Bound",nationality:"British",role:"Villain",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/0/0e/Diego_Brando_Infobox_Manga.png",emoji:"🦕"},
  {name:"Funny Valentine",part:7,stand:true,standName:"Dirty Deeds Done Dirt Cheap",standType:"Close-Range",nationality:"American",role:"Villain",hair:"Blond",img:"https://static.wikia.nocookie.net/jjba/images/d/d8/Funny_Valentine_Infobox_Manga.png",emoji:"🇺🇸"},
  {name:"Gappy (Josuke)",part:8,stand:true,standName:"Soft & Wet",standType:"Close-Range",nationality:"Japanese",role:"Protagonist",hair:"Black",img:"https://static.wikia.nocookie.net/jjba/images/2/2e/Josuke8_Infobox_Manga.png",emoji:"🫧"},
  {name:"Yasuho Hirose",part:8,stand:true,standName:"Paisley Park",standType:"Remote",nationality:"Japanese",role:"Ally",hair:"Pink",img:"https://static.wikia.nocookie.net/jjba/images/7/74/Yasuho_Hirose_Infobox_Manga.png",emoji:"🌸"},
  {name:"Norisuke Higashikata IV",part:8,stand:true,standName:"King Nothing",standType:"Close-Range",nationality:"Japanese",role:"Ally",hair:"Brown",img:"https://static.wikia.nocookie.net/jjba/images/8/86/Norisuke_IV_Infobox_Manga.png",emoji:"🔍"},
  {name:"Tooru",part:8,stand:true,standName:"Wonder of U",standType:"Automatic",nationality:"Japanese",role:"Villain",hair:"Silver",img:"https://static.wikia.nocookie.net/jjba/images/9/9e/Tooru_Infobox_Manga.png",emoji:"♾️"},
];

const COLS=["part","stand","standType","nationality","role","hair"];
const EMOJI_RESULT={correct:"🟩",partial:"🟧",wrong:"⬛"};

const todayIdx=Math.floor(Date.now()/86400000)%CHARS.length;
const yesterdayIdx=(todayIdx-1+CHARS.length)%CHARS.length;
const target=CHARS[todayIdx];
const yesterday=CHARS[yesterdayIdx];

let guesses=[],gameOver=false;
const MAX=8;

// Date display
const todayStr=new Date().toLocaleDateString("en-US",{month:"short",day:"numeric",year:"numeric"});
document.getElementById("date-display").textContent=todayStr;

// Yesterday banner
const ydayImgWrap=document.getElementById("yday-img-wrap");
if(yesterday.img){
  const imgEl=document.createElement("img");
  imgEl.src=yesterday.img;
  imgEl.alt=yesterday.name;
  imgEl.className="yday-img";
  imgEl.onerror=()=>{imgEl.replaceWith(makePlaceholder(yesterday.emoji));};
  ydayImgWrap.appendChild(imgEl);
}else{
  ydayImgWrap.appendChild(makePlaceholder(yesterday.emoji));
}
function makePlaceholder(emoji){
  const d=document.createElement("div");
  d.className="yday-img placeholder";
  d.textContent=emoji;
  return d;
}
document.getElementById("yday-name").textContent=yesterday.name;
document.getElementById("yday-sub").textContent=
  `Part ${yesterday.part} · ${yesterday.stand?(yesterday.standName+" ("+yesterday.standType+")"):"No Stand"} · ${yesterday.role}`;

// Elements
const searchEl=document.getElementById("search");
const dropdown=document.getElementById("dropdown");
const guessesEl=document.getElementById("guesses");
const msgEl=document.getElementById("msg");

function showMsg(text,type){msgEl.textContent=text;msgEl.className=type;msgEl.style.display="block";}

let ddSelected=-1;
searchEl.addEventListener("input",()=>{
  ddSelected=-1;
  const q=searchEl.value.trim().toLowerCase();
  if(!q){dropdown.style.display="none";return;}
  const used=guesses.map(g=>g.name);
  // Match on character name OR stand name
  const hits=CHARS.filter(c=>{
    if(used.includes(c.name))return false;
    if(c.name.toLowerCase().includes(q))return true;
    if(c.standName&&c.standName.toLowerCase().includes(q))return true;
    return false;
  });
  if(!hits.length){dropdown.style.display="none";return;}
  dropdown.innerHTML=hits.slice(0,10).map((c,i)=>{
    const standLabel=c.stand?`Stand: ${c.standName}`:`No Stand`;
    const thumb=c.img
      ?`<div class="dd-thumb"><img src="${c.img}" alt="${c.name}" onerror="this.parentElement.textContent='${c.emoji}'"/></div>`
      :`<div class="dd-thumb">${c.emoji}</div>`;
    return`<div class="dd-item" data-name="${c.name}" data-i="${i}">${thumb}<div class="dd-info"><span>${c.name}</span><span class="dd-stand-label">${standLabel}</span></div></div>`;
  }).join("");
  dropdown.style.display="block";
  dropdown.querySelectorAll(".dd-item").forEach(el=>{
    el.addEventListener("click",()=>{searchEl.value=el.dataset.name;dropdown.style.display="none";});
  });
});

searchEl.addEventListener("keydown",e=>{
  const items=dropdown.querySelectorAll(".dd-item");
  if(e.key==="ArrowDown"){e.preventDefault();ddSelected=Math.min(ddSelected+1,items.length-1);highlight(items);}
  else if(e.key==="ArrowUp"){e.preventDefault();ddSelected=Math.max(ddSelected-1,-1);highlight(items);}
  else if(e.key==="Enter"){
    if(ddSelected>=0&&items[ddSelected]){searchEl.value=items[ddSelected].dataset.name;dropdown.style.display="none";}
    else makeGuess();
  }else if(e.key==="Escape"){dropdown.style.display="none";}
});
function highlight(items){items.forEach((el,i)=>el.classList.toggle("selected",i===ddSelected));}
document.addEventListener("click",e=>{if(!e.target.closest(".input-area"))dropdown.style.display="none";});
document.getElementById("guess-btn").addEventListener("click",makeGuess);

function compare(guess,key){
  const gv=guess[key],tv=target[key];
  if(gv===tv)return"correct";
  if(key==="part"&&typeof gv==="number"&&Math.abs(gv-tv)===1)return"partial";
  return"wrong";
}

function makeGuess(){
  if(gameOver)return;
  const name=searchEl.value.trim();
  const char=CHARS.find(c=>c.name.toLowerCase()===name.toLowerCase());
  if(!char){showMsg("Character not found — check spelling or try their stand name.","info");return;}
  if(guesses.find(g=>g.name===char.name)){showMsg("Already guessed that one!","info");return;}
  guesses.push(char);
  renderRow(char);
  searchEl.value="";
  dropdown.style.display="none";
  msgEl.style.display="none";
  if(char.name===target.name){
    showMsg(`Yare yare… You got it in ${guesses.length} ${guesses.length===1?"try":"tries"}! It was ${target.name}.`,"win");
    gameOver=true;revealShare(true);
    document.getElementById("guess-btn").disabled=true;searchEl.disabled=true;
  }else if(guesses.length>=MAX){
    showMsg(`No more tries! The answer was ${target.name} — ${target.stand?target.standName:"no stand"}.`,"lose");
    gameOver=true;revealShare(false);
    document.getElementById("guess-btn").disabled=true;searchEl.disabled=true;
  }
}

function renderRow(char){
  const row=document.createElement("div");
  row.className="guess-row";

  // Portrait cell
  const portCell=document.createElement("div");
  portCell.className="cell portrait";
  if(char.img){
    const im=document.createElement("img");
    im.src=char.img;im.alt=char.name;
    im.onerror=()=>{im.style.display="none";portCell.querySelector(".no-img").style.display="flex";};
    const fallback=document.createElement("div");
    fallback.className="no-img";fallback.textContent=char.emoji;fallback.style.display="none";
    portCell.appendChild(im);portCell.appendChild(fallback);
  }else{
    const fb=document.createElement("div");
    fb.className="no-img";fb.textContent=char.emoji;
    portCell.appendChild(fb);
  }
  row.appendChild(portCell);

  // Name cell
  const nameCell=document.createElement("div");
  nameCell.className="cell name";nameCell.textContent=char.name;
  row.appendChild(nameCell);

  // Data cells
  COLS.forEach(key=>{
    const result=compare(char,key);
    const cell=document.createElement("div");
    cell.className="cell "+result;
    let val=char[key];
    if(key==="stand")val=val?"Yes":"No";
    if(key==="part")val="Part "+val;
    if(key==="standType"&&!char.stand)val="None";
    if(result==="partial"&&key==="part")val+=char[key]<target[key]?" ↑":" ↓";
    cell.textContent=val;
    row.appendChild(cell);
  });
  guessesEl.prepend(row);
}

function revealShare(won){
  document.getElementById("share-section").style.display="block";
  const lines=guesses.map(char=>COLS.map(key=>EMOJI_RESULT[compare(char,key)]).join(""));
  const result=won?`${guesses.length}/${MAX}`:`X/${MAX}`;
  const text=`JoJo-dle ${todayStr} — ${result}\n\n${lines.join("\n")}\n\nhttps://67chotiphonlit-droid.github.io/JOJOwordle/`;
  document.getElementById("share-text").value=text;
}
document.getElementById("copy-btn").addEventListener("click",()=>{
  navigator.clipboard.writeText(document.getElementById("share-text").value).then(()=>{
    document.getElementById("copy-btn").textContent="Copied!";
    setTimeout(()=>document.getElementById("copy-btn").textContent="Copy to clipboard",2000);
  });
});
</script>
</body>
</html>