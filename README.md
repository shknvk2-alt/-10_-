biology-10_gryby.github.io 
тест-інтерактив
<!DOCTYPE html>
<html lang="uk">
<head>
<meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>Грибний батл — онлайн-кімната</title>
<style>
:root{--g:#20563e;--g2:#317b55;--gold:#f3c95b;--bg:#eef4ec;--line:#d8e3d8;--ink:#20332a}
*{box-sizing:border-box}body{margin:0;background:var(--bg);color:var(--ink);font:16px/1.5 system-ui,sans-serif}
header{background:linear-gradient(120deg,#153e2d,#317b55);color:white;padding:25px 16px}main{max-width:940px;margin:auto;padding:20px 14px 45px}.wrap{max-width:940px;margin:auto}h1{font-size:clamp(2rem,5vw,3rem);margin:5px 0;line-height:1.1}header p{margin:10px 0 0}.panel{background:white;border:1px solid var(--line);border-radius:17px;padding:20px;margin-bottom:15px;box-shadow:0 5px 18px #1b4c2c0b}
h2{margin:0 0 12px}.muted{color:#5d7163}.grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}label{display:block;font-weight:700;margin:9px 0 5px}input,select{width:100%;padding:12px;border:1px solid #b7c9b9;border-radius:10px;font:inherit;background:white}.btn{border:0;border-radius:11px;padding:12px 17px;font:inherit;font-weight:750;cursor:pointer}.primary{background:var(--g);color:white}.gold{background:var(--gold);color:#372b08}.light{background:#e6efe6;color:var(--g)}.row{display:flex;flex-wrap:wrap;gap:10px;margin-top:14px}.hidden{display:none!important}.status{padding:11px 13px;background:#edf5ed;border-radius:10px;margin:12px 0}.code{font-size:2rem;letter-spacing:.12em;font-weight:900;color:var(--g);word-break:break-word}.question{font-size:1.25rem;font-weight:800;margin:14px 0}.options{display:grid;gap:8px}.opt{display:flex;align-items:flex-start;gap:9px;padding:11px;border:1px solid var(--line);border-radius:10px;cursor:pointer}.opt input{width:auto;margin-top:6px;accent-color:var(--g)}.match{display:grid;grid-template-columns:1fr 1.2fr;gap:10px;align-items:center;margin:8px 0}.wordbank{background:#f1f7ef;padding:10px;border-radius:10px;margin:10px 0}.wordbank span{display:inline-block;background:white;border:1px solid var(--line);padding:3px 7px;border-radius:6px;margin:3px}table{width:100%;border-collapse:collapse}th,td{text-align:left;padding:9px;border-bottom:1px solid var(--line)}.score{font-size:2rem;color:var(--g);font-weight:900}.playerrow{padding:8px 10px;border-bottom:1px solid var(--line)}.playerrow:last-child{border:0}.pill{display:inline-block;background:#eaf4e9;border-radius:999px;padding:4px 10px;font-size:.88rem}
@media(max-width:600px){.grid,.match{grid-template-columns:1fr}.panel{padding:15px}.code{font-size:1.6rem}}
</style>
</head>
<body>
<header><div class="wrap"><span class="pill">БІОЛОГІЯ · 10 КЛАС</span><h1>🍄 Грибний батл: онлайн-кімната</h1><p>Учитель створює кімнату, учні приєднуються за кодом, а рейтинг оновлюється наживо.</p></div></header>
<main>
<section class="panel" id="home">
<h2>Як хочеш приєднатися?</h2>
<p class="muted">Для зв’язку між пристроями потрібен інтернет. Учитель має залишити вкладку кімнати відкритою до завершення гри.</p>
<div class="grid">
<div class="panel" style="margin:0;background:#f8fbf6"><h2>👩‍🏫 Я — учитель</h2><label for="teacherName">Ім’я вчителя</label><input id="teacherName" maxlength="30" placeholder="Наприклад, пані Олена"><div class="row"><button class="btn primary" id="createBtn">Створити кімнату</button></div></div>
<div class="panel" style="margin:0;background:#f8fbf6"><h2>🧑‍🎓 Я — учень</h2><label for="roomCodeInput">Код кімнати</label><input id="roomCodeInput" maxlength="40" placeholder="Введи код від учителя"><label for="studentName">Твій нікнейм</label><input id="studentName" maxlength="24" placeholder="Наприклад, Мухоморчик"><div class="row"><button class="btn primary" id="joinBtn">Приєднатися</button></div></div>
</div>
<div class="status" id="connectionStatus" aria-live="polite">Стан: ще не підключено.</div>
<p class="muted" style="font-size:.9rem">Технічна примітка: зв’язок організовано через PeerJS для демонстраційної гри. Доступність зовнішнього сервісу залежить від мережі та її обмежень.</p>
</section>

<section class="panel hidden" id="roomPanel">
<div class="grid">
<div><h2>🔑 Код кімнати</h2><div class="code" id="roomCodeDisplay">—</div><p class="muted">Передай цей код учням. Вони вводять його на своїх пристроях.</p><div class="row"><button class="btn light" id="copyBtn">Копіювати код</button></div></div>
<div><h2>👥 Учасники <span class="pill" id="playerCount">0</span></h2><div id="playersList" class="muted">Очікуємо на учасників…</div></div>
</div>
<div class="row" id="teacherControls"><button class="btn primary" id="startGameBtn" disabled>Почати гру</button><button class="btn light" id="closeRoomBtn">Закрити кімнату</button></div>
<div class="status" id="hostStatus">Запусти кімнату й дочекайся, поки учні приєднаються.</div>
</section>

<section class="panel hidden" id="gamePanel">
<div class="grid"><div><span class="pill" id="phaseLabel">Очікування</span><h2 id="roundLabel">Раунд</h2></div><div><h2>🏆 Рейтинг наживо</h2><div id="leaderboard"></div></div></div>
<div id="questionBox"></div>
<div class="row" id="answerControls"><button class="btn primary" id="sendAnswerBtn">Надіслати відповідь</button></div>
<div class="status" id="answerStatus">Очікуємо запитання…</div>
<div class="row hidden" id="hostGameControls"><button class="btn gold" id="revealBtn">Показати результати раунду</button><button class="btn primary" id="nextQuestionBtn">Наступне запитання →</button><button class="btn light" id="finishBtn">Завершити гру</button></div>
</section>

<section class="panel hidden" id="endPanel"><h2>🎉 Гру завершено!</h2><div class="score" id="winnerText"></div><div id="finalBoard"></div><div class="row"><button class="btn primary" onclick="location.reload()">На головну</button></div></section>
</main>
<script src="https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js"></script>
<script>
const $=id=>document.getElementById(id);
const bank=[
{type:"Одна відповідь",title:"Яка структура утворює основну вегетативну частину більшості багатоклітинних грибів?",kind:"radio",opts:["Плодове тіло","Міцелій (грибниця)","Спора","Шапинка"],ans:[1],exp:"Міцелій — система гіф, що росте в субстраті або на його поверхні."},
{type:"Кілька відповідей",title:"Які твердження про гриби правильні?",kind:"check",opts:["Гриби є еукаріотами","Клітинна стінка грибів містить хітин","Усі гриби утворюють великі плодові тіла","Гриби живляться готовими органічними речовинами","Гриби мають хлорофіл і фотосинтезують"],ans:[0,1,3],exp:"Гриби — еукаріотичні гетеротрофи; у клітинній стінці багатьох грибів є хітин."},
{type:"Відповідність",title:"Поєднай структуру гриба з її функцією.",kind:"match",pairs:[["Гіфи","Тонкі нитки, з яких формується міцелій"],["Міцелій","Вегетативне тіло гриба"],["Плодове тіло","Структура, у якій або на якій утворюються спори"],["Спора","Структура для розмноження та поширення"]],exp:"Гіфи формують міцелій; плодове тіло бере участь у спороутворенні."},
{type:"Склади речення",title:"Розташуй слова в правильному порядку.",kind:"order",words:["гриби","готові","органічні","речовини","поглинають","із","довкілля"],ansText:"гриби поглинають готові органічні речовини із довкілля",exp:"Гриби є гетеротрофами й поглинають розчинені поживні речовини."},
{type:"Одна відповідь",title:"Яка речовина є запасним вуглеводом у грибів?",kind:"radio",opts:["Крохмаль","Глікоген","Целюлоза","Сахароза"],ans:[1],exp:"Для грибів характерне запасання глікогену."},
{type:"Кілька відповідей",title:"Які способи живлення трапляються у грибів?",kind:"check",opts:["Сапротрофне — використання органічних решток","Паразитичне — живлення за рахунок хазяїна","Симбіотичне — співіснування з іншими організмами","Автотрофне — фотосинтез"],ans:[0,1,2],exp:"Гриби можуть бути сапротрофами, паразитами або симбіонтами."},
{type:"Відповідність",title:"Поєднай групу грибів з характеристикою.",kind:"match",pairs:[["Дріжджі","Одноклітинні гриби, що часто розмножуються брунькуванням"],["Плісняві гриби","Нитчастий міцелій; приклад — мукор"],["Шапинкові гриби","Мають плодові тіла, наприклад шапинку й ніжку"],["Гриби-паразити","Живуть за рахунок організму-хазяїна"]],exp:"Серед грибів є одноклітинні дріжджі, плісняві, шапинкові та паразитичні види."},
{type:"Зайвий термін",title:"Обери зайвий термін.",kind:"odd",opts:["Хітин","Глікоген","Хлорофіл","Гіфи"],ans:[2],exp:"Хлорофіл потрібний для фотосинтезу й не характерний для грибів."},
{type:"Одна відповідь",title:"Що зазвичай називають мікоризою?",kind:"radio",opts:["Симбіоз гриба з коренями рослини","Злиття двох спор","Плодове тіло дріжджів","Вірусну хворобу"],ans:[0],exp:"Мікориза — симбіотичний зв’язок гриба з коренями рослини."},
{type:"Кілька відповідей",title:"Які ознаки характерні для грибів?",kind:"check",opts:["Клітини мають ядро","Поглинають розчинені поживні речовини","Мають пластиди з хлорофілом","Клітинна стінка часто містить хітин","Усі гриби багатоклітинні"],ans:[0,1,3],exp:"Гриби — еукаріоти й гетеротрофи; дріжджі є одноклітинними грибами."},
{type:"Склади речення",title:"Розташуй слова в правильному порядку.",kind:"order",words:["спори","допомагають","грибам","розмножуватися","та","поширюватися"],ansText:"спори допомагають грибам розмножуватися та поширюватися",exp:"Спори можуть переноситися різними шляхами й давати початок новому міцелію."},
{type:"Відповідність",title:"Поєднай приклад з екологічною роллю.",kind:"match",pairs:[["Пліснява на рештках листя","Розкладання органічних решток"],["Трутовик на дереві","Розкладання деревини; деякі види паразитують"],["Гриб у складі лишайника","Симбіоз із фотосинтезувальним партнером"],["Борошниста роса","Паразитування на рослинах"]],exp:"Гриби є редуцентами, симбіонтами та паразитами."},
{type:"Зайвий термін",title:"Обери зайвий термін.",kind:"odd",opts:["Печериця","Білий гриб","Маслюк","Дріжджі"],ans:[3],exp:"Дріжджі переважно одноклітинні, а інші приклади — шапинкові гриби."},
{type:"Одна відповідь",title:"Яка частина шапинкового гриба найчастіше утворює спори?",kind:"radio",opts:["Увесь міцелій завжди утворює всі спори","Гіменофор (пластинки або трубочки)","Лише ніжка","Лише покрив шапинки"],ans:[1],exp:"У багатьох шапинкових грибів спори утворюються на гіменофорі під шапинкою."},
{type:"Кілька відповідей",title:"Які твердження про значення грибів правильні?",kind:"check",opts:["Розкладають органічні рештки","Використовуються у виробництві хліба та харчових продуктів","Усі гриби небезпечні для людини","Деякі спричиняють хвороби","Мікоризні гриби можуть сприяти мінеральному живленню рослин"],ans:[0,1,3,4],exp:"Гриби мають екологічне й господарське значення."},
{type:"Зайвий термін",title:"Обери зайвий термін.",kind:"odd",opts:["Сапротроф","Паразит","Симбіонт","Автотроф"],ans:[3],exp:"Гриби є гетеротрофами; автотрофи самі утворюють органічні речовини з неорганічних."}
];
let peer=null,host=false,roomCode='',hostConn=null,connections={},players={},myName='',questionIndex=-1,phase='lobby',answered=false,questionStart=0,roundAnswers={},scores={},revealed=false;
const esc=s=>String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
function status(s){$('connectionStatus').textContent='Стан: '+s}
function show(id){['home','roomPanel','gamePanel','endPanel'].forEach(x=>$(x).classList.toggle('hidden',x!==id))}
function codeGen(){return 'GRYBY-'+Math.floor(100000+Math.random()*900000)}
function broadcast(msg){Object.values(connections).forEach(c=>{if(c.open)c.send(msg)})}
function connectPeer(id){return new Promise((resolve,reject)=>{peer=new Peer(id);peer.on('open',resolve);peer.on('error',reject);setTimeout(()=>reject(new Error('Час очікування підключення минув.')),12000)})}
$('createBtn').onclick=async()=>{host=true;let nm=$('teacherName').value.trim()||'Учитель';roomCode=codeGen();status('створюємо кімнату…');try{await connectPeer(roomCode.toLowerCase());$('roomCodeDisplay').textContent=roomCode;show('roomPanel');$('hostStatus').textContent=`Кімната готова. Власник: ${nm}. Надішли учням код ${roomCode}.`;peer.on('connection',conn=>{conn.on('open',()=>{});conn.on('data',data=>handleHostData(conn,data));conn.on('close',()=>{delete connections[conn.peer];renderPlayers()});});status('кімната створена. Учні можуть приєднуватися.')}catch(e){status('не вдалося створити кімнату. Перевір інтернет або спробуй ще раз.');if(peer)peer.destroy();peer=null;host=false}};
function handleHostData(conn,d){if(!d||!d.type)return;if(d.type==='join'){let name=String(d.name||'Учасник').slice(0,24);if(Object.values(players).some(p=>p.name.toLowerCase()===name.toLowerCase()))name=name+'-'+Math.floor(Math.random()*90+10);players[conn.peer]={name,score:0,correct:0,answered:false};connections[conn.peer]=conn;conn.send({type:'welcome',name,roomCode,phase,questionIndex,players:publicPlayers()});renderPlayers();if(phase==='question')sendQuestion(conn);broadcast({type:'players',players:publicPlayers()});}
if(d.type==='answer'&&players[conn.peer]&&phase==='question'&&!players[conn.peer].answered){let q=bank[questionIndex];let result=grade(q,d.answer);let elapsed=Date.now()-questionStart;players[conn.peer].answered=true;roundAnswers[conn.peer]={correct:result.correct,earned:result.correct?Math.max(1,10-Math.floor(elapsed/4000)):0,explain:result.explain};players[conn.peer].score+=roundAnswers[conn.peer].earned;if(result.correct)players[conn.peer].correct++;conn.send({type:'answerAck',correct:result.correct,explain:result.explain,waiting:true});broadcast({type:'progress',count:Object.keys(roundAnswers).length,total:Object.keys(players).length,players:publicPlayers()});}
}
function publicPlayers(){return Object.entries(players).map(([id,p])=>({id,name:p.name,score:p.score,correct:p.correct,answered:p.answered})).sort((a,b)=>b.score-a.score||b.correct-a.correct)}
function renderPlayers(){let list=publicPlayers();$('playerCount').textContent=list.length;$('playersList').innerHTML=list.length?list.map((p,i)=>`<div class="playerrow">${i+1}. ${esc(p.name)} <span class="pill">${p.score} балів</span></div>`).join(''):'Очікуємо на учасників…';$('startGameBtn').disabled=list.length===0;renderLeaderboard()}
function renderLeaderboard(){let list=publicPlayers();$('leaderboard').innerHTML=list.length?list.slice(0,8).map((p,i)=>`<div class="playerrow">${i===0?'🥇':i===1?'🥈':i===2?'🥉':(i+1)+'.'} ${esc(p.name)} <strong style="float:right">${p.score}</strong></div>`).join(''):'Ще немає учасників.'}
$('copyBtn').onclick=async()=>{try{await navigator.clipboard.writeText(roomCode);$('hostStatus').textContent='Код скопійовано: '+roomCode}catch(e){$('hostStatus').textContent='Код для копіювання: '+roomCode}};
$('closeRoomBtn').onclick=()=>{if(confirm('Закрити кімнату?')){if(peer)peer.destroy();location.reload()}};
$('startGameBtn').onclick=()=>{if(!host||!Object.keys(players).length)return;questionIndex=0;phase='question';startRound()};
function startRound(){revealed=false;roundAnswers={};Object.values(players).forEach(p=>p.answered=false);questionStart=Date.now();show('gamePanel');$('phaseLabel').textContent=`Запитання ${questionIndex+1} / ${bank.length}`;$('roundLabel').textContent=bank[questionIndex].type;renderHostQuestion();broadcast({type:'question',index:questionIndex,question:publicQuestion(bank[questionIndex])});$('answerStatus').textContent='Учні відповідають…';$('hostGameControls').classList.remove('hidden');$('revealBtn').disabled=false;$('nextQuestionBtn').disabled=true;renderLeaderboard()}
function publicQuestion(q){return JSON.parse(JSON.stringify(q))}
function renderHostQuestion(){let q=bank[questionIndex];$('questionBox').innerHTML=`<div class="question">${esc(q.title)}</div><p class="muted">Це екран ведучого. Відповіді надсилають учні зі своїх пристроїв.</p>`}
function sendQuestion(conn){conn.send({type:'question',index:questionIndex,question:publicQuestion(bank[questionIndex])})}
$('revealBtn').onclick=()=>{if(!host)return;revealed=true;phase='reveal';let correct=Object.values(roundAnswers).filter(a=>a.correct).length;let explanation=bank[questionIndex].exp;broadcast({type:'reveal',answers:roundAnswers,explanation,players:publicPlayers()});$('answerStatus').textContent=`Відповіли: ${Object.keys(roundAnswers).length}/${Object.keys(players).length}. Правильних відповідей: ${correct}. ${explanation}`;$('revealBtn').disabled=true;$('nextQuestionBtn').disabled=false;renderLeaderboard()};
$('nextQuestionBtn').onclick=()=>{if(questionIndex>=bank.length-1){finishHost();return}questionIndex++;phase='question';startRound()};
$('finishBtn').onclick=()=>{if(confirm('Завершити гру зараз?'))finishHost()};
function finishHost(){phase='finished';broadcast({type:'finished',players:publicPlayers()});showEnd(publicPlayers());}
function showEnd(list){show('endPanel');let top=list[0];$('winnerText').textContent=top?`Переможець: ${top.name} — ${top.score} балів!`:'Гру завершено';$('finalBoard').innerHTML=list.map((p,i)=>`<div class="playerrow">${i===0?'🥇':i===1?'🥈':i===2?'🥉':i+1+'.'} ${esc(p.name)} — <strong>${p.score} балів</strong> · правильних: ${p.correct}</div>`).join('')}
function grade(q,a){let correct=false,explain=q.exp;if(q.kind==='radio'||q.kind==='check'||q.kind==='odd'){let sel=Array.isArray(a)?a.map(Number):[];correct=sel.length===q.ans.length&&sel.every(x=>q.ans.includes(x));if(!correct)explain='Правильна відповідь: '+q.ans.map(i=>q.opts[i]).join('; ')+'. '+q.exp}
if(q.kind==='order'){correct=norm(a||'')===norm(q.ansText);if(!correct)explain='Один із правильних варіантів: «'+q.ansText+'». '+q.exp}
if(q.kind==='match'){let n=0;(q.pairs||[]).forEach((p,i)=>{if(a&&Number(a[i])===i)n++});correct=n===q.pairs.length;explain=`Правильно встановлено ${n} із ${q.pairs.length} відповідностей. `+q.exp}
return {correct,explain}}
function norm(s){return String(s).toLocaleLowerCase('uk').replace(/[.,!?;:]/g,'').replace(/\s+/g,' ').trim()}
$('joinBtn').onclick=async()=>{let code=$('roomCodeInput').value.trim().toLowerCase(),name=$('studentName').value.trim();if(!code||!name){status('введи код кімнати та нікнейм.');return}myName=name;status('підключаємося до кімнати…');try{peer=new Peer();peer.on('error',e=>status('помилка з’єднання: '+e.type+'. Перевір код і спробуй ще раз.'));peer.on('open',()=>{hostConn=peer.connect(code,{reliable:true});hostConn.on('open',()=>{hostConn.send({type:'join',name});status('підключено до кімнати '+code)});hostConn.on('data',handleStudentData);hostConn.on('close',()=>status('з’єднання з кімнатою втрачено.'));});}catch(e){status('Не вдалося підключитися. Перевір код та інтернет.')}};
function handleStudentData(d){if(!d||!d.type)return;if(d.type==='welcome'){show('roomPanel');$('roomCodeDisplay').textContent=d.roomCode||$('roomCodeInput').value.toUpperCase();$('teacherControls').classList.add('hidden');$('hostStatus').textContent=`Ти приєднався/приєдналася як ${d.name}. Чекай, поки вчитель почне гру.`;players={};(d.players||[]).forEach(p=>players[p.id]=p);renderPlayers();if(d.phase==='question'&&d.questionIndex>=0){/* question arrives from host separately */}}
if(d.type==='players'||d.type==='progress'){players={};(d.players||[]).forEach(p=>players[p.id]=p);renderPlayers();if(d.type==='progress')$('answerStatus').textContent=`Відповіли: ${d.count}/${d.total}.`;}
if(d.type==='question'){phase='question';questionIndex=d.index;answered=false;show('gamePanel');$('phaseLabel').textContent=`Запитання ${d.index+1} / ${bank.length}`;$('roundLabel').textContent=d.question.type;$('answerStatus').textContent='Обери відповідь і надішли її.';renderStudentQuestion(d.question)}
if(d.type==='answerAck'){$('answerStatus').textContent=d.correct?'Відповідь прийнято! 🎉':'Відповідь прийнято — дочекайся пояснення.';answered=true;$('sendAnswerBtn').disabled=true}
if(d.type==='reveal'){$('answerStatus').textContent=d.explanation;players={};(d.players||[]).forEach(p=>players[p.id]=p);renderLeaderboard();$('sendAnswerBtn').classList.add('hidden');$('questionBox').insertAdjacentHTML('beforeend',`<div class="status">${esc(d.explanation)}</div>`)}
if(d.type==='finished')showEnd(d.players||[])}
function renderStudentQuestion(q){$('answerControls').classList.remove('hidden');$('sendAnswerBtn').classList.remove('hidden');$('sendAnswerBtn').disabled=false;let h=`<div class="question">${esc(q.title)}</div>`;
if(q.kind==='radio'||q.kind==='check'||q.kind==='odd'){h+='<div class="options">'+q.opts.map((o,i)=>`<label class="opt"><input type="${q.kind==='check'?'checkbox':'radio'}" name="stuanswer" value="${i}"><span>${esc(o)}</span></label>`).join('')+'</div>';if(q.kind==='check')h+='<p class="muted">Можна обрати кілька відповідей.</p>'}
if(q.kind==='order'){h+=`<div class="wordbank">${q.words.map(w=>`<span>${esc(w)}</span>`).join(' ')}</div><label for="studentSentence">Склади речення</label><input id="studentSentence" type="text" placeholder="Введи речення">`}
if(q.kind==='match'){h+=q.pairs.map((p,i)=>`<div class="match"><strong>${esc(p[0])}</strong><select name="stuMatch${i}"><option value="">— Обери характеристику —</option>${q.pairs.map((v,j)=>`<option value="${j}">${esc(v[1])}</option>`).join('')}</select></div>`).join('')}
$('questionBox').innerHTML=h;document.querySelectorAll('.opt input').forEach(inp=>inp.onchange=()=>document.querySelectorAll('.opt').forEach(el=>el.classList.toggle('selected',el.querySelector('input').checked)))}
$('sendAnswerBtn').onclick=()=>{if(!hostConn||!hostConn.open||answered)return;let q=bank[questionIndex],answer;if(q.kind==='radio'||q.kind==='check'||q.kind==='odd'){answer=[...document.querySelectorAll('input[name="stuanswer"]:checked')].map(x=>+x.value);if(!answer.length){$('answerStatus').textContent='Спочатку обери відповідь.';return}}
if(q.kind==='order'){answer=$('studentSentence').value.trim();if(!answer){$('answerStatus').textContent='Введи речення.';return}}
if(q.kind==='match'){answer=q.pairs.map((p,i)=>{let e=$(`questionBox`).querySelector(`[name="stuMatch${i}"]`);return e.value});if(answer.some(x=>x==='')){$('answerStatus').textContent='Заповни всі відповідності.';return}}
hostConn.send({type:'answer',answer});$('sendAnswerBtn').disabled=true;$('answerStatus').textContent='Відповідь надіслано. Очікуй на результати.'};
</script>
</body></html>
