 = """<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<meta name="theme-color" content="#090b10">
<title>最後一堂課 | The Last Lesson</title>
<style>
:root{--bg:#090b10;--panel:#151923;--line:#39404d;--ink:#e8e9e5;--muted:#a5a9b2;--gold:#e2c77b;--red:#bd5555;--stamina:#4caf50}
*{box-sizing:border-box}
html,body{margin:0;width:100%;height:100%;overflow:hidden;background:var(--bg);color:var(--ink);font-family:ui-monospace,Consolas,monospace;touch-action:none}
button{font:inherit;color:inherit;background:#202633;border:1px solid #596171;border-radius:8px;padding:10px 13px}
button:active{background:#394354;transform:translateY(1px)}
#game{display:block;width:100vw;height:100dvh;background:#080a0e;image-rendering:pixelated}
#hud{position:fixed;inset:0;pointer-events:none;display:flex;flex-direction:column;justify-content:space-between;padding:env(safe-area-inset-top) 12px env(safe-area-inset-bottom)}
.top{display:flex;justify-content:space-between;align-items:flex-start;gap:8px}
.panel{background:#090d14d9;border:1px solid #48505d;border-radius:8px;padding:8px 10px;box-shadow:0 3px 12px #0008}
#chapter{font-size:12px;color:var(--gold);line-height:1.5}
#status{font-size:11px;color:#d8dbe0;line-height:1.6;text-align:right}
.stamina-container{width:110px;height:8px;background:#1a202c;border:1px solid #4a5568;border-radius:4px;overflow:hidden;margin-top:4px;display:inline-block;vertical-align:middle}
#staminaBar{width:100%;height:100%;background:var(--stamina);transition:width 0.1s linear, background-color 0.2s}
#message{align-self:center;max-width:min(92vw,560px);min-height:36px;margin-bottom:132px;text-align:center;font-size:13px;line-height:1.5;background:#090d14e8;border:1px solid #596171;border-radius:8px;padding:9px 13px;opacity:0;transition:opacity .2s}
#message.show{opacity:1}
#controls{position:fixed;inset:auto 0 0;display:flex;justify-content:space-between;align-items:flex-end;padding:0 15px calc(12px + env(safe-area-inset-bottom));pointer-events:none}
#stickArea{width:142px;height:142px;position:relative;pointer-events:auto;touch-action:none}
#stickBase{position:absolute;inset:15px;border:2px solid #9ca4b366;border-radius:50%;background:#11172299}
#stick{position:absolute;width:48px;height:48px;left:47px;top:47px;border-radius:50%;background:#aab2c077;border:2px solid #d5d9df99}
.actions{display:grid;grid-template-columns:repeat(2,62px);gap:8px;pointer-events:auto}
.actions button{height:53px;padding:4px;font-size:11px;background:#111722dd}
#runBtn.exhausted{border-color:var(--red);color:var(--red);opacity:0.6}
#interactBtn{border-color:#b89b59;color:#f0d68b}
#desktopHint{position:fixed;bottom:8px;left:50%;transform:translateX(-50%);font-size:10px;color:#a5a9b2aa;pointer-events:none;white-space:nowrap}
#overlay{position:fixed;inset:0;background:linear-gradient(#070910e8,#080b12f2);display:flex;align-items:center;justify-content:center;padding:20px;z-index:5}
.card{width:min(92vw,520px);max-height:90dvh;overflow:auto;background:#121722;border:1px solid #535d6d;border-radius:14px;padding:22px;box-shadow:0 12px 50px #000b}
h1{font-size:clamp(25px,7vw,38px);letter-spacing:2px;margin:0 0 5px;color:#eee7d4}
.sub{color:#b9bdc5;font-size:12px;letter-spacing:2px;margin-bottom:22px}
.card p{font-size:13px;line-height:1.8;color:#d0d3d8}
.card .small{font-size:11px;color:#9da4af}
.row{display:flex;gap:9px;flex-wrap:wrap;margin-top:14px}
.primary{background:#695632;border-color:#c9a966;color:#fff4d6;flex:1}
#langBtn{margin-left:auto}
.hidden{display:none!important}
#rotatePrompt{display:none;position:fixed;z-index:10;inset:0;background:#090b10;color:#eee;align-items:center;justify-content:center;text-align:center;padding:28px}
#rotatePrompt .rotateCard{max-width:340px;border:1px solid #596171;background:#151923;border-radius:14px;padding:24px}
#rotatePrompt .phoneIcon{font-size:52px;display:block;margin-bottom:14px;animation:turnPhone 1.8s ease-in-out infinite}
@keyframes turnPhone{0%,20%{transform:rotate(0)}70%,100%{transform:rotate(90deg)}}
@media (orientation:portrait) and (max-width:700px){#rotatePrompt{display:flex}}
#noteModal{position:fixed;z-index:6;inset:0;background:#000b;display:none;align-items:center;justify-content:center;padding:20px}
#noteModal .card{max-width:420px}
@media(min-width:800px){#controls{padding-left:35px;padding-right:35px}#stickArea{width:155px;height:155px}#desktopHint{bottom:10px}}
</style>
</head>
<body>
<canvas id="game"></canvas>
<div id="hud">
  <div class="top">
    <div class="panel">
      <div id="chapter">第一章：放學後</div>
      <div id="objective" style="font-size:11px;margin-top:4px">找到出口鑰匙</div>
    </div>
    <div class="panel" id="status">
      手電筒 100%<br>
      體力：<div class="stamina-container"><div id="staminaBar"></div></div><br>
      鑰匙：0
    </div>
  </div>
  <div id="message"></div>
</div>
<div id="controls">
  <div id="stickArea"><div id="stickBase"></div><div id="stick"></div></div>
  <div class="actions">
    <button id="flashBtn">手電筒<br>LIGHT</button>
    <button id="runBtn">奔跑<br>RUN</button>
    <button id="interactBtn">互動<br>USE</button>
    <button id="pauseBtn">暫停<br>PAUSE</button>
  </div>
</div>
<div id="desktopHint">手機：左搖桿移動・畫面拖曳左右/上下視角 | 電腦：WASD 移動・Shift 跑步・↑↓/I/K 上下視角・E 互動</div>

<div id="overlay">
 <div class="card">
  <div style="display:flex;align-items:center;gap:8px"><div style="font-size:11px;color:#d7bd7d;letter-spacing:3px">PIXEL HORROR / 0.2</div><button id="langBtn">EN</button></div>
  <h1 id="title">最後一堂課</h1>
  <div class="sub" id="subtitle">THE LAST LESSON</div>
  <p id="intro">你在放學後的教室醒來。整棟學校空無一人，時鐘停在 21:13。<br>走廊的廣播響起：「請尚未完成今日課程的學生，前往三年二班。」<br><br>找到出口鑰匙，留意黑暗中的動靜。</p>
  <p class="small" id="howto">操作：左側搖桿移動，拖曳右側畫面可調整左右與上下視角。按「奔跑」消耗體力，耗盡後需持續走路 30 秒才能恢復。</p>
  <div class="row"><button class="primary" id="startBtn">開始遊戲</button></div>
  <p class="small" id="disclaimer">原型版本：新增全方位視角與體力恢復系統。</p>
 </div>
</div>
<div id="rotatePrompt"><div class="rotateCard"><span class="phoneIcon">📱</span><h2 id="rotateTitle">請將手機轉為橫向</h2><p id="rotateText">《最後一堂課》採用橫向畫面設計。請旋轉手機，並關閉螢幕旋轉鎖定。</p><div class="small">PLEASE ROTATE YOUR DEVICE</div></div></div>
<div id="noteModal"><div class="card"><div style="color:#e2c77b;font-size:11px;letter-spacing:2px" id="noteLabel">FOUND NOTE</div><h2 id="noteTitle">紙條</h2><p id="noteText"></p><div class="row"><button class="primary" id="closeNote">關閉</button></div></div></div>

<script>
(()=>{
const canvas=document.getElementById('game'),ctx=canvas.getContext('2d',{alpha:false});
const $=id=>document.getElementById(id);
let lang='zh',started=false,paused=false,won=false,dead=false;
const tr={
 zh:{chapter:'第一章：放學後',objective:'找到出口鑰匙',battery:'手電筒',key:'鑰匙',stamina:'體力',start:'開始遊戲',intro:'你在放學後的教室醒來。整棟學校空無一人，時鐘停在 21:13。<br>走廊的廣播響起：「請尚未完成今日課程的學生，前往三年二班。」<br><br>找到出口鑰匙，留意黑暗中的動靜。',how:'操作：左側搖桿移動，拖曳右側畫面轉向與上下仰俯。按「奔跑」耗體力，耗盡需走路 30 秒恢復。',hint:'手機：左搖桿移動・拖曳視角 | 電腦：WASD 移動・Shift 跑步・↑↓/IK 視角・E 互動・F 手電筒',found:'取得：',need:'門鎖住了。需要找到鑰匙。',door:'你用鑰匙打開了出口。冷風從門外吹來……',noteTitle:'泛黃的紙條',note:'「如果聽見廣播叫你的名字，不要回答。圖書館的燈還能用一會兒。鑰匙放在教職員室。」',note2:'值日紀錄',note2text:'21:13 之後，不要再去三年二班。今天沒有那個班級。',shadow:'遠處似乎有一道影子……',chaser:'腳步聲突然加快了！快離開走廊！',batteryFound:'找到一顆電池，手電筒電量恢復。',noBattery:'手電筒電量耗盡了。',locked:'目前無法開啟。',win:'第一章完成',winText:'你離開了教學大樓，但校門外的路牌上，寫著一個陌生的校名。',restart:'重新開始',pause:'已暫停',resume:'繼續遊戲',gameover:'你迷失在黑暗之中',gameoverText:'這只是第一個夜晚。再試一次，記得留意周圍的聲音。',monster:'有什麼東西在附近……',empty:'這裡沒有其他東西。',batteryLow:'電量偏低。',exhausted:'體力耗盡！需持續走路 30 秒恢復體力！',staminaRestored:'體力已完全恢復！',lang:'EN'
 },
 en:{chapter:'CHAPTER 1: AFTER SCHOOL',objective:'Find the exit key',battery:'FLASHLIGHT',key:'KEYS',stamina:'STAMINA',start:'START GAME',intro:'You wake up in your classroom after school. The building is empty. Every clock has stopped at 9:13 PM.<br>A voice echoes through the speakers: “Students who have not completed today’s lesson, report to Room 3-B.”<br><br>Find the exit key. Listen carefully in the dark.',how:'Controls: Left stick moves, drag right side to look around/up/down. Run uses stamina; 30s walking restores it if depleted.',hint:'MOBILE: left stick move, drag look | PC: WASD move, Shift run, Up/Down/I/K pitch, E use, F light',found:'Found: ',need:'The door is locked. Find the key.',door:'The exit unlocks. Cold air drifts in from outside…',noteTitle:'A Yellowed Note',note:'“If the announcement calls your name, do not answer. The library lights may work for a little longer. The key is in the staff room.”',note2:'Duty Log',note2text:'After 9:13 PM, do not go to Room 3-B. There is no such class today.',shadow:'A shape moves at the far end of the hall…',chaser:'The footsteps are getting faster! Leave the corridor!',batteryFound:'You found a battery. Flashlight recharged.',noBattery:'The flashlight battery is empty.',locked:'It will not open yet.',win:'CHAPTER COMPLETE',winText:'You leave the school building. But the sign outside shows a school name you have never seen.',restart:'PLAY AGAIN',pause:'PAUSED',resume:'RESUME',gameover:'LOST IN THE DARK',gameoverText:'This was only the first night. Try again, and listen carefully.',monster:'Something is nearby…',empty:'Nothing else here.',batteryLow:'Battery low.',exhausted:'Exhausted! Walk for 30s to fully recover!',staminaRestored:'Stamina fully restored!',lang:'中文'}
};
function T(k){return tr[lang][k]||k}
function setLang(){document.documentElement.lang=lang==='zh'?'zh-Hant':'en';$('rotateTitle').textContent=lang==='zh'?'請將手機轉為橫向':'PLEASE ROTATE YOUR PHONE';$('rotateText').textContent=lang==='zh'?'《最後一堂課》採用橫向畫面設計。請旋轉手機，並關閉螢幕旋轉鎖定。':'The Last Lesson is designed for landscape play. Rotate your phone and turn off rotation lock.';$('langBtn').textContent=T('lang');$('title').textContent=lang==='zh'?'最後一堂課':'THE LAST LESSON';$('subtitle').textContent=lang==='zh'?'THE LAST LESSON':'A PIXEL HORROR GAME';$('intro').innerHTML=T('intro');$('howto').textContent=T('how');$('startBtn').textContent=T('start');$('chapter').textContent=T('chapter');$('objective').textContent=T('objective');$('desktopHint').textContent=T('hint');$('flashBtn').innerHTML=lang==='zh'?'手電筒<br>LIGHT':'LIGHT';$('runBtn').innerHTML=lang==='zh'?'奔跑<br>RUN':'RUN';$('interactBtn').innerHTML=lang==='zh'?'互動<br>USE':'USE';$('pauseBtn').innerHTML=lang==='zh'?'暫停<br>PAUSE':'PAUSE';}
$('langBtn').onclick=()=>{lang=lang==='zh'?'en':'zh';setLang();};
setLang();
let W=320,H=200,scale=1;
function resize(){const dpr=Math.min(devicePixelRatio||1,1.5);canvas.width=Math.floor(innerWidth*dpr);canvas.height=Math.floor(innerHeight*dpr);canvas.style.width=innerWidth+'px';canvas.style.height=innerHeight+'px';W=canvas.width;H=canvas.height;ctx.imageSmoothingEnabled=false;}
addEventListener('resize',resize);resize();
const map=[
"1111111111111111",
"1000000000000001",
"1011101110111101",
"1000100010000001",
"1110101010110101",
"1000001010000101",
"1011101011100101",
"1000000000100001",
"1011110110101101",
"1000010000000001",
"1011011110110101",
"1000000000000001",
"1111111111111111"
];
const MW=map[0].length,MH=map.length;
let player,items,entities,keys={},battery,flash,hasKey,notes,enemyAlert,gameTime,lastTime=0;
let stamina=100,isExhausted=false,walkTimer=0;
const MAX_STAMINA=100;
const EXHAUST_RECOVERY_TIME=30;

const wall=(x,y)=>{const mx=Math.floor(x),my=Math.floor(y);return mx<0||my<0||mx>=MW||my>=MH||map[my][mx]==='1'};
function reset(){
 player={x:1.7,y:1.7,a:0.05,pitch:0,hp:100};battery=100;flash=true;hasKey=false;notes=0;enemyAlert=0;gameTime=0;won=false;dead=false;paused=false;
 stamina=100;isExhausted=false;walkTimer=0;
 updateStaminaUI();
 items=[
 {x:3.5,y:1.5,type:'note',taken:false},
 {x:7.5,y:3.5,type:'battery',taken:false},
 {x:12.5,y:5.5,type:'key',taken:false},
 {x:5.5,y:11.5,type:'note2',taken:false},
 {x:14.3,y:11.5,type:'exit',taken:false}
 ];
 entities=[
 {x:10.5,y:1.6,type:'shadow',active:false,speed:.48,phase:0},
 {x:3.5,y:10.5,type:'visitor',active:false,speed:.28,phase:0}
 ];
}
reset();
function msg(s,ms=2600){$('message').innerHTML=s;$('message').classList.add('show');clearTimeout(msg.timer);msg.timer=setTimeout(()=>$('message').classList.remove('show'),ms);}
function openOverlay(title,body,button){
 $('overlay').classList.remove('hidden');$('title').textContent=title;$('subtitle').textContent='THE LAST LESSON';$('intro').innerHTML=body;$('howto').textContent='';$('startBtn').textContent=button;$('disclaimer').textContent='';}
function endGame(win){
 won=win;dead=!win;started=false;
 openOverlay(win?T('win'):T('gameover'),win?T('winText'):T('gameoverText'),T('restart'));
}
async function start(){
 reset();started=true;$('overlay').classList.add('hidden');lastTime=performance.now();
 try{if(screen.orientation&&screen.orientation.lock){await screen.orientation.lock('landscape');}}catch(e){}
 msg(lang==='zh'?'你醒來了。教室門沒有上鎖。':'You wake up. The classroom door is unlocked.',3000);audioStart();
}
$('startBtn').onclick=()=>{if(!started)start();else start();};
$('pauseBtn').onclick=()=>{if(!started)return;paused=!paused;if(paused){$('pauseBtn').innerHTML=T('resume')+'<br>▶';msg(T('pause'),100000);}else{$('pauseBtn').innerHTML=lang==='zh'?'暫停<br>PAUSE':'PAUSE';$('message').classList.remove('show');lastTime=performance.now();}};
$('flashBtn').onclick=()=>{if(!started)return;flash=!flash;beep(520,.05);};

let running=false;
$('runBtn').addEventListener('touchstart',e=>{e.preventDefault();if(!isExhausted)running=true;},{passive:false});
$('runBtn').addEventListener('touchend',()=>running=false);
$('runBtn').addEventListener('mousedown',()=>if(!isExhausted)running=true);
$('runBtn').addEventListener('mouseup',()=>running=false);

function interact(){
 if(!started||paused)return;
 let best=null,bd=1.35;
 for(const it of items){if(it.taken)continue;const d=Math.hypot(it.x-player.x,it.y-player.y);if(d<bd){bd=d;best=it;}}
 if(!best){msg(T('empty'),1200);return;}
 if(best.type==='note'||best.type==='note2'){
  best.taken=true;notes++;$('noteTitle').textContent=best.type==='note'?T('noteTitle'):T('note2');$('noteText').textContent=best.type==='note'?T('note'):T('note2text');$('noteModal').style.display='flex';beep(740,.12);
 }else if(best.type==='battery'){
  best.taken=true;battery=Math.min(100,battery+45);msg(T('batteryFound'));beep(880,.1);
 }else if(best.type==='key'){
  best.taken=true;hasKey=true;msg(T('found')+(lang==='zh'?'出口鑰匙':'Exit key'));beep(980,.16);
 }else if(best.type==='exit'){
  if(hasKey){beep(660,.25);endGame(true);}else msg(T('need'));
 }
}
$('interactBtn').onclick=interact;$('closeNote').onclick=()=>$('noteModal').style.display='none';
$('noteModal').addEventListener('click',e=>{if(e.target===$('noteModal'))$('noteModal').style.display='none';});

let joy={x:0,y:0},joyActive=false,joyId=null,lookId=null,lastLookX=0,lastLookY=0;
const stickArea=$('stickArea'),stick=$('stick');
function setStick(x,y){const max=42,len=Math.hypot(x,y)||1;if(len>max){x=x/len*max;y=y/len*max;}joy.x=x/max;joy.y=y/max;stick.style.transform=`translate(${x}px,${y}px)`;}
stickArea.addEventListener('pointerdown',e=>{e.preventDefault();joyActive=true;joyId=e.pointerId;stickArea.setPointerCapture(e.pointerId);const r=stickArea.getBoundingClientRect();setStick(e.clientX-(r.left+r.width/2),e.clientY-(r.top+r.height/2));});
stickArea.addEventListener('pointermove',e=>{if(!joyActive||e.pointerId!==joyId)return;const r=stickArea.getBoundingClientRect();setStick(e.clientX-(r.left+r.width/2),e.clientY-(r.top+r.height/2));});
function joyEnd(e){if(e.pointerId!==joyId)return;joyActive=false;joyId=null;joy={x:0,y:0};stick.style.transform='translate(0,0)';}
stickArea.addEventListener('pointerup',joyEnd);stickArea.addEventListener('pointercancel',joyEnd);

canvas.addEventListener('pointerdown',e=>{
 lookId=e.pointerId;
 lastLookX=e.clientX;
 lastLookY=e.clientY;
});
canvas.addEventListener('pointermove',e=>{
 if(lookId!==e.pointerId||!started||paused)return;
 const dx=e.clientX-lastLookX;
 const dy=e.clientY-lastLookY;
 lastLookX=e.clientX;
 lastLookY=e.clientY;
 player.a+=dx*.006;
 player.pitch=Math.max(-1.0,Math.min(1.0,player.pitch - dy*.005));
});
canvas.addEventListener('pointerup',e=>{if(lookId===e.pointerId)lookId=null;});
canvas.addEventListener('pointercancel',e=>{if(lookId===e.pointerId)lookId=null;});

addEventListener('keydown',e=>{
 keys[e.key.toLowerCase()]=true;
 keys[e.key]=true;
 if(e.key.toLowerCase()==='e')interact();
 if(e.key.toLowerCase()==='f')$('flashBtn').click();
 if(e.key==='Escape')$('pauseBtn').click();
});
addEventListener('keyup',e=>{
 keys[e.key.toLowerCase()]=false;
 keys[e.key]=false;
});

function move(dx,dy){const r=.19;if(!wall(player.x+dx+r*Math.sign(dx),player.y))player.x+=dx;if(!wall(player.x,player.y+dy+r*Math.sign(dy)))player.y+=dy;}

function updateStaminaUI(){
 const sBar=$('staminaBar');
 if(sBar){
  const pct=Math.max(0,Math.min(100,stamina));
  sBar.style.width=pct+'%';
  if(isExhausted){
   sBar.style.backgroundColor='#bd5555';
   $('runBtn').classList.add('exhausted');
  }else if(pct<30){
   sBar.style.backgroundColor='#e2c77b';
   $('runBtn').classList.remove('exhausted');
  }else{
   sBar.style.backgroundColor='#4caf50';
   $('runBtn').addEventListener('mousedown',()=>{if(!isExhausted)running=true;});

function update(dt){
 if(!started||paused||won||dead)return;
 gameTime+=dt;
 let forward=joy.y*-1,side=joy.x;
 if(keys.w||keys.arrowup)forward=1;if(keys.s||keys.arrowdown)forward=-1;
 if(keys.a)side=-1;if(keys.d)side=1;
 if(keys.arrowleft)player.a-=dt*1.8;if(keys.arrowright)player.a+=dt*1.8;
 
 if(keys.i||keys.PageUp)player.pitch=Math.min(1.0,player.pitch+dt*1.5);
 if(keys.k||keys.PageDown)player.pitch=Math.max(-1.0,player.pitch-dt*1.5);

 const isMoving=(Math.abs(forward)>0.1||Math.abs(side)>0.1);
 let wantsToRun=(running||keys.shift||keys.Shift)&&isMoving;

 if(isExhausted){
  wantsToRun=false;
  if(isMoving){
   walkTimer+=dt;
   stamina=Math.min(MAX_STAMINA,(walkTimer/EXHAUST_RECOVERY_TIME)*MAX_STAMINA);
   if(walkTimer>=EXHAUST_RECOVERY_TIME){
    isExhausted=false;
    stamina=MAX_STAMINA;
    walkTimer=0;
    msg(T('staminaRestored'),2000);
    beep(800,.1);
   }
  }
 }else{
  if(wantsToRun){
   stamina-=dt*20;
   if(stamina<=0){
    stamina=0;
    isExhausted=true;
    walkTimer=0;
    running=false;
    msg(T('exhausted'),3000);
    beep(180,.3);
   }
  }else{
   if(stamina<MAX_STAMINA){
    stamina=Math.min(MAX_STAMINA,stamina+dt*15);
   }
  }
 }
 updateStaminaUI();

 const curSpeed=wantsToRun?0.95:0.66;
 const speed=curSpeed*dt;
 const dx=(Math.cos(player.a)*forward-Math.sin(player.a)*side)*speed;
 const dy=(Math.sin(player.a)*forward+Math.cos(player.a)*side)*speed;
 move(dx,dy);

 if(flash&&battery>0)battery=Math.max(0,battery-dt*1.05);
 if(battery<=0&&flash){flash=false;msg(T('noBattery'));}
 
 if(gameTime>12)entities[0].active=true;
 if(gameTime>34)entities[1].active=true;
 for(const en of entities){
  if(!en.active)continue;
  const d=Math.hypot(en.x-player.x,en.y-player.y);
  en.phase+=dt;
  if(d<7.2){
   const ang=Math.atan2(player.y-en.y,player.x-en.x);
   const sp=en.speed*(d<2.7?1.12:.72)*dt;
   const nx=en.x+Math.cos(ang)*sp,ny=en.y+Math.sin(ang)*sp;
   if(!wall(nx,en.y))en.x=nx;if(!wall(en.x,ny))en.y=ny;
  }else if(en.phase>3){en.phase=0;const a=Math.random()*Math.PI*2;const nx=en.x+Math.cos(a)*.35,ny=en.y+Math.sin(a)*.35;if(!wall(nx,ny)){en.x=nx;en.y=ny;}}
  if(d<.62){player.hp-=dt*30;if(player.hp<=0)endGame(false);}
 }
 if(gameTime>18&&Math.floor(gameTime)%19===0&&Math.floor(gameTime-dt)!==Math.floor(gameTime)){msg(T('shadow'),1800);beep(120,.25);}
 if(gameTime>38&&Math.floor(gameTime)%27===0&&Math.floor(gameTime-dt)!==Math.floor(gameTime)){msg(T('chaser'),1800);beep(95,.32);}
}

function castRay(angle){
 let sin=Math.sin(angle),cos=Math.cos(angle),dist=0,hit=0,side=0;
 for(let i=0;i<480;i++){dist+=.035;const x=player.x+cos*dist,y=player.y+sin*dist;if(wall(x,y)){hit=1;side=(Math.floor(x)!==Math.floor(x-cos*.035))?1:0;break;}}
 return {dist:Math.max(.03,dist),side,hit};
}

function render(){
 const w=W,h=H;
 const horizonShift = Math.floor(player.pitch * (h * 0.45));
 const horizon = Math.floor(h / 2) + horizonShift;

 ctx.fillStyle='#111722';ctx.fillRect(0,0,w,Math.max(0,horizon));
 ctx.fillStyle='#29251f';ctx.fillRect(Math.max(0,horizon),0,w,h-horizon);
 
 for(let y=0;y<h/2;y+=Math.max(2,Math.floor(h/70))){
  ctx.fillStyle=`rgba(0,0,0,${.12+y/(h/2)*.12})`;
  ctx.fillRect(0,Math.min(h,Math.max(0,horizonShift+y)),w,2);
 }

 const zbuf=new Float32Array(w);
 const fov=Math.PI/2.9;
 for(let x=0;x<w;x+=2){
  const cam=(x/w-.5)*fov,ray=castRay(player.a+cam),dist=ray.dist*Math.cos(cam);zbuf[x]=dist;zbuf[x+1]=dist;
  const wallH=Math.min(h*2.5,h/(dist*.88)),top=horizon-wallH/2;
  let shade=Math.max(18,155-dist*18);if(ray.side)shade*=.72;
  if(flash&&battery>0){const center=1-Math.abs(x/w-.5)*1.7;shade*=Math.max(.25,center);}
  const r=Math.floor(shade*.75),g=Math.floor(shade*.83),b=Math.floor(shade*.9);
  ctx.fillStyle=`rgb(${r},${g},${b})`;ctx.fillRect(x,top,2,wallH);
  if(wallH>16){ctx.fillStyle='rgba(0,0,0,.16)';for(let yy=top+8;yy<top+wallH;yy+=13)ctx.fillRect(x,yy,2,1);}
 }
 
 const sprites=[];
 for(const en of entities)if(en.active)sprites.push({x:en.x,y:en.y,type:en.type,d:Math.hypot(en.x-player.x,en.y-player.y)});
 for(const it of items)if(!it.taken)sprites.push({x:it.x,y:it.y,type:it.type,d:Math.hypot(it.x-player.x,it.y-player.y)});
 sprites.sort((a,b)=>b.d-a.d);
 for(const s of sprites){
  let a=Math.atan2(s.y-player.y,s.x-player.x)-player.a;while(a>Math.PI)a-=Math.PI*2;while(a< -Math.PI)a+=Math.PI*2;
  if(Math.abs(a)>fov*.62||s.d<.2)continue;
  const sx=(.5+a/fov)*w,sz=Math.min(h*.85,h/(s.d*.9)),sy=horizon-sz/2;
  if(zbuf[Math.max(0,Math.min(w-1,Math.floor(sx)))]<s.d*.9)continue;
  if(s.type==='shadow'||s.type==='visitor'){
   const col=s.type==='shadow'?'#08090d':'#302d38';
   ctx.fillStyle=col;ctx.fillRect(sx-sz*.16,sy+sz*.23,sz*.32,sz*.72);
   ctx.fillRect(sx-sz*.23,sy+sz*.13,sz*.46,sz*.27);
   ctx.fillStyle='#b6a7a0';ctx.fillRect(sx-sz*.11,sy+sz*.22,sz*.055,sz*.035);ctx.fillRect(sx+sz*.055,sy+sz*.22,sz*.055,sz*.035);
   ctx.fillStyle='#08090d';ctx.fillRect(sx-sz*.23,sy+sz*.88,sz*.16,sz*.12);ctx.fillRect(sx+sz*.07,sy+sz*.88,sz*.16,sz*.12);
  }else{
   const colors={key:'#e4c46e',battery:'#8acb83',note:'#ded9c4',note2:'#c9bba0',exit:'#b2c3d0'};
   ctx.fillStyle=colors[s.type]||'#ddd';ctx.fillRect(sx-sz*.09,sy+sz*.35,sz*.18,sz*.28);
   ctx.fillStyle='#17191e';ctx.fillRect(sx-sz*.035,sy+sz*.42,sz*.07,sz*.07);
  }
 }
 
 const grad=ctx.createRadialGradient(w/2,h/2,h*.12,w/2,h/2,h*.75);grad.addColorStop(0,'rgba(0,0,0,0)');grad.addColorStop(1,'rgba(0,0,0,.72)');ctx.fillStyle=grad;ctx.fillRect(0,0,w,h);
 if(battery<25&&flash){ctx.fillStyle='rgba(120,25,25,.08)';ctx.fillRect(0,0,w,h);}
 
 const reticleY = Math.max(10, Math.min(h-10, horizon));
 ctx.fillStyle='#d9d8cb99';ctx.fillRect(w/2-2,reticleY-1,4,2);
}

function hud(){
 const statusText = `${T('battery')} ${Math.ceil(battery)}%<br>${T('key')}：${hasKey?'1':'0'}<br>${lang==='zh'?'血量':'HP'} ${Math.max(0,Math.ceil(player.hp))}%`;
 const staminaHTML = `<br>${T('stamina')}：<div class="stamina-container"><div id="staminaBar"></div></div>` + (isExhausted ? ` <span style="color:#bd5555;font-size:9px">(${Math.ceil(30-walkTimer)}s)</span>` : '');
 $('status').innerHTML = `${T('battery')} ${Math.ceil(battery)}%${staminaHTML}<br>${T('key')}：${hasKey?'1':'0'}`;
 updateStaminaUI();
 $('objective').textContent=hasKey?(lang==='zh'?'前往出口':'Reach the exit'):T('objective');
}

let audioCtx=null,master=null,ambOsc=null,ambGain=null;
function audioStart(){
 try{
  if(!audioCtx){audioCtx=new (window.AudioContext||window.webkitAudioContext)();master=audioCtx.createGain();master.gain.value=.18;master.connect(audioCtx.destination);}
  if(audioCtx.state==='suspended')audioCtx.resume();
  if(!ambOsc){ambOsc=audioCtx.createOscillator();ambGain=audioCtx.createGain();ambOsc.type='sine';ambOsc.frequency.value=48;ambGain.gain.value=.035;ambOsc.connect(ambGain);ambGain.connect(master);ambOsc.start();}
 }catch(e){}
}
function beep(freq=220,dur=.15){try{audioStart();const o=audioCtx.createOscillator(),g=audioCtx.createGain();o.type='triangle';o.frequency.value=freq;g.gain.setValueAtTime(.0001,audioCtx.currentTime);g.gain.exponentialRampToValueAtTime(.18,audioCtx.currentTime+.015);g.gain.exponentialRampToValueAtTime(.0001,audioCtx.currentTime+dur);o.connect(g);g.connect(master);o.start();o.stop(audioCtx.currentTime+dur+.02);}catch(e){}}

function loop(now){const dt=Math.min(.05,(now-lastTime)/1000||0);lastTime=now;if(started){update(dt);render();hud();}else{render();}requestAnimationFrame(loop);}
requestAnimationFrame(loop);
})();
</script>
</body>
</html>"""

with open("the_last_lesson.html", "w", encoding="utf-8") as f:
    f.write(html_code)

print("HTML file generated successfully!")
# the-last-lesson
The Last Lesson - Pixel Horror Game
