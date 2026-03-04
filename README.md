<!DOCTYPE html>
<html>
<head>
<title>NSE Auto 5-Min Breakout</title>
<style>
body{
background:#0f172a;
color:white;
font-family:Arial;
text-align:center;
}
.container{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(300px,1fr));
gap:20px;
padding:20px;
}
.box{
background:#1e293b;
padding:15px;
border-radius:10px;
}
.alert{
margin-top:10px;
font-weight:bold;
font-size:18px;
}
</style>
</head>
<body>

<h2>📈 NSE AUTO 5-MIN BREAKOUT SYSTEM</h2>
<p>Auto detects first candle & breakout</p>

<div class="container" id="stocks"></div>

<script>

let stocks = [
"HINDALCO.NS",
"BPCL.NS",
"TATASTEEL.NS",
"IOC.NS",
"M&M.NS",
"BEL.NS",
"CANBK.NS",
"ONGC.NS",
"^NSEI",
"TATAMOTORS.NS"
];

let dataStore = {};
let firstCandleDone = false;

function createUI(){
let container = document.getElementById("stocks");
stocks.forEach(sym=>{
dataStore[sym] = {high:0, low:999999, alerted:false};
let box = document.createElement("div");
box.className="box";
box.id=sym;
box.innerHTML=`<h3>${sym}</h3>
<div class="alert">Waiting...</div>`;
container.appendChild(box);
});
}

async function fetchPrice(symbol){
let url=`https://query1.finance.yahoo.com/v8/finance/chart/${symbol}?interval=1m`;
let res = await fetch(url);
let data = await res.json();

let candles = data.chart.result[0];
let high = Math.max(...candles.indicators.quote[0].high.slice(-5));
let low = Math.min(...candles.indicators.quote[0].low.slice(-5));
let price = candles.meta.regularMarketPrice;

return {high,low,price};
}

async function monitor(){
let now = new Date();
let minutes = now.getHours()*60 + now.getMinutes();
let marketStart = 9*60+15;
let first5End = 9*60+20;

for(let sym of stocks){

try{
let {high,low,price} = await fetchPrice(sym);

if(minutes >= marketStart && minutes < first5End){
dataStore[sym].high = Math.max(dataStore[sym].high, high);
dataStore[sym].low = Math.min(dataStore[sym].low, low);
document.querySelector(`#${sym} .alert`).innerHTML =
"Recording First Candle...";
}

if(minutes >= first5End){

if(!dataStore[sym].alerted){

if(price > dataStore[sym].high){
alert(sym + " BUY BREAKOUT 🚀");
document.querySelector(`#${sym} .alert`).innerHTML =
"🚀 BUY BREAKOUT";
playSound();
dataStore[sym].alerted=true;
}

if(price < dataStore[sym].low){
alert(sym + " SELL BREAKOUT 🔻");
document.querySelector(`#${sym} .alert`).innerHTML =
"🔻 SELL BREAKOUT";
playSound();
dataStore[sym].alerted=true;
}

}
}

}catch(e){
console.log("Error fetching",sym);
}

}
}

function playSound(){
let audio = new Audio("https://actions.google.com/sounds/v1/alarms/beep_short.ogg");
audio.play();
}

createUI();
setInterval(monitor,15000);

</script>

</body>
</html>
