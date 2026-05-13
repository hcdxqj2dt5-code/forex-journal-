# forex-journal-
html <!DOCTYPE html> 
<html lang="en"> 
<head> <meta charset="UTF-8"> 
<meta name="viewport" content="width=device-width, initial-scale=1.0"> <title>Forex Trading Journal</title>  
<style> 
body{     
    margin:0;     font-family:Arial, sans-serif;     background:#0f172a;     color:white; 
}  
.container{     
    max-width:1200px;     
    margin:auto;     
    padding:20px; }  
h1{     
    text-align:center;     
    margin-bottom:30px; 
}  
.card{     
    background:#1e293b;     
    padding:20px;     
    border-radius:15px;     
    margin-bottom:20px;     
    box-shadow:0 0 10px rgba(0,0,0,0.3); 
}  
input, textarea, button{     
    width:100%;     
    padding:12px;     
    margin-top:10px;     
    border:none;     
    border-radius:10px;     
    font-size:16px; }  input, textarea
{     
    background:#334155;     
    color:white; }  
    button{     
        background:#3b82f6;     
        color:white;     
        cursor:pointer;     
        transition:0.3s; }  
button:hover{     
    background:#2563eb; }  
.stats{     
    display:grid;     
    grid-template-columns:repeat(auto-fit,minmax(200px,1fr));     
    gap:15px; }  
.stat-box{     
    background:#334155;     
    padding:20px;     
    border-radius:15px;     
    text-align:center; }  
.trade-list{     
    display:grid;     
    gap:20px; }  
.trade-item{     
    background:#334155;     
    padding:15px;     
    border-radius:15px; }  
.trade-item img{     
    width:100%;     
    margin-top:10px;     
    border-radius:10px; }  
.green{     
    color:#22c55e; }  
.red{     color:#ef4444; }  
.calendar{     
    display:grid;     
    grid-template-columns:repeat(7,1fr);     
    gap:10px; }  
.day{     
    background:#334155;     
    padding:15px;     
    border-radius:10px;     
    text-align:center; }  
@media(max-width:600px){     
.calendar{         
    grid-template-columns:repeat(2,1fr);
   } 
} 
</style> 
</head>  
<body>  
<div class="container">  
<h1>📈 Forex Trading Journal</h1>  
<div class="card"> 
<h2>Add Trade</h2>  
<input type="date" id="date">  
<input type="text" id="pair" placeholder="Pair (EUR/USD)">  
<input type="number" id="risk" placeholder="Risk %">  
<input type="number" id="reward" placeholder="Reward %">  
<input type="number" id="profit" placeholder="Profit/Loss $">  
<textarea id="notes" placeholder="Trade notes..."></textarea>  
<input type="file" id="image">  
<button onclick="saveTrade()">Save Trade</button> </div>  
<div class="card"> 
<h2>Statistics</h2>  
<div class="stats">  
<div class="stat-box"> 
<h3>Total Trades</h3> 
<p id="totalTrades">0</p> 
</div>  
<div class="stat-box"> 
<h3>Wins</h3> 
<p id="wins">0</p> 
</div>  
<div class="stat-box"> 
<h3>Losses</h3> 
<p id="losses">0</p> 
</div>  
<div class="stat-box"> 
<h3>Win Rate</h3> 
<p id="winRate">0%</p> 
</div>  
<div class="stat-box"> 
<h3>Total P/L</h3> 
<p id="totalPL">$0</p> 
</div>  
</div> 
</div> 
<div class="card"> 
<h2>Trading Calendar</h2>  
<div class="calendar" id="calendar"></div> 
</div>  
<div class="card"> 
<h2>Trade History</h2>  
<div class="trade-list" id="tradeList"></div> 
</div>  
</div>  
<script>  
let trades = JSON.parse(localStorage.getItem("trades")) || [];  
function saveTrade(){      
    const date = document.getElementById("date").value;     
    const pair = document.getElementById("pair").value;     
    const risk = document.getElementById("risk").value;     
    const reward = document.getElementById("reward").value;     
    const profit = document.getElementById("profit").value;     
    const notes = document.getElementById("notes").value;      
    const imageInput = document.getElementById("image");      
    const reader = new FileReader();      
    reader.onload = function(){          
        const trade = {             
            date,             
            pair,             
            risk,             
            reward,             
            profit,             
            notes,             
            image: reader.result         
};          trades.push(trade);          
localStorage.setItem("trades", JSON.stringify(trades));          
renderTrades();         
renderStats();         
renderCalendar();          
alert("Trade Saved!");      
};      
if(imageInput.files[0]){         
    reader.readAsDataURL(imageInput.files[0]);     
} else {          
    const trade = {             
        date,             
        pair,             
        risk,             
        reward,             
        profit,             
        notes,             
        image:null         
};          
trades.push(trade);          
localStorage.setItem("trades", JSON.stringify(trades));          
renderTrades();         
renderStats();         
renderCalendar();          
alert("Trade Saved!");     
} 
}  function renderTrades(){      
    const tradeList = document.getElementById("tradeList");      
    tradeList.innerHTML = "";      
    trades.reverse().forEach(trade=>{          
        tradeList.innerHTML += `         
<div class="trade-item">              
<h3>${trade.pair}</h3>              
<p><strong>Date:</strong> ${trade.date}</p>              
<p><strong>Risk:</strong> ${trade.risk}%</p>              
<p><strong>Reward:</strong> ${trade.reward}%</p>              
<p>                 
<strong>P/L:</strong>                 
<span class="${trade.profit >= 0 ? 'green':'red'}">                 
$${trade.profit}                 
</span>             
</p>              
<p><strong>Notes:</strong> ${trade.notes}</p>              
${                 
trade.image ?                 
`<img src="${trade.image}">`                 :                 
""             
}          
</div>         
`;     
});      
trades.reverse(); }  
function renderStats(){      
    const totalTrades = trades.length;      
    const wins = trades.filter(t=>t.profit > 0).length;      
    const losses = trades.filter(t=>t.profit <=0).length;      
    const totalPL = trades.reduce((a,b)=>a + Number(b.profit),0);      
    const winRate = totalTrades         ? ((wins / totalTrades) *100).toFixed(1)         
:0;      
document.getElementById("totalTrades").innerText = totalTrades;     
document.getElementById("wins").innerText = wins;     
document.getElementById("losses").innerText = losses;     
document.getElementById("winRate").innerText = winRate + "%";     
document.getElementById("totalPL").innerText = "$" + totalPL;  }  
function renderCalendar(){      
    const calendar = document.getElementById("calendar");      
    calendar.innerHTML = "";      
    trades.forEach(trade=>{          
        const div = document.createElement("div");          
        div.classList.add("day");          
        div.innerHTML = `         
<strong>${trade.date}</strong><br>         
${trade.pair}<br>         
<span class="${trade.profit >=0 ? 'green':'red'}">         
$${trade.profit}         
</span>         
`;          
calendar.appendChild(div);     
}); 
}  
renderTrades(); 
renderStats(); 
renderCalendar();  
</script>  
</body> 
</html> 