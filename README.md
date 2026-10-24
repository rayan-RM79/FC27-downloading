
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>بوابة الطاقة | محاكاة تفاعلية</title>

<style>
*{box-sizing:border-box}
body{margin:0;font-family:Tahoma,Arial,sans-serif;background:#f4f7f5;color:#163c30}
header{background:#064b38;color:white;padding:18px 6%;display:flex;align-items:center;justify-content:space-between;gap:12px}
.brand{font-size:21px;font-weight:bold}
.brand small{display:block;font-size:11px;margin-top:6px}
button{border:0;border-radius:7px;padding:13px 19px;font-size:15px;font-weight:bold;cursor:pointer}
.login{background:#ddc477;color:#163c30}
main{max-width:1100px;margin:auto;padding:45px 20px}
.hero{background:linear-gradient(120deg,#064331,#16815f);color:white;padding:45px 30px;border-radius:12px}
.hero h1{font-size:clamp(28px,5vw,40px)}
.hero p{line-height:2}
.cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:18px;margin-top:24px}
.card{background:white;padding:22px;border-radius:10px;box-shadow:0 3px 15px #0000000a}
footer{text-align:center;padding:25px;color:#66736d;font-size:12px}

#simulation{display:none;position:fixed;inset:0;z-index:99;background:#020604;color:#42ff83;padding:20px;font-family:Consolas,monospace;overflow-y:auto}
#simulation.active{display:block}
.sim-note{text-align:center;color:#a0b6a8;font-size:12px}
#heading{text-align:center;color:#ff3030;font-size:clamp(25px,6vw,45px);font-weight:bold;margin:30px 0;animation:blink .7s infinite}
@keyframes blink{50%{opacity:.4}}
.track{height:22px;max-width:650px;border:1px solid #42ff83;margin:20px auto}
#bar{height:100%;width:0;background:#42ff83;transition:width .2s}
#percent{text-align:center;font-size:24px}
#logs{max-width:850px;margin:25px auto;line-height:1.9;overflow-wrap:anywhere}
.log{margin:5px 0}
#reveal{display:none;text-align:center;max-width:650px;margin:30px auto;padding:25px;border:2px solid #42ff83;background:#071b10;color:white}
#reveal h2{font-size:38px;color:#42ff83}
.control{display:block;margin:14px auto;background:#164630;color:white;border:1px solid #42ff83}
.stop{background:#922222;border-color:#922222}
@media(max-width:600px){header{padding:14px}.brand{font-size:16px}.hero{padding:28px 20px}}
</style>
</head>

<body>
<header>
  <div class="brand">
    بوابة الطاقة
    <small>واجهة خيالية تجريبية غير رسمية</small>
  </div>
  <button class="login" onclick="startSimulation()">تسجيل الدخول</button>
</header>

<main>
  <section class="hero">
    <h1>مرحباً بك في بوابة الطاقة</h1>
    <p>اكتشف الخدمات الرقمية والتقنيات الحديثة في هذه التجربة التفاعلية.</p>
    <button class="login" onclick="startSimulation()">استعراض النظام</button>
  </section>

  <section class="cards">
    <div class="card"><h3>⚡ حلول الطاقة</h3><p>تقنيات مبتكرة لمستقبل الطاقة.</p></div>
    <div class="card"><h3>🌱 الاستدامة</h3><p>حلول تدعم كفاءة الموارد.</p></div>
    <div class="card"><h3>💻 التقنية</h3><p>تجربة رقمية تفاعلية.</p></div>
  </section>
</main>

<footer>محاكاة ترفيهية مستقلة — ليست موقعاً رسمياً ولا تجمع بيانات دخول.</footer>

<section id="simulation">
  <div class="sim-note">LOCAL SIMULATION — محاكاة محلية فقط</div>
  <div id="heading">SYSTEM INITIALIZING...</div>
  <div class="track"><div id="bar"></div></div>
  <div id="percent">0%</div>
  <div id="logs"></div>

  <div id="reveal">
    <div style="font-size:65px">😂</div>
    <h2>صدقت!</h2>
    <p style="font-size:25px">طقطقة ياهطف 🤣</p>
    <p>هذي مجرد مؤثرات داخل المتصفح، وما صار أي اختراق لجهازك.</p>
    <button class="login" onclick="location.reload()">إعادة المزحة</button>
  </div>

  <button id="soundButton" class="control" onclick="enableSound()">🔊 تشغيل الأصوات</button>
  <button class="control stop" onclick="stopSimulation()">إيقاف المحاكاة</button>
</section>

<script>
let timer=null;
let audioContext=null;
let soundEnabled=false;
let running=false;
let progress=0;
let messageIndex=0;

const messages=[
  "[ OK ] Initializing visual interface...",
  "[ OK ] Loading simulated system modules...",
  "[ INFO ] Checking demonstration status...",
  "[ WARN ] Simulated unusual activity detected!",
  "[ INFO ] Rendering terminal effects...",
  "[ WARN ] SYSTEM WARNING — DEMO ONLY",
  "[ OK ] Preparing the prank reveal..."
];

function addLog(message,color){
  const logs=document.getElementById("logs");
  const line=document.createElement("div");
  line.className="log";
  line.textContent=message;
  if(color)line.style.color=color;
  logs.appendChild(line);
  logs.scrollTop=logs.scrollHeight;
}

function enableSound(){
  try{
    const AudioClass=window.AudioContext||window.webkitAudioContext;
    if(!AudioClass){
      alert("المتصفح لا يدعم الصوت. جرّب متصفحاً حديثاً.");
      return;
    }
    if(!audioContext)audioContext=new AudioClass();
    audioContext.resume();
    soundEnabled=true;
    document.getElementById("soundButton").textContent="🔊 الأصوات مفعّلة";
    beep(700,.12);
  }catch(error){
    alert("تعذر تشغيل الصوت في هذا المتصفح.");
  }
}

function beep(frequency,duration){
  if(!soundEnabled||!audioContext)return;
  const oscillator=audioContext.createOscillator();
  const gain=audioContext.createGain();
  oscillator.type="square";
  oscillator.frequency.value=frequency;
  gain.gain.setValueAtTime(.035,audioContext.currentTime);
  gain.gain.exponentialRampToValueAtTime(.001,audioContext.currentTime+duration);
  oscillator.connect(gain);
  gain.connect(audioContext.destination);
  oscillator.start();
  oscillator.stop(audioContext.currentTime+duration);
}

function startSimulation(){
  if(running)return;
  running=true;
  progress=0;
  messageIndex=0;

  document.getElementById("simulation").classList.add("active");
  document.getElementById("logs").textContent="";
  document.getElementById("reveal").style.display="none";
  document.getElementById("bar").style.width="0%";
  document.getElementById("percent").textContent="0%";
  document.getElementById("heading").textContent="SYSTEM INITIALIZING...";

  addLog("Starting visual simulation...");
  addLog("No files accessed. No credentials collected.");

  timer=setInterval(()=>{
    progress=Math.min(100,progress+Math.floor(Math.random()*4)+2);

    document.getElementById("bar").style.width=progress+"%";
    document.getElementById("percent").textContent=progress+"%";

    if(soundEnabled)beep(450+progress*5,.06);

    if(progress>=20&&messageIndex<1){
      addLog(messages[messageIndex++]);
    }
    if(progress>=35&&messageIndex<2){
      addLog(messages[messageIndex++]);
    }
    if(progress>=48&&messageIndex<3){
      addLog(messages[messageIndex++],"#ff4848");
    }
    if(progress>=60&&messageIndex<4){
      document.getElementById("heading").textContent="SYSTEM WARNING";
      addLog(messages[messageIndex++]);
    }
    if(progress>=75&&messageIndex<5){
      addLog(messages[messageIndex++],"#ff4848");
    }
    if(progress>=90&&messageIndex<6){
      addLog(messages[messageIndex++]);
    }

    if(progress>=100){
      clearInterval(timer);
      timer=null;
      setTimeout(finishSimulation,700);
    }
  },300);
}

function finishSimulation(){
  document.getElementById("heading").textContent="SYSTEM WARNING";
  addLog("SIMULATION COMPLETE.", "#42ff83");
  addLog("STATUS: NO REAL SECURITY INCIDENT.", "#ffe66d");
  document.getElementById("reveal").style.display="block";
  if(soundEnabled){
    beep(900,.2);
    setTimeout(()=>beep(650,.25),250);
    setTimeout(()=>beep(900,.2),500);
  }
  running=false;
}

function stopSimulation(){
  if(timer){
    clearInterval(timer);
    timer=null;
  }
  running=false;
  document.getElementById("simulation").classList.remove("active");
}
</script>
</body>
</html>
