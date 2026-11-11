<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<meta name="theme-color" content="#090b10">
<title>最後一堂課：校舍擴建版 | The Last Lesson Expanded</title>
<style>
:root{--bg:#090b10;--panel:#151923;--line:#39404d;--ink:#e8e9e5;--muted:#a5a9b2;--gold:#e2c77b;--gold-bright:#fff0bd;--red:#bd5555;--stamina:#4caf50}
*{box-sizing:border-box}
html,body{margin:0;width:100%;height:100%;overflow:hidden;background:var(--bg);color:var(--ink);font-family:ui-monospace,Consolas,monospace;touch-action:none;user-select:none;-webkit-user-select:none}
button{font:inherit;color:inherit;background:#202633;border:1px solid #596171;border-radius:8px;padding:10px 13px;cursor:pointer;transition:all 0.15s ease}
button:active{background:#394354;transform:translateY(1px)}

#game{display:block;width:100vw;height:100dvh;background:#080a0e;image-rendering:pixelated}

#hud{position:fixed;inset:0;pointer-events:none;display:flex;flex-direction:column;justify-content:space-between;padding:env(safe-area-inset-top) 12px env(safe-area-inset-bottom);z-index:2}
.top{display:flex;justify-content:space-between;align-items:flex-start;gap:8px}
.panel{background:#090d14d9;border:1px solid #48505d;border-radius:8px;padding:8px 10px;box-shadow:0 3px 12px #0008}
#chapter{font-size:12px;color:var(--gold);line-height:1.5;font-weight:bold}
#status{font-size:11px;color:#d8dbe0;line-height:1.6;text-align:right}
.stamina-container{width:110px;height:8px;background:#1a202c;border:1px solid #4a5568;border-radius:4px;overflow:hidden;margin-top:4px;display:inline-block;vertical-align:middle}
#staminaBar{width:100%;height:100%;background:var(--stamina);transition:width 0.1s linear, background-color 0.2s}
#message{align-self:center;max-width:min(92vw,560px);min-height:36px;margin-bottom:132px;text-align:center;font-size:13px;line-height:1.5;background:#090d14e8;border:1px solid #596171;border-radius:8px;padding:9px 13px;opacity:0;transition:opacity .2s}
#message.show{opacity:1}
#dangerAlert{position:fixed;top:65px;left:50%;transform:translateX(-50%);background:rgba(189,85,85,0.85);color:#fff;padding:6px 16px;border-radius:20px;font-size:12px;font-weight:bold;letter-spacing:1px;box-shadow:0 0 10px rgba(255,0,0,0.5);display:none;z-index:4;pointer-events:none;animation:pulse 1s infinite alternate}
@keyframes pulse{0%{opacity:0.7}100%{opacity:1}}

#controls{position:fixed;inset:auto 0 0;display:flex;justify-content:space-between;align-items:flex-end;padding:0 15px calc(12px + env(safe-area-inset-bottom));pointer-events:none;z-index:3}
#stickArea{width:142px;height:142px;position:relative;pointer-events:auto;touch-action:none}
#stickBase{position:absolute;inset:15px;border:2px solid #9ca4b366;border-radius:50%;background:#11172299}
#stick{position:absolute;width:48px;height:48px;left:47px;top:47px;border-radius:50%;background:#aab2c077;border:2px solid #d5d9df99}
.actions{display:grid;grid-template-columns:repeat(2,62px);gap:8px;pointer-events:auto}
.actions button{height:53px;padding:4px;font-size:11px;background:#111722dd}
#runBtn.exhausted{border-color:var(--red);color:var(--red);opacity:0.6}
#interactBtn{border-color:#b89b59;color:#f0d68b}
#desktopHint{position:fixed;bottom:8px;left:50%;transform:translateX(-50%);font-size:10px;color:#a5a9b2aa;pointer-events:none;white-space:nowrap;z-index:3}

/* 選單遮罩與主按鈕樣式 */
#overlay{position:fixed;inset:0;background:rgba(7, 9, 16, 0.94);display:flex;align-items:center;justify-content:center;padding:20px;z-index:10;backdrop-filter:blur(4px)}
.card{width:min(92vw,520px);max-height:90dvh;overflow:auto;background:#121722;border:1px solid #535d6d;border-radius:14px;padding:24px;box-shadow:0 12px 50px #000b;display:flex;flex-direction:column}
h1{font-size:clamp(24px,6vw,36px);letter-spacing:2px;margin:0 0 5px;color:#eee7d4}
.sub{color:#b9bdc5;font-size:12px;letter-spacing:2px;margin-bottom:18px}
.card p{font-size:13px;line-height:1.7;color:#d0d3d8;margin-top:0;margin-bottom:14px}
.card .small{font-size:11px;color:#9da4af}
.row{display:flex;gap:10px;margin-top:16px;width:100%}

/* 醒目的遊戲開始按鈕 */
.primary-btn{
  background: linear-gradient(135deg, #7a6338 0%, #4a3a1d 100%);
  border: 2px solid var(--gold);
  color: var(--gold-bright);
  font-size: 16px;
  font-weight: bold;
  letter-spacing: 2px;
  padding: 14px 20px;
  border-radius: 10px;
  width: 100%;
  box-shadow: 0 4px 15px rgba(226, 199, 123, 0.25);
  cursor: pointer;
}
.primary-btn:hover{
  background: linear-gradient(135deg, #947947 0%, #5c4925 100%);
  border-color: #fff0bd;
  box-shadow: 0 6px 20px rgba(226, 199, 123, 0.4);
}
.primary-btn:active{
  transform: translateY(2px);
}

.hidden{display:none!important}
#rotatePrompt{display:none;position:fixed;z-index:20;inset:0;background:#090b10;color:#eee;align-items:center;justify-content:center;text-align:center;padding:28px}
#rotatePrompt .rotateCard{max-width:340px;border:1px solid #596171;background:#151923;border-radius:14px;padding:24px}
#rotatePrompt .phoneIcon{font-size:52px;display:block;margin-bottom:14px;animation:turnPhone 1.8s ease-in-out infinite}
@keyframes turnPhone{0%,20%{transform:rotate(0)}70%,100%{transform:rotate(90deg)}}
@media (orientation:portrait) and (max-width:700px){#rotatePrompt{display:flex}}

#noteModal{position:fixed;z-index:15;inset:0;background:#000b;display:none;align-items:center;justify-content:center;padding:20px}
#noteModal .card{max-width:420px}
@media(min-width:800px){#controls{padding-left:35px;padding-right:35px}#stickArea{width:155px;height:155px}#desktopHint{bottom:10px}}
</style>
</head>
<body>
<canvas id="game"></canvas>

<!-- HUD 資訊介面 -->
<div id="hud">
  <div class="top">
    <div class="panel">
      <div id="chapter">第一章：擴建校舍</div>
      <div id="objective" style="font-size:11px;margin-top:4px">找到 5 把出口鑰匙 (0/5)</div>
    </div>
    <div class="panel" id="status">
      手電筒 100%<br>
      體力：<div class="stamina-container"><div id="staminaBar"></div></div><br>
      鑰匙：0/5
    </div>
  </div>
  <div id="dangerAlert">⚠️ 警告：巡邏異物接近中！距離：<span id="monsterDist">0</span>m</div>
  <div id="message"></div>
</div>

<!-- 控制動作按鈕 -->
<div id="controls">
  <div id="stickArea"><div id="stickBase"></div><div id="stick"></div></div>
  <div class="actions">
    <button id="flashBtn">手電筒<br>LIGHT</button>
    <button id="runBtn">奔跑<br>RUN</button>
    <button id="interactBtn">互動<br>USE</button>
    <button id="pauseBtn">暫停<br>PAUSE</button>
  </div>
</div>
<div id="desktopHint">手機：左搖桿移動・畫面拖曳視角 | 電腦：WASD 移動・Shift 跑步・↑↓/I/K 視角・E 互動・F 手電筒</div>

<!-- 遊戲開始與選單遮罩 -->
<div id="overlay">
 <div class="card">
  <div style="display:flex;align-items:center;justify-content:space-between;gap:8px;margin-bottom:8px">
    <div style="font-size:11px;color:#d7bd7d;letter-spacing:3px">PIXEL HORROR / 1.5 EXPANDED</div>
    <button id="langBtn" style="padding:4px 8px;font-size:11px">EN</button>
  </div>
  <h1 id="title">最後一堂課</h1>
  <div class="sub" id="subtitle">THE LAST LESSON: EXPANDED</div>
  <p id="intro">你在放學後的昏暗教學樓醒來。校舍地圖已擴建為 25x18 大尺寸區域，包含南北多棟教室與貫穿交錯的走廊！<br><br>請利用分支通道避開巡邏的異物，穿梭於教室區搜尋 5 把出口鑰匙並成功逃脫！</p>
  <p class="small" id="howto">操作說明：左側搖桿移動，畫面右側拖曳調整視角。點擊「互動」按鈕搜尋物品或開門！</p>
  <div class="row">
    <button class="primary-btn" id="startBtn">▶ 開始遊戲 START GAME</button>
  </div>
  <p class="small" id="disclaimer" style="margin-top:14px;margin-bottom:0">版本說明：點擊上方按鈕即可關閉選單並開始遊玩。</p>
 </div>
</div>

<div id="rotatePrompt"><div class="rotateCard"><span class="phoneIcon">📱</span><h2 id="rotateTitle">請將手機轉為橫向</h2><p id="rotateText">《最後一堂課》採用橫向畫面設計。請旋轉手機，並關閉螢幕旋轉鎖定。</p><div class="small">PLEASE ROTATE YOUR DEVICE</div></div></div>
<div id="noteModal"><div class="card"><div style="color:#e2c77b;font-size:11px;letter-spacing:2px" id="noteLabel">FOUND NOTE</div><h2 id="noteTitle">紙條</h2><p id="noteText"></p><div class="row"><button class="primary-btn" id="closeNote" style="padding:10px;font-size:14px">關閉</button></div></div></div>

<script>
(()=>{
const canvas=document.getElementById('game'),ctx=canvas.getContext('2d',{alpha:false});
const $=id=>document.getElementById(id);
let lang='zh',started=false,paused=false,won=false,dead=false;
const REQUIRED_KEYS=5;

const tr={
 zh:{
  chapter:'第一章：擴建校舍',objective:`找到 ${REQUIRED_KEYS} 把出口鑰匙`,battery:'手電筒',key:'鑰匙',stamina:'體力',
  start:'▶ 開始遊戲 START GAME',intro:'你在放學後的昏暗教學樓醒來。校舍地圖已擴建為 25x18 大尺寸區域，包含南北多棟教室與貫穿交錯的走廊！<br><br>請利用分支通道避開巡邏的異物，穿梭於教室區搜尋 5 把出口鑰匙並成功逃脫！',
  how:'操作說明：左側搖桿移動，畫面右側拖曳調整視角。點擊「互動」按鈕搜尋物品或開門！',
  hint:'手機：左搖桿移動・拖曳視角 | 電腦：WASD 移動・Shift 跑步・↑↓/IK 視角・E 互動・F 手電筒',
  found:'取得：',need:`藍色雙開門鎖住了。需要先找到全部 ${REQUIRED_KEYS} 把出口鑰匙才能打開！`,
  door:'你用 5 把鑰匙打開了藍色大門，雙門向兩側開啟！冷風吹來……',roomOpened:'你推開了教室木門！',
  noteTitle:'廣播室紀錄',note:'「校舍範圍已擴建，南北通道交錯。巡邏者活動範圍很大，善用分支小走廊是唯一生路。」',
  note2:'值日紀錄',note2text:'校舍擴建後有多達 12 間教室區域，集齊 5 把鑰匙才能打開東側藍色大門！',
  batteryFound:'找到一顆電池，手電筒電量恢復。',noBattery:'手電筒電量耗盡了。',locked:'目前無法開啟。',
  win:'第一章完成',winText:`你成功集齊了 ${REQUIRED_KEYS} 把鑰匙，順利甩開巡邏者並打開出口大門逃離教學大樓！`,
  gameover:'你被抓住了',gameoverText:'黑影近身碰到了你……在黑暗中失去了意識。',restart:'重新開始 PLAY AGAIN',
  pause:'已暫停',resume:'繼續遊戲',empty:'這裡沒有其他東西。',batteryLow:'電量偏低。',
  exhausted:'體力耗盡！需持續走路 30 秒恢復體力！',staminaRestored:'體力已完全恢復！',lang:'EN'
 },
 en:{
  chapter:'CHAPTER 1: EXPANDED CAMPUS',objective:`Find all ${REQUIRED_KEYS} exit keys`,battery:'FLASHLIGHT',key:'KEYS',stamina:'STAMINA',
  start:'▶ START GAME',intro:'You wake up in an expanded 25x18 school facility. Main corridors and side branch hallways are interconnected!',
  how:'Controls: Left stick moves, drag right side to look around. Running depletes stamina.',
  hint:'MOBILE: left stick move, drag look | PC: WASD move, Shift run, Up/Down pitch, E use, F light',
  found:'Found: ',need:`The exit door is locked. Find all ${REQUIRED_KEYS} keys first!`,
  door:'You unlocked the blue exit door with 5 keys!',roomOpened:'You pushed open the wooden door!',
  noteTitle:'Radio Room Note',note:'"The layout is expanded. The patroller roams a larger loop. Use side branches wisely."',
  note2:'Duty Log',note2text:'12 classroom blocks detected. Find all 5 keys to open the main eastern exit door.',
  batteryFound:'You found a battery. Flashlight recharged.',noBattery:'The flashlight battery is empty.',
  locked:'It will not open yet.',win:'CHAPTER COMPLETE',winText:'You collected all 5 keys and escaped the expanded campus!',
  gameover:'GAME OVER',gameoverText:'The shadow caught you in the dark...',restart:'PLAY AGAIN',
  pause:'PAUSED',resume:'RESUME',empty:'Nothing else here.',batteryLow:'Battery low.',
  exhausted:'Exhausted! Walk for 30s to recover!',staminaRestored:'Stamina fully restored!',lang:'中文'
 }
};

function T(k){return tr[lang][k]||k}
function setLang(){
  document.documentElement.lang=lang==='zh'?'zh-Hant':'en';
  $('rotateTitle').textContent=lang==='zh'?'請將手機轉為橫向':'PLEASE ROTATE YOUR PHONE';
  $('rotateText').textContent=lang==='zh'?'《最後一堂課》採用橫向畫面設計。請旋轉手機，並關閉螢幕旋轉鎖定。':'The Last Lesson is designed for landscape play. Rotate your phone.';
  $('langBtn').textContent=T('lang');
  $('title').textContent=lang==='zh'?'最後一堂課':'THE LAST LESSON';
  $('subtitle').textContent=lang==='zh'?'THE LAST LESSON: EXPANDED':'A PIXEL HORROR GAME';
  $('intro').innerHTML=T('intro');
  $('howto').textContent=T('how');
  $('startBtn').textContent=T('start');
  $('chapter').textContent=T('chapter');
  $('objective').textContent=T('objective');
  $('desktopHint').textContent=T('hint');
  $('flashBtn').innerHTML=lang==='zh'?'手電筒<br>LIGHT':'LIGHT';
  $('runBtn').innerHTML=lang==='zh'?'奔跑<br>RUN':'RUN';
  $('interactBtn').innerHTML=lang==='zh'?'互動<br>USE':'USE';
  $('pauseBtn').innerHTML=lang==='zh'?'暫停<br>PAUSE':'PAUSE';
}
$('langBtn').onclick=()=>{lang=lang==='zh'?'en':'zh';setLang();};
setLang();

let W=320,H=200;
function resize(){
  const dpr=Math.min(devicePixelRatio||1,1.5);
  canvas.width=Math.floor(innerWidth*dpr);
  canvas.height=Math.floor(innerHeight*dpr);
  canvas.style.width=innerWidth+'px';
  canvas.style.height=innerHeight+'px';
  W=canvas.width;H=canvas.height;
  ctx.imageSmoothingEnabled=false;
}
addEventListener('resize',resize);resize();

// 25x18 擴建版迷宮地形 (1:牆壁, 0:走廊空地, 2:木門, 3:出口大藍門, 4:窗戶, 5:柱子裝飾)
const mapGrid=[
"1414111414111414111414111",
"1000010000010000010000001",
"1000000000000000000000001",
"1121510015111215100115111",
"1000000000000000000000001",
"1000000000000000000000001",
"1121510015111215100113111",
"1000000000000000000000001",
"1000010000010000010000001",
"1121510015111215100115111",
"1000000000000000000000001",
"1000000000000000000000001",
"1121510015111215100115111",
"1000000000000000000000001",
"1000000000000000000000001",
"1121510015111215100115111",
"1000010000010000010000001",
"1414111414111414111414111"
];
let map = [];
function initMap(){ map = mapGrid.map(row => row.split('')); }

const MW=mapGrid[0].length,MH=mapGrid.length;
let player,items,furniture=[],monster,keys={},battery,flash,keyCount=0,notes,gameTime,lastTime=0;
let stamina=100,isExhausted=false,walkTimer=0;
let doorOpened=false;
const MAX_STAMINA=100;
const EXHAUST_RECOVERY_TIME=30;

const wall=(x,y)=>{
  const mx=Math.floor(x),my=Math.floor(y);
  if(mx<0||my<0||mx>=MW||my>=MH) return true;
  const tile=map[my][mx];
  if(tile==='1'||tile==='4'||tile==='5') return true;
  if(tile==='2') return true;
  if(tile==='3') return !doorOpened;
  return false;
};

const monsterWaypoints = [
  {x: 1.5, y: 4.5},
  {x: 11.5, y: 4.5},
  {x: 11.5, y: 10.5},
  {x: 18.5, y: 10.5},
  {x: 18.5, y: 16.5},
  {x: 6.5, y: 16.5},
  {x: 6.5, y: 7.5},
  {x: 22.5, y: 7.5},
  {x: 22.5, y: 2.5},
  {x: 1.5, y: 2.5}
];

function getRandomEmptySpots(count=1) {
  const candidates = [];
  for(let y=0; y<MH; y++){
    for(let x=0; x<MW; x++){
      if(mapGrid[y][x]==='0' && (y===1 || y===8 || y===14 || y===16)){
        const pos = {x: x+0.5, y: y+0.5};
        if(Math.hypot(pos.x - 1.7, pos.y - 4.5) > 3.0) {
          candidates.push(pos);
        }
      }
    }
  }
  const result = [];
  for(let i=0; i<count; i++){
    if(candidates.length===0) break;
    const idx = Math.floor(Math.random()*candidates.length);
    result.push(candidates.splice(idx, 1)[0]);
  }
  return result;
}

function initClassroomFurniture(){
  furniture = [];
  const roomXRanges = [
    {minX:1, maxX:4, yTop:1, yBot:2},
    {minX:7, maxX:10, yTop:1, yBot:2},
    {minX:12, maxX:15, yTop:1, yBot:2},
    {minX:20, maxX:23, yTop:1, yBot:2},
    {minX:1, maxX:4, yTop:7, yBot:8},
    {minX:7, maxX:10, yTop:7, yBot:8},
    {minX:12, maxX:15, yTop:7, yBot:8},
    {minX:20, maxX:23, yTop:7, yBot:8},
    {minX:1, maxX:4, yTop:13, yBot:14},
    {minX:7, maxX:10, yTop:13, yBot:14},
    {minX:12, maxX:15, yTop:13, yBot:14},
    {minX:20, maxX:23, yTop:16, yBot:17}
  ];

  roomXRanges.forEach(rm => {
    furniture.push({x: rm.minX + 0.8, y: rm.yTop + 0.4, type: 'teacher_desk'});
    for(let r=0; r<2; r++){
      for(let c=0; c<2; c++){
        const fx = rm.minX + 0.6 + c * 0.9;
        const fy = rm.yTop + 0.4 + r * 0.5;
        furniture.push({
          x: fx, y: fy,
          type: Math.random() > 0.25 ? 'desk' : 'chair_overturned'
        });
      }
    }
  });

  furniture.push({x: 5.5, y: 2.1, type: 'locker'});
  furniture.push({x: 17.5, y: 7.9, type: 'locker'});
  furniture.push({x: 11.5, y: 14.1, type: 'locker'});
}

function reset(){
 initMap();
 player={x:1.7,y:4.5,a:0.0,pitch:0,hp:100};
 monster={x:18.5, y:10.5, targetIdx:0, speed: 0.48, state:'patrol'};
 battery=100;flash=true;keyCount=0;notes=0;gameTime=0;won=false;dead=false;paused=false;
 stamina=100;isExhausted=false;walkTimer=0;doorOpened=false;
 updateStaminaUI();
 initClassroomFurniture();

 const spots = getRandomEmptySpots(10);
 items=[
  {x:3.5,y:8.5,type:'note',taken:false},
  {x:6.5,y:2.5,type:'note2',taken:false},
  {x:spots[0].x, y:spots[0].y, type:'key', taken:false},
  {x:spots[1].x, y:spots[1].y, type:'key', taken:false},
  {x:spots[2].x, y:spots[2].y, type:'key', taken:false},
  {x:spots[3].x, y:spots[3].y, type:'key', taken:false},
  {x:spots[4].x, y:spots[4].y, type:'key', taken:false},
  {x:spots[5].x, y:spots[5].y, type:'battery', taken:false},
  {x:spots[6].x, y:spots[6].y, type:'battery', taken:false},
  {x:spots[7].x, y:spots[7].y, type:'battery', taken:false}
 ];
}
reset();

function msg(s,ms=2600){
  $('message').innerHTML=s;
  $('message').classList.add('show');
  clearTimeout(msg.timer);
  msg.timer=setTimeout(()=>$('message').classList.remove('show'),ms);
}

function openOverlay(title,body,buttonText){
  $('overlay').classList.remove('hidden');
  $('title').textContent=title;
  $('subtitle').textContent='THE LAST LESSON: EXPANDED';
  $('intro').innerHTML=body;
  $('howto').textContent='';
  $('startBtn').textContent=buttonText;
  $('disclaimer').textContent='';
}

function endGame(win){
  won=win;dead=!win;started=false;
  openOverlay(win?T('win'):T('gameover'),win?T('winText'):T('gameoverText'),T('restart'));
}

async function startGame(){
  reset();
  started=true;
  $('overlay').classList.add('hidden');
  lastTime=performance.now();
  try{if(screen.orientation&&screen.orientation.lock){await screen.orientation.lock('landscape');}}catch(e){}
  msg(lang==='zh'?`校舍已擴建！搜尋廣大的教室區域收集 ${REQUIRED_KEYS} 把出口鑰匙！`:`Expanded campus! Find all ${REQUIRED_KEYS} keys to escape!`,3500);
  audioStart();
}

// 開始遊戲按鈕綁定
$('startBtn').addEventListener('click', ()=>{
  startGame();
});

$('pauseBtn').onclick=()=>{
  if(!started)return;
  paused=!paused;
  if(paused){
    $('pauseBtn').innerHTML=T('resume')+'<br>▶';
    msg(T('pause'),100000);
  }else{
    $('pauseBtn').innerHTML=lang==='zh'?'暫停<br>PAUSE':'PAUSE';
    $('message').classList.remove('show');
    lastTime=performance.now();
  }
};

$('flashBtn').onclick=()=>{if(!started)return;flash=!flash;beep(520,.05);};

let running=false;
$('runBtn').addEventListener('touchstart',e=>{e.preventDefault();if(!isExhausted)running=true;},{passive:false});
$('runBtn').addEventListener('touchend',()=>running=false);
$('runBtn').addEventListener('mousedown',()=>{if(!isExhausted)running=true;});
$('runBtn').addEventListener('mouseup',()=>running=false);

function interact(){
 if(!started||paused)return;
 
 for(let dy=-1; dy<=1; dy++){
  for(let dx=-1; dx<=1; dx++){
   const checkX = Math.floor(player.x + dx);
   const checkY = Math.floor(player.y + dy);
   if(checkX>=0 && checkY>=0 && checkX<MW && checkY<MH){
    if(map[checkY][checkX] === '2'){
     map[checkY][checkX] = '0';
     beep(580, .15);
     msg(T('roomOpened'), 2000);
     return;
    }
   }
  }
 }

 const distToDoor = Math.hypot(player.x - 21.5, player.y - 6.5);
 if(distToDoor < 1.8){
  if(keyCount >= REQUIRED_KEYS){
   if(!doorOpened){
    doorOpened = true;
    beep(660,.25);
    msg(T('door'), 3000);
    setTimeout(()=> endGame(true), 2000);
   }
  } else {
   msg(lang==='zh'?`藍色出口大門鎖住了。需要集齊 ${REQUIRED_KEYS} 把鑰匙 (目前 ${keyCount}/${REQUIRED_KEYS})！`:`Exit door locked! (${keyCount}/${REQUIRED_KEYS})`, 2500);
   beep(300,.1);
  }
  return;
 }

 let best=null,bd=1.35;
 for(const it of items){if(it.taken)continue;const d=Math.hypot(it.x-player.x,it.y-player.y);if(d<bd){bd=d;best=it;}}
 if(!best){msg(T('empty'),1200);return;}
 if(best.type==='note'||best.type==='note2'){
  best.taken=true;notes++;$('noteTitle').textContent=best.type==='note'?T('noteTitle'):T('note2');$('noteText').textContent=best.type==='note'?T('note'):T('note2text');$('noteModal').style.display='flex';beep(740,.12);
 }else if(best.type==='battery'){
  best.taken=true;battery=Math.min(100,battery+45);msg(T('batteryFound'));beep(880,.1);
 }else if(best.type==='key'){
  best.taken=true;keyCount++;
  if(keyCount>=REQUIRED_KEYS){
   msg(lang==='zh'?`集齊全部 ${REQUIRED_KEYS} 把出口鑰匙！前往東側牆上的藍色大門逃生！`:`Collected all ${REQUIRED_KEYS} keys! Head to the blue door on the east wall!`);
  }else{
   msg(lang==='zh'?`取得出口鑰匙！進度 (${keyCount}/${REQUIRED_KEYS})`:`Found an Exit Key! (${keyCount}/${REQUIRED_KEYS})`);
  }
  beep(980,.16);
 }
}

$('interactBtn').onclick=interact;
$('closeNote').onclick=()=>$('noteModal').style.display='none';
$('noteModal').addEventListener('click',e=>{if(e.target===$('noteModal'))$('noteModal').style.display='none';});

// 搖桿與螢幕觸控轉向
let joy={x:0,y:0},joyActive=false,joyId=null,lookId=null,lastLookX=0,lastLookY=0;
const stickArea=$('stickArea'),stick=$('stick');
function setStick(x,y){const max=42,len=Math.hypot(x,y)||1;if(len>max){x=x/len*max;y=y/len*max;}joy.x=x/max;joy.y=y/max;stick.style.transform=`translate(${x}px,${y}px)`;}
stickArea.addEventListener('pointerdown',e=>{e.preventDefault();joyActive=true;joyId=e.pointerId;stickArea.setPointerCapture(e.pointerId);const r=stickArea.getBoundingClientRect();setStick(e.clientX-(r.left+r.width/2),e.clientY-(r.top+r.height/2));});
stickArea.addEventListener('pointermove',e=>{if(!joyActive||e.pointerId!==joyId)return;const r=stickArea.getBoundingClientRect();setStick(e.clientX-(r.left+r.width/2),e.clientY-(r.top+r.height/2));});
function joyEnd(e){if(e.pointerId!==joyId)return;joyActive=false;joyId=null;joy={x:0,y:0};stick.style.transform='translate(0,0)';}
stickArea.addEventListener('pointerup',joyEnd);stickArea.addEventListener('pointercancel',joyEnd);

canvas.addEventListener('pointerdown',e=>{lookId=e.pointerId;lastLookX=e.clientX;lastLookY=e.clientY;});
canvas.addEventListener('pointermove',e=>{
 if(lookId!==e.pointerId||!started||paused)return;
 const dx=e.clientX-lastLookX;const dy=e.clientY-lastLookY;lastLookX=e.clientX;lastLookY=e.clientY;
 player.a+=dx*.006;player.pitch=Math.max(-1.0,Math.min(1.0,player.pitch - dy*.005));
});
canvas.addEventListener('pointerup',e=>{if(lookId===e.pointerId)lookId=null;});
canvas.addEventListener('pointercancel',e=>{if(lookId===e.pointerId)lookId=null;});

// 電腦鍵盤控制
addEventListener('keydown',e=>{
 keys[e.key.toLowerCase()]=true;keys[e.key]=true;
 if(e.key.toLowerCase()==='e')interact();
 if(e.key.toLowerCase()==='f')$('flashBtn').click();
 if(e.key==='Escape')$('pauseBtn').click();
});
addEventListener('keyup',e=>{keys[e.key.toLowerCase()]=false;keys[e.key]=false;});

function move(dx,dy){
  const r=.19;
  if(!wall(player.x+dx+r*Math.sign(dx),player.y))player.x+=dx;
  if(!wall(player.x,player.y+dy+r*Math.sign(dy)))player.y+=dy;
}

function updateStaminaUI(){
 const sBar=$('staminaBar');
 if(sBar){
  const pct=Math.max(0,Math.min(100,stamina));
  sBar.style.width=pct+'%';
  if(isExhausted){sBar.style.backgroundColor='#bd5555';$('runBtn').classList.add('exhausted');}
  else if(pct<30){sBar.style.backgroundColor='#e2c77b';$('runBtn').classList.remove('exhausted');}
  else{sBar.style.backgroundColor='#4caf50';$('runBtn').classList.remove('exhausted');}
 }
}

function updateMonster(dt){
 const mDist = Math.hypot(monster.x - player.x, monster.y - player.y);
 const alertEl = $('dangerAlert');
 if(mDist < 7.5){alertEl.style.display = 'block';$('monsterDist').textContent = mDist.toFixed(1);}
 else {alertEl.style.display = 'none';}

 if(mDist < 0.35){endGame(false);return;}

 if(mDist < 3.8 && flash && battery > 0){monster.state = 'chase';}
 else if(mDist > 6.0){monster.state = 'patrol';}

 let target = monsterWaypoints[monster.targetIdx];
 if(monster.state === 'chase'){target = {x: player.x, y: player.y};}

 const dx = target.x - monster.x, dy = target.y - monster.y;
 const dist = Math.hypot(dx, dy);

 if(dist < 0.35 && monster.state === 'patrol'){
  monster.targetIdx = (monster.targetIdx + 1) % monsterWaypoints.length;
 } else if(dist > 0.05) {
  const speed = (monster.state === 'chase' ? 0.72 : 0.48) * dt;
  monster.x += (dx / dist) * speed;
  monster.y += (dy / dist) * speed;
 }
}

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
   walkTimer+=dt;stamina=Math.min(MAX_STAMINA,(walkTimer/EXHAUST_RECOVERY_TIME)*MAX_STAMINA);
   if(walkTimer>=EXHAUST_RECOVERY_TIME){isExhausted=false;stamina=MAX_STAMINA;walkTimer=0;msg(T('staminaRestored'),2000);beep(800,.1);}
  }
 }else{
  if(wantsToRun){
   stamina-=dt*20;
   if(stamina<=0){stamina=0;isExhausted=true;walkTimer=0;running=false;msg(T('exhausted'),3000);beep(180,.3);}
  }else if(stamina<MAX_STAMINA){stamina=Math.min(MAX_STAMINA,stamina+dt*15);}
 }
 updateStaminaUI();

 const curSpeed=wantsToRun?0.95:0.66;
 const speed=curSpeed*dt;
 move((Math.cos(player.a)*forward-Math.sin(player.a)*side)*speed,(Math.sin(player.a)*forward+Math.cos(player.a)*side)*speed);
 updateMonster(dt);

 if(flash&&battery>0)battery=Math.max(0,battery-dt*1.05);
 if(battery<=0&&flash){flash=false;msg(T('noBattery'));}
}

function castRay(angle){
 let sin=Math.sin(angle),cos=Math.cos(angle),dist=0,hit=0,side=0,tileType='1';
 for(let i=0;i<520;i++){
   dist+=.035;
   const x=player.x+cos*dist,y=player.y+sin*dist;
   const mx=Math.floor(x),my=Math.floor(y);
   if(wall(x,y)){
     hit=1;side=(Math.floor(x)!==Math.floor(x-cos*.035))?1:0;
     if(mx>=0&&my>=0&&mx<MW&&my<MH) tileType=map[my][mx];
     break;
   }
 }
 return {dist:Math.max(.03,dist),side,hit,tileType};
}

function renderFloorBeams(w, h, horizon, fov) {
 let target = null;
 if(keyCount < REQUIRED_KEYS) { target = items.find(i=>!i.taken && i.type==='key'); }
 else { target = {x: 21.5, y: 6.5}; }
 
 if(target) {
  const dx = target.x - player.x, dy = target.y - player.y;
  const dist = Math.hypot(dx, dy);
  for(let step = 0.6; step < dist; step += 0.7) {
   const gx = player.x + (dx/dist)*step, gy = player.y + (dy/dist)*step;
   if(wall(gx, gy)) break;
   let a = Math.atan2(gy - player.y, gx - player.x) - player.a;
   while(a > Math.PI) a -= Math.PI*2; while(a < -Math.PI) a += Math.PI*2;
   if(Math.abs(a) < fov*0.55) {
    const sx = (0.5 + a/fov) * w, pointDist = step * Math.cos(a);
    const sy = horizon + (h * 0.5) / pointDist;
    const pulse = (Math.sin(gameTime*4 + step)*0.3 + 0.7);
    const size = Math.max(3, Math.min(18, 14 / pointDist));
    ctx.fillStyle = 'rgba(255, 215, 0, ' + (0.6 * pulse) + ')';
    ctx.beginPath(); ctx.arc(sx, sy, size, 0, Math.PI*2); ctx.fill();
   }
  }
 }
}

function render(){
 const w=W,h=H;
 const horizonShift = Math.floor(player.pitch * (h * 0.45));
 const horizon = Math.floor(h / 2) + horizonShift;

 ctx.fillStyle='#07110d';ctx.fillRect(0,0,w,Math.max(0,horizon));
 ctx.fillStyle='#171d18';ctx.fillRect(Math.max(0,horizon),0,w,h-horizon);

 const zbuf=new Float32Array(w);
 const fov=Math.PI/2.9;
 for(let x=0;x<w;x+=2){
  const cam=(x/w-.5)*fov,ray=castRay(player.a+cam),dist=ray.dist*Math.cos(cam);zbuf[x]=dist;zbuf[x+1]=dist;
  const wallH=Math.min(h*2.5,h/(dist*.88)),top=horizon-wallH/2;
  let shade=Math.max(25,150-dist*15);
  if(ray.side)shade*=.72;
  if(flash && battery>0){shade*=Math.max(.4, (1-Math.abs(x/w-.5)*1.7)*1.6);}
  else if(battery<=0) {shade *= 0.45;}
  
  let r=Math.floor(shade*0.45), g=Math.floor(shade*0.75), b=Math.floor(shade*0.65);
  if(ray.tileType==='2'){r=Math.floor(shade*0.6); g=Math.floor(shade*0.4); b=Math.floor(shade*0.25);}
  else if(ray.tileType==='3'){r=Math.floor(shade*0.1); g=Math.floor(shade*0.45); b=Math.floor(shade*0.9);}
  
  ctx.fillStyle=`rgb(${r},${g},${b})`;ctx.fillRect(x,top,2,wallH);
 }

 renderFloorBeams(w, h, horizon, fov);

 const sprites=[];
 for(const it of items)if(!it.taken)sprites.push({x:it.x,y:it.y,type:it.type,d:Math.hypot(it.x-player.x,it.y-player.y)});
 for(const fn of furniture)sprites.push({x:fn.x,y:fn.y,type:fn.type,d:Math.hypot(fn.x-player.x,fn.y-player.y)});
 if(monster) sprites.push({x:monster.x, y:monster.y, type:'monster', d:Math.hypot(monster.x-player.x, monster.y-player.y)});
 sprites.sort((a,b)=>b.d-a.d);

 for(const s of sprites){
  let a=Math.atan2(s.y-player.y,s.x-player.x)-player.a;while(a>Math.PI)a-=Math.PI*2;while(a< -Math.PI)a+=Math.PI*2;
  if(Math.abs(a)>fov*.65||s.d<.2)continue;
  const spriteDist = s.d * Math.cos(a);
  const sz = Math.min(h * 2.2, h / (spriteDist * 0.88));
  const sx = (.5 + a/fov) * w, groundY = horizon + (h * 0.44) / spriteDist;
  if(zbuf[Math.max(0,Math.min(w-1,Math.floor(sx)))] < spriteDist * 0.95) continue;

  if(s.type==='monster'){
   const mH = sz * 0.75, mY = groundY - mH;
   ctx.fillStyle='#0a0e14';
   ctx.beginPath(); ctx.arc(sx, mY + mH*0.25, sz*0.14, 0, Math.PI*2); ctx.fill();
   ctx.fillRect(sx - sz*0.12, mY + mH*0.25, sz*0.24, mH*0.7);
   ctx.fillStyle='rgba(255,40,40,0.9)';
   ctx.fillRect(sx - sz*0.06, mY + mH*0.22, sz*0.03, sz*0.03);
   ctx.fillRect(sx + sz*0.03, mY + mH*0.22, sz*0.03, sz*0.03);
  } else if(s.type==='key'){
   ctx.fillStyle='#f5d76e';
   ctx.beginPath(); ctx.arc(sx, groundY - sz*0.1, sz*0.08, 0, Math.PI*2); ctx.fill();
  } else if(s.type==='battery'){
   ctx.fillStyle='#4696ff';ctx.fillRect(sx - sz*.06, groundY - sz*.12, sz*.12, sz*.2);
  }
 }
}

function hud(){
 const staminaHTML = `<br>${T('stamina')}：<div class="stamina-container"><div id="staminaBar"></div></div>`;
 $('status').innerHTML = `${T('battery')} ${Math.ceil(battery)}%${staminaHTML}<br>${T('key')}：${keyCount}/${REQUIRED_KEYS}`;
 updateStaminaUI();
 $('objective').textContent=keyCount>=REQUIRED_KEYS?(lang==='zh'?'跟隨金色光束尋找出口藍大門！':'Find Blue Exit Door!'):(lang==='zh'?`尋找出口鑰匙 (${keyCount}/${REQUIRED_KEYS})`:`Find Exit Keys (${keyCount}/${REQUIRED_KEYS})`);
}

let audioCtx=null,master=null;
function audioStart(){
 try{
  if(!audioCtx){audioCtx=new (window.AudioContext||window.webkitAudioContext)();master=audioCtx.createGain();master.gain.value=.18;master.connect(audioCtx.destination);}
  if(audioCtx.state==='suspended')audioCtx.resume();
 }catch(e){}
}
function beep(freq=220,dur=.15){try{audioStart();const o=audioCtx.createOscillator(),g=audioCtx.createGain();o.type='triangle';o.frequency.value=freq;g.gain.setValueAtTime(.0001,audioCtx.currentTime);g.gain.exponentialRampToValueAtTime(.18,audioCtx.currentTime+.015);g.gain.exponentialRampToValueAtTime(.0001,audioCtx.currentTime+dur);o.connect(g);g.connect(master);o.start();o.stop(audioCtx.currentTime+dur+.02);}catch(e){}}

function loop(now){const dt=Math.min(.05,(now-lastTime)/1000||0);lastTime=now;if(started){update(dt);render();hud();}else{render();}requestAnimationFrame(loop);}
requestAnimationFrame(loop);
})();
</script>
</body>
</html>
