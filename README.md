<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<meta name="theme-color" content="#090b10">
<title>最後一堂課：校舍擴建版 | The Last Lesson Expanded</title>
<style>
:root{--bg:#090b10;--panel:#151923;--line:#39404d;--ink:#e8e9e5;--muted:#a5a9b2;--gold:#e2c77b;--red:#bd5555;--stamina:#4caf50}
*{box-sizing:border-box}
html,body{margin:0;width:100%;height:100%;overflow:hidden;background:var(--bg);color:var(--ink);font-family:ui-monospace,Consolas,monospace;touch-action:none}
button{font:inherit;color:inherit;background:#202633;border:1px solid #596171;border-radius:8px;padding:10px 13px;cursor:pointer}
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
#dangerAlert{position:fixed;top:65px;left:50%;transform:translateX(-50%);background:rgba(189,85,85,0.85);color:#fff;padding:6px 16px;border-radius:20px;font-size:12px;font-weight:bold;letter-spacing:1px;box-shadow:0 0 10px rgba(255,0,0,0.5);display:none;z-index:4;pointer-events:none;animation:pulse 1s infinite alternate}
@keyframes pulse{0%{opacity:0.7}100%{opacity:1}}
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
      <div id="chapter">第一章：擴建的大樓</div>
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
<div id="controls">
  <div id="stickArea"><div id="stickBase"></div><div id="stick"></div></div>
  <div class="actions">
    <button id="flashBtn">手電筒<br>LIGHT</button>
    <button id="runBtn">奔跑<br>RUN</button>
    <button id="interactBtn">互動<br>USE</button>
    <button id="pauseBtn">暫停<br>PAUSE</button>
  </div>
</div>
<div id="desktopHint">手機：左搖桿移動・畫面拖曳視角 | 電腦：WASD 移動・Shift 跑步・↑↓/I/K 仰俯視角・E 互動</div>

<div id="overlay">
 <div class="card">
  <div style="display:flex;align-items:center;gap:8px"><div style="font-size:11px;color:#d7bd7d;letter-spacing:3px">PIXEL HORROR / 1.5 EXPANDED</div><button id="langBtn">EN</button></div>
  <h1 id="title">最後一堂課</h1>
  <div class="sub" id="subtitle">THE LAST LESSON: EXPANDED</div>
  <p id="intro">你在放學後的昏暗教學樓醒來。校舍地圖已進一步擴建，探索面積加倍！包含南北多棟教室區與交錯縱橫的走廊通道。<br><br>請利用分支走廊避開巡邏的異物，自由穿梭於廣大的教室區域尋找 5 把出口鑰匙成功逃脫！</p>
  <p class="small" id="howto">操作：左側搖桿移動，拖曳右側畫面可調整視角。按「互動」開門與搜尋物品！</p
