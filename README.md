<!DOCTYPE html>
<html>
<head>
<title>First 5 Min Breakout Alert</title>
<style>
body{
background:#0f172a;
color:white;
font-family:Arial;
text-align:center;
padding:30px;
}
.box{
background:#1e293b;
padding:20px;
border-radius:10px;
width:350px;
margin:auto;
}
input,button{
padding:10px;
margin:10px;
width:90%;
border:none;
border-radius:5px;
}
button{
background:#22c55e;
font-weight:bold;
cursor:pointer;
}
.result{
margin-top:20px;
}
</style>
</head>
<body>

<h2>First 5 Minute Breakout Alert</h2>

<div class="box">
<input type="number" id="high" placeholder="First Candle High">
<input type="number" id="low" placeholder="First Candle Low">

<button onclick="generate()">Generate Alert</button>

<div class="result" id="output"></div>
</div>

<script>
function generate(){
let high=parseFloat(document.getElementById("high").value);
let low=parseFloat(document.getElementById("low").value);

let buyAlert=high+2;
let sellAlert=low-2;

document.getElementById("output").innerHTML=`
Buy Alert Above: ${buyAlert}<br>
Stop Loss: ${low}<br><br>
Sell Alert Below: ${sellAlert}<br>
Stop Loss: ${high}
`;
}
</script>

</body>
</html>
