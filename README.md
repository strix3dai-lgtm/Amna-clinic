<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, user-scalable=no">
  <meta name="theme-color" content="#13b9b1">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <title>Amna Clinic - Save the Smiles!</title>
  <style>
    :root{--teal:#18c8bd;--deep:#087d83;--sky:#aeeaff;--pink:#ff7bac;--ink:#16445a;--cream:#f7feff;--shadow:0 12px 30px rgba(17,99,120,.18)}
    *{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
    html,body{height:100%;margin:0;overflow:hidden;background:#dffaff;font-family:Arial,"Helvetica Neue",sans-serif;color:var(--ink);touch-action:manipulation}
    button{font:inherit;border:0;cursor:pointer;color:inherit}
    button:focus-visible{outline:4px solid #fff;box-shadow:0 0 0 7px var(--pink)}
    .app{position:relative;isolation:isolate;height:100%;height:100dvh;min-height:560px;overflow:hidden;background:linear-gradient(160deg,#dffbff 0%,#bdeffd 46%,#b6f2ed 100%)}
    .bubbles,.lights{position:absolute;inset:0;overflow:hidden;pointer-events:none;z-index:-1}
    .bubble{position:absolute;bottom:-90px;width:34px;height:34px;border:3px solid rgba(255,255,255,.52);border-radius:50%;animation:float 10s linear infinite}
    .bubble:nth-child(1){left:6%;animation-delay:-2s}.bubble:nth-child(2){left:24%;width:18px;height:18px;animation-delay:-7s}.bubble:nth-child(3){left:67%;width:52px;height:52px;animation-delay:-4s}.bubble:nth-child(4){left:88%;width:25px;height:25px;animation-delay:-9s}.bubble:nth-child(5){left:47%;width:12px;height:12px;animation-delay:-5s}
    @keyframes float{to{transform:translateY(-115vh) rotate(180deg);opacity:.05}}
    .lights:before,.lights:after{content:"";position:absolute;top:-52px;width:120px;height:86px;border-radius:50%;background:#fff;box-shadow:0 10px 35px 15px rgba(255,255,255,.8);animation:glow 2.4s ease-in-out infinite alternate}
    .lights:before{left:14%}.lights:after{right:14%;animation-delay:-1.2s}@keyframes glow{to{opacity:.65;transform:scale(.92)}}

    /* Screens */
    .screen{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;padding:24px calc(20px + env(safe-area-inset-right)) calc(24px + env(safe-area-inset-bottom)) calc(20px + env(safe-area-inset-left));z-index:20;transition:.35s ease}
    .screen.hidden{opacity:0;visibility:hidden;transform:scale(1.06);pointer-events:none}
    .card{width:min(92vw,500px);padding:30px 24px;text-align:center;border:3px solid rgba(255,255,255,.9);border-radius:32px;background:rgba(255,255,255,.86);box-shadow:0 24px 60px rgba(10,101,123,.24);backdrop-filter:blur(10px)}
    .logo{position:relative;width:112px;height:112px;margin:0 auto 10px;border-radius:34px;background:linear-gradient(145deg,#23d5ca,#079ca5);box-shadow:0 13px 0 #087b86,0 20px 30px rgba(5,119,130,.25);animation:logoBob 2.4s ease-in-out infinite}
    .logo:before{content:"";position:absolute;left:26px;top:19px;width:59px;height:67px;background:white;border-radius:45% 45% 50% 50%;clip-path:polygon(0 0,100% 0,90% 63%,73% 100%,54% 66%,45% 66%,27% 100%,10% 63%)}
    .logo:after{content:"✦";position:absolute;right:10px;top:5px;color:#ffe66d;font-size:30px}@keyframes logoBob{50%{transform:translateY(-7px) rotate(2deg)}}
    h1{font-size:clamp(38px,10vw,62px);line-height:.95;margin:20px 0 8px;color:var(--deep);letter-spacing:-2px;text-shadow:0 3px #fff}
    .slogan{margin:0 0 22px;color:var(--pink);font-weight:900;font-size:clamp(20px,5vw,28px)}
    .intro{margin:0 auto 24px;max-width:360px;line-height:1.45;font-weight:700;color:#4e7584}
    .primary{position:relative;min-width:210px;padding:17px 30px;border-radius:20px;color:white;background:linear-gradient(#ff8bb6,#f24d8a);box-shadow:0 8px 0 #c82e6b,0 14px 25px rgba(242,77,138,.28);font-size:22px;font-weight:900;letter-spacing:.4px;transition:.12s}
    .primary:active{transform:translateY(6px);box-shadow:0 2px 0 #c82e6b}
    .sound-note{font-size:12px;margin-top:20px;color:#6b8994;font-weight:bold}

    /* Game HUD */
    .game{height:100%;display:grid;grid-template-rows:auto 1fr auto;gap:8px;padding:calc(9px + env(safe-area-inset-top)) max(10px,env(safe-area-inset-right)) calc(9px + env(safe-area-inset-bottom)) max(10px,env(safe-area-inset-left));opacity:1;transition:.25s}
    .game.inactive{opacity:.45;filter:blur(3px);pointer-events:none}
    .hud{display:grid;grid-template-columns:repeat(4,1fr);gap:7px;z-index:3}
    .stat{min-width:0;padding:7px 4px;border:2px solid white;border-radius:15px;background:rgba(255,255,255,.86);box-shadow:0 5px 13px rgba(13,110,125,.12);text-align:center}
    .stat .label{display:block;font-size:9px;letter-spacing:.7px;font-weight:900;color:#6c94a1;text-transform:uppercase}
    .stat strong{display:block;margin-top:2px;font-size:18px;color:var(--deep);white-space:nowrap}.stat.timer strong{color:#ed477f}.stat.danger{animation:danger .55s infinite alternate;background:#fff0f3}@keyframes danger{to{transform:scale(1.05)}}

    /* Clinic room and patient */
    .clinic{position:relative;display:flex;min-height:0;align-items:center;justify-content:center;overflow:hidden;border:3px solid rgba(255,255,255,.65);border-radius:25px;background:linear-gradient(to bottom,rgba(255,255,255,.55) 0 66%,rgba(55,183,190,.18) 66%);box-shadow:inset 0 0 50px rgba(255,255,255,.7)}
    .clinic:before{content:"AMNA CLINIC  ✦";position:absolute;top:8px;color:rgba(8,125,131,.24);font-size:clamp(16px,5vw,25px);font-weight:900;letter-spacing:3px}
    .cabinet{position:absolute;right:2%;top:21%;width:12%;max-width:65px;height:42%;border-radius:8px;background:#fff;box-shadow:inset 0 0 0 5px #9de4e7}.cabinet:after{content:"+";display:grid;place-items:center;margin:15px auto;width:28px;height:28px;border-radius:50%;background:var(--pink);color:#fff;font-weight:900;font-size:22px}
    .chair{position:absolute;left:50%;bottom:-9%;transform:translateX(-50%);width:min(73vw,360px);height:47%;border-radius:48% 48% 18% 18%;background:linear-gradient(90deg,#079ba5,#26c9c2 48%,#079ba5);box-shadow:0 12px 0 #08747e,0 18px 28px rgba(0,90,110,.22)}
    .chair:after{content:"";position:absolute;left:12%;right:12%;top:15%;height:28%;border-radius:45%;background:rgba(255,255,255,.16)}
    .patient-area{position:relative;z-index:2;width:min(82vw,400px);height:min(58vh,445px);max-height:95%;display:flex;flex-direction:column;align-items:center;justify-content:flex-end;padding-bottom:5%}
    .case-card{position:absolute;top:5%;z-index:5;display:flex;align-items:center;gap:9px;max-width:90%;padding:8px 14px;border-radius:17px;background:#fff;box-shadow:var(--shadow);font-size:14px;font-weight:900;text-align:center;animation:pop .35s cubic-bezier(.2,1.6,.4,1)}
    .case-icon{font-size:24px}.case-card small{display:block;color:#7895a0;font-size:10px;margin-top:1px}
    .patience{position:absolute;top:calc(5% + 58px);width:min(72%,260px);height:10px;padding:2px;border-radius:9px;background:#fff;box-shadow:0 2px 8px #85bdc5}
    .patience span{display:block;height:100%;width:100%;border-radius:7px;background:linear-gradient(90deg,#ff669c,#ffd861,#24d1aa);transform-origin:left;transition:transform .1s linear}
    .person{position:relative;width:230px;height:275px;transform-origin:50% 100%;animation:breathe 2.2s ease-in-out infinite}
    @keyframes breathe{50%{transform:scale(1.018) translateY(-2px)}}
    .hair{position:absolute;z-index:0;left:42px;top:10px;width:146px;height:138px;border-radius:52% 52% 35% 35%;background:#3d2a35;box-shadow:inset 12px 10px #5d3b48}
    .head{position:absolute;z-index:1;left:52px;top:24px;width:126px;height:158px;border-radius:46% 46% 50% 50%;background:var(--skin,#f1b88e);box-shadow:inset -8px -5px rgba(155,79,70,.1),0 6px 10px rgba(45,88,96,.18)}
    .ear{position:absolute;top:86px;width:24px;height:43px;border-radius:50%;background:var(--skin,#f1b88e)}.ear.left{left:39px}.ear.right{right:39px}
    .eyes{position:absolute;z-index:3;left:73px;top:78px;width:84px;display:flex;justify-content:space-between}.eye{width:16px;height:20px;border-radius:50%;background:#253b47;border:5px solid #fff;box-shadow:0 2px 0 #b57770;animation:blink 4s infinite}.eye:after{content:"";display:block;width:3px;height:3px;background:white;border-radius:50%}@keyframes blink{0%,45%,50%,100%{transform:scaleY(1)}48%{transform:scaleY(.08)}}
    .nose{position:absolute;z-index:3;left:108px;top:100px;width:14px;height:22px;border:3px solid rgba(137,72,65,.22);border-top:0;border-radius:0 0 10px 10px}
    .cheeks{position:absolute;z-index:2;left:61px;top:117px;width:108px;display:flex;justify-content:space-between}.cheeks i{width:25px;height:12px;border-radius:50%;background:rgba(255,98,133,.22)}
    .mouth{position:absolute;z-index:4;left:72px;top:127px;width:86px;height:62px;padding:6px;border:5px solid #bd5b65;border-radius:22px 22px 35px 35px;background:#672f43;box-shadow:inset 0 5px #462032;overflow:hidden;transition:.25s}
    .teeth{display:grid;grid-template-columns:repeat(4,1fr);gap:2px}.tooth{position:relative;height:22px;border-radius:4px 4px 8px 8px;background:#fffdf3;box-shadow:inset 0 -3px #dbeff0;transition:.3s}.tooth:nth-child(n+5){transform:rotate(180deg)}
    .tooth.cavity:after{content:"";position:absolute;width:8px;height:8px;border-radius:50%;background:#50322e;left:5px;top:6px;box-shadow:0 0 0 2px #8b5b3f}
    .mouth.yellow .tooth{background:#ffe27e;box-shadow:inset 0 -3px #e6b84f}.tooth.tartar:after{content:"";position:absolute;left:1px;right:1px;bottom:0;height:7px;background:#c79b51;border-radius:50% 50% 3px 3px}.tooth.broken{clip-path:polygon(0 0,100% 0,100% 100%,68% 77%,49% 100%,28% 74%,0 100%)}
    .pain-mark{position:absolute;z-index:7;right:22px;top:88px;color:#ff3979;font-size:36px;font-weight:900;transform:rotate(10deg);animation:ache .45s infinite alternate}@keyframes ache{to{transform:rotate(-8deg) scale(1.2)}}
    .body{position:absolute;z-index:0;left:24px;bottom:-10px;width:182px;height:116px;border-radius:50% 50% 18% 18%;background:var(--shirt,#ff80ab);box-shadow:inset -14px 0 rgba(121,31,83,.08)}
    .body:after{content:"";position:absolute;left:70px;top:0;border-left:21px solid transparent;border-right:21px solid transparent;border-top:37px solid #fff}
    .feedback{position:absolute;z-index:10;top:38%;font-size:32px;font-weight:900;color:#fff;text-shadow:0 3px 0 rgba(0,0,0,.18);pointer-events:none;opacity:0}.feedback.show{animation:feedback .75s ease-out}@keyframes feedback{0%{opacity:0;transform:scale(.4)}25%{opacity:1;transform:scale(1.2)}100%{opacity:0;transform:translateY(-60px)}}
    .person.happy .mouth{height:42px;border-radius:12px 12px 45px 45px}.person.happy{animation:celebrate .55s ease-in-out}@keyframes celebrate{50%{transform:translateY(-16px) rotate(3deg)}}
    .person.shake{animation:shake .35s}@keyframes shake{25%{transform:translateX(-12px) rotate(-3deg)}75%{transform:translateX(12px) rotate(3deg)}}

    /* Tools */
    .tray{z-index:5;padding:8px 7px 7px;border:3px solid white;border-radius:22px;background:linear-gradient(#f9ffff,#dff8fa);box-shadow:0 -8px 26px rgba(13,111,127,.14)}
    .tray-title{text-align:center;margin:-2px 0 5px;color:#5a8997;font-size:10px;font-weight:900;letter-spacing:1.5px;text-transform:uppercase}
    .tools{display:grid;grid-template-columns:repeat(5,1fr);gap:6px}
    .tool{position:relative;min-width:0;height:78px;padding:6px 2px 4px;border:2px solid #c3eef0;border-radius:16px;background:#fff;box-shadow:0 5px 0 #97d7db;transition:.12s;overflow:hidden}.tool:active{transform:translateY(4px);box-shadow:0 1px 0 #97d7db}.tool svg{display:block;width:37px;height:42px;margin:auto;overflow:visible}.tool span{display:block;font-size:10px;font-weight:900;white-space:nowrap;color:#386875}.tool.correct-glow{animation:toolGlow .6s}@keyframes toolGlow{50%{background:#a9ffe4;transform:scale(1.08)}}
    .metal{stroke:#4e8b9b;stroke-width:5;stroke-linecap:round;fill:none}.handle{stroke:#18bdb7;stroke-width:9;stroke-linecap:round}

    /* Overlays and small heights */
    .final-score{font-size:54px;font-weight:900;color:var(--deep);line-height:1}.final-label{text-transform:uppercase;letter-spacing:2px;font-size:11px;color:#74929c;font-weight:900}.results{display:flex;justify-content:center;gap:10px;margin:18px 0 26px}.result{padding:10px 14px;border-radius:15px;background:#e8fafb;font-weight:900}.result small{display:block;font-size:10px;color:#74929c}.stars{font-size:30px;color:#ffd34e;letter-spacing:4px;margin:6px 0 16px}
    .mute{position:absolute;right:12px;top:calc(12px + env(safe-area-inset-top));z-index:30;width:42px;height:42px;border:2px solid white;border-radius:50%;background:rgba(255,255,255,.8);box-shadow:0 4px 12px rgba(0,80,100,.18);font-size:19px}
    @media(max-height:690px){.game{gap:5px;padding-top:calc(5px + env(safe-area-inset-top));padding-bottom:calc(5px + env(safe-area-inset-bottom))}.clinic{border-radius:18px}.person{transform:scale(.83);margin-bottom:-24px}.person.happy{animation:celebrateSmall .55s}.case-card{top:3%;padding:5px 11px}.patience{top:calc(3% + 48px)}.tool{height:68px}.tool svg{height:34px}.tray{padding-top:5px}.tray-title{display:none}@keyframes celebrateSmall{50%{transform:scale(.83) translateY(-12px) rotate(3deg)}}}
    @media(min-width:700px) and (orientation:landscape){.game{max-width:900px;margin:auto}.clinic{min-height:330px}.tray{max-width:700px;width:100%;margin:auto}.person{transform:scale(.9);margin-bottom:-12px}}
    @media(prefers-reduced-motion:reduce){*,*:before,*:after{animation-duration:.01ms!important;animation-iteration-count:1!important}}
  </style>
</head>
<body>
<main class="app">
  <div class="bubbles" aria-hidden="true"><i class="bubble"></i><i class="bubble"></i><i class="bubble"></i><i class="bubble"></i><i class="bubble"></i></div><div class="lights"></div>
  <button class="mute" id="mute" aria-label="Toggle sound">♪</button>

  <section class="screen" id="titleScreen">
    <div class="card"><div class="logo" aria-hidden="true"></div><h1>Amna Clinic</h1><p class="slogan">Save the Smiles!</p><p class="intro">Pick the right dental tool, treat patients quickly, and build the happiest clinic in town.</p><button class="primary" id="startBtn">Open the Clinic</button><div class="sound-note">Sound starts when you tap play</div></div>
  </section>

  <section class="game inactive" id="game" aria-label="Amna Clinic game">
    <header class="hud">
      <div class="stat"><span class="label">Score</span><strong id="score">0</strong></div>
      <div class="stat timer" id="timerBox"><span class="label">Time</span><strong id="timer">60</strong></div>
      <div class="stat"><span class="label">Level</span><strong id="level">1</strong></div>
      <div class="stat"><span class="label">Reputation</span><strong id="rep">♥♥♥♥♥</strong></div>
    </header>
    <div class="clinic">
      <div class="cabinet" aria-hidden="true"></div><div class="chair" aria-hidden="true"></div>
      <div class="patient-area">
        <div class="case-card"><span class="case-icon" id="caseIcon">●</span><div><span id="caseName">Cavity</span><small id="caseHint">Choose the right tool</small></div></div>
        <div class="patience" aria-label="Patient patience"><span id="patienceFill"></span></div>
        <div class="feedback" id="feedback">Great!</div>
        <div class="person" id="person">
          <div class="hair"></div><div class="ear left"></div><div class="ear right"></div><div class="head"></div>
          <div class="eyes"><i class="eye"></i><i class="eye"></i></div><div class="nose"></div><div class="cheeks"><i></i><i></i></div>
          <div class="mouth" id="mouth"><div class="teeth" id="teeth"></div></div><div class="pain-mark" id="painMark" hidden>!</div><div class="body"></div>
        </div>
      </div>
    </div>
    <footer class="tray">
      <div class="tray-title">Dental Tool Tray</div>
      <div class="tools" id="tools">
        <button class="tool" data-tool="mirror" aria-label="Mirror"><svg viewBox="0 0 50 60"><circle class="metal" cx="20" cy="17" r="12" fill="#dffaff"/><path class="metal" d="M29 26L42 51"/><path class="handle" d="M38 44L44 56"/></svg><span>Mirror</span></button>
        <button class="tool" data-tool="scaler" aria-label="Scaler"><svg viewBox="0 0 50 60"><path class="metal" d="M10 7c0 7 7 7 10 9l20 35"/><path class="handle" d="M21 20L37 47"/></svg><span>Scaler</span></button>
        <button class="tool" data-tool="suction" aria-label="Suction"><svg viewBox="0 0 50 60"><path class="metal" d="M13 5v14c0 6 5 8 9 8h5"/><path class="handle" d="M27 27L41 48"/><path class="metal" d="M41 48l3 7"/></svg><span>Suction</span></button>
        <button class="tool" data-tool="filling" aria-label="Filling tool"><svg viewBox="0 0 50 60"><path class="metal" d="M25 4v11M25 47v9"/><path class="handle" d="M25 15V47"/><circle cx="25" cy="5" r="4" fill="#ff7bac"/></svg><span>Filling</span></button>
        <button class="tool" data-tool="whitening" aria-label="Whitening tool"><svg viewBox="0 0 50 60"><path class="handle" d="M25 23V53"/><rect x="12" y="7" width="26" height="19" rx="7" fill="#fff" stroke="#4e8b9b" stroke-width="4"/><path d="M20 11l-3 7m12-7l-3 7" stroke="#7ce9ff" stroke-width="3"/></svg><span>Whiten</span></button>
      </div>
    </footer>
  </section>

  <section class="screen hidden" id="gameOver">
    <div class="card"><div class="final-label">Clinic Closed</div><h1>Great Work!</h1><div class="stars" id="stars">★★★</div><div class="final-score" id="finalScore">0</div><div class="final-label">Final Score</div><div class="results"><div class="result"><span id="finalLevel">1</span><small>LEVEL</small></div><div class="result"><span id="finalPatients">0</span><small>SMILES</small></div><div class="result"><span id="bestCombo">0</span><small>BEST COMBO</small></div></div><button class="primary" id="restartBtn">Play Again</button></div>
  </section>
</main>

<script>
(() => {
  'use strict';
  const $ = id => document.getElementById(id);
  const problems = [
    {name:'Cavity', tool:'filling', icon:'●', hint:'A dark spot needs repair', className:'cavity'},
    {name:'Tartar', tool:'scaler', icon:'◆', hint:'Scrape away the buildup', className:'tartar'},
    {name:'Yellow Teeth', tool:'whitening', icon:'✦', hint:'Bring back the sparkle', className:'yellow'},
    {name:'Tooth Pain', tool:'mirror', icon:'!', hint:'Inspect the aching tooth', className:'pain'},
    {name:'Broken Tooth', tool:'suction', icon:'⚡', hint:'Clear the broken area', className:'broken'}
  ];
  const skins = ['#f1b88e','#8f593f','#d88d67','#f5c8a7','#704231'];
  const shirts = ['#ff80ab','#6b9dff','#9b79e8','#ffad62','#45caaa'];
  const names = ['Lina','Omar','Noor','Zaid','Maya','Sami','Rana','Adam'];
  const el = {title:$('titleScreen'),over:$('gameOver'),game:$('game'),score:$('score'),timer:$('timer'),timerBox:$('timerBox'),level:$('level'),rep:$('rep'),person:$('person'),mouth:$('mouth'),teeth:$('teeth'),pain:$('painMark'),caseName:$('caseName'),caseIcon:$('caseIcon'),caseHint:$('caseHint'),patience:$('patienceFill'),feedback:$('feedback')};
  let state, frame, audio, muted=false;

  function resetState(){state={playing:true,score:0,time:60,level:1,reputation:5,combo:0,bestCombo:0,patients:0,problem:null,patientStart:0,patientLimit:8000,lastTick:performance.now(),locked:false};}
  function buildTeeth(problem){
    el.teeth.innerHTML=''; el.mouth.className='mouth'; el.pain.hidden=true;
    for(let i=0;i<8;i++){const tooth=document.createElement('i');tooth.className='tooth';if((problem.className==='cavity'&&i===2)||(problem.className==='tartar'&&(i===4||i===5))||(problem.className==='broken'&&i===1)) tooth.classList.add(problem.className);el.teeth.appendChild(tooth);}
    if(problem.className==='yellow') el.mouth.classList.add('yellow');
    if(problem.className==='pain') el.pain.hidden=false;
  }
  function nextPatient(){
    if(!state.playing)return;
    state.locked=false; const previous=state.problem; let choices=problems.filter(p=>p!==previous); state.problem=choices[Math.floor(Math.random()*choices.length)];
    state.patientLimit=Math.max(3200,8200-(state.level-1)*650); state.patientStart=performance.now();
    el.caseName.textContent=state.problem.name+' · '+names[Math.floor(Math.random()*names.length)];el.caseIcon.textContent=state.problem.icon;el.caseHint.textContent=state.problem.hint;
    el.person.style.setProperty('--skin',skins[Math.floor(Math.random()*skins.length)]);el.person.style.setProperty('--shirt',shirts[Math.floor(Math.random()*shirts.length)]);el.person.className='person';buildTeeth(state.problem);el.patience.style.transform='scaleX(1)';
  }
  function updateHUD(){el.score.textContent=state.score.toLocaleString();el.timer.textContent=Math.ceil(state.time);el.level.textContent=state.level;el.rep.textContent='♥'.repeat(state.reputation)+'♡'.repeat(5-state.reputation);el.timerBox.classList.toggle('danger',state.time<=10);}
  function showFeedback(text,good){el.feedback.textContent=text;el.feedback.style.color=good?'#fff':'#ff3e7d';el.feedback.classList.remove('show');void el.feedback.offsetWidth;el.feedback.classList.add('show');}
  function treat(tool,button){
    if(!state.playing||state.locked)return;
    if(tool===state.problem.tool){
      state.locked=true;state.combo++;state.bestCombo=Math.max(state.bestCombo,state.combo);state.patients++;
      const speed=Math.max(0,1-(performance.now()-state.patientStart)/state.patientLimit);const gained=100+state.combo*20+Math.round(speed*100);state.score+=gained;state.reputation=Math.min(5,state.reputation+1);
      const newLevel=1+Math.floor(state.patients/5);if(newLevel>state.level){state.level=newLevel;state.time=Math.min(75,state.time+5);sound('level');showFeedback('LEVEL '+state.level+'!',true);}else showFeedback(state.combo>1?'COMBO x'+state.combo+'  +'+gained:'Perfect! +'+gained,true);
      button.classList.add('correct-glow');el.person.classList.add('happy');sound('correct');buildTeeth({className:'clean'});updateHUD();setTimeout(()=>{button.classList.remove('correct-glow');nextPatient();},650);
    }else{
      state.time=Math.max(0,state.time-4);state.combo=0;state.reputation=Math.max(0,state.reputation-1);el.person.classList.remove('shake');void el.person.offsetWidth;el.person.classList.add('shake');showFeedback('-4 seconds!',false);sound('wrong');updateHUD();
      if(state.reputation===0||state.time<=0)endGame();
    }
  }
  function missed(){if(state.locked||!state.playing)return;state.locked=true;state.combo=0;state.reputation=Math.max(0,state.reputation-1);showFeedback('Patient left!',false);sound('wrong');updateHUD();if(state.reputation===0)endGame();else setTimeout(nextPatient,550);}
  function loop(now){
    if(!state.playing)return;const dt=(now-state.lastTick)/1000;state.lastTick=now;state.time-=dt;
    const patience=Math.max(0,1-(now-state.patientStart)/state.patientLimit);el.patience.style.transform=`scaleX(${patience})`;updateHUD();
    if(state.time<=0){state.time=0;updateHUD();endGame();return;}if(patience<=0)missed();frame=requestAnimationFrame(loop);
  }
  function start(){
    initAudio();resetState();el.title.classList.add('hidden');el.over.classList.add('hidden');el.game.classList.remove('inactive');updateHUD();nextPatient();sound('start');cancelAnimationFrame(frame);frame=requestAnimationFrame(loop);
  }
  function endGame(){
    if(!state.playing)return;state.playing=false;cancelAnimationFrame(frame);el.game.classList.add('inactive');$('finalScore').textContent=state.score.toLocaleString();$('finalLevel').textContent=state.level;$('finalPatients').textContent=state.patients;$('bestCombo').textContent=state.bestCombo;$('stars').textContent='★'.repeat(state.score>=2500?3:state.score>=1000?2:1)+'☆'.repeat(state.score>=2500?0:state.score>=1000?1:2);setTimeout(()=>el.over.classList.remove('hidden'),300);sound('over');
  }

  // Tiny Web Audio melodies keep the game fully offline.
  function initAudio(){if(!audio) audio=new (window.AudioContext||window.webkitAudioContext)();if(audio.state==='suspended')audio.resume();}
  function tone(freq,at,duration,type='sine',volume=.08){if(!audio||muted)return;const o=audio.createOscillator(),g=audio.createGain();o.type=type;o.frequency.setValueAtTime(freq,audio.currentTime+at);g.gain.setValueAtTime(volume,audio.currentTime+at);g.gain.exponentialRampToValueAtTime(.001,audio.currentTime+at+duration);o.connect(g).connect(audio.destination);o.start(audio.currentTime+at);o.stop(audio.currentTime+at+duration);}
  function sound(type){if(muted)return;initAudio();if(type==='correct'){tone(520,0,.12,'sine');tone(700,.08,.13,'sine');tone(900,.17,.2,'sine');}else if(type==='wrong'){tone(190,0,.18,'sawtooth',.05);tone(135,.12,.25,'sawtooth',.04);}else if(type==='start'){[330,440,550,660].forEach((n,i)=>tone(n,i*.08,.18));}else if(type==='level'){[520,660,780,1040].forEach((n,i)=>tone(n,i*.07,.25));}else{tone(350,0,.3);tone(260,.22,.45);}}

  $('startBtn').addEventListener('click',start);$('restartBtn').addEventListener('click',start);
  document.querySelectorAll('.tool').forEach(btn=>btn.addEventListener('click',()=>treat(btn.dataset.tool,btn)));
  $('mute').addEventListener('click',()=>{muted=!muted;$('mute').textContent=muted?'×':'♪';$('mute').setAttribute('aria-label',muted?'Turn sound on':'Turn sound off');if(!muted){initAudio();tone(660,0,.1);}});
  document.addEventListener('visibilitychange',()=>{if(state&&state.playing){if(document.hidden){cancelAnimationFrame(frame);}else{state.lastTick=performance.now();frame=requestAnimationFrame(loop);}}});
})();
</script>
</body>
</html>
