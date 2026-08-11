<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Password Strength Checker</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial,Helvetica,sans-serif;
}

body{
    background:#0f172a;
    display:flex;
    justify-content:center;
    align-items:center;
    min-height:100vh;
}

.container{
    width:420px;
    background:#1e293b;
    color:white;
    padding:25px;
    border-radius:15px;
    box-shadow:0 0 20px rgba(0,0,0,.4);
}

h2{
    text-align:center;
    margin-bottom:20px;
}

.input-box{
    position:relative;
}

input{
    width:100%;
    padding:12px;
    padding-right:80px;
    font-size:16px;
    border:none;
    outline:none;
    border-radius:8px;
}

button{
    position:absolute;
    top:4px;
    right:4px;
    padding:8px 12px;
    border:none;
    border-radius:6px;
    background:#2563eb;
    color:white;
    cursor:pointer;
}

.progress{
    width:100%;
    height:10px;
    background:#475569;
    border-radius:20px;
    overflow:hidden;
    margin-top:20px;
}

.bar{
    width:0%;
    height:100%;
    background:red;
    transition:.3s;
}

.result{
    margin-top:15px;
    font-size:20px;
    font-weight:bold;
}

ul{
    list-style:none;
    margin-top:20px;
}

li{
    margin:8px 0;
}

.good{
    color:#22c55e;
}

.bad{
    color:#ef4444;
}

.time{
    margin-top:20px;
    color:#38bdf8;
    font-weight:bold;
}
</style>

</head>
<body>

<div class="container">

<h2>Password Strength Checker</h2>

<div class="input-box">
<input
type="password"
id="password"
placeholder="Enter Password"
autocomplete="off">
<button onclick="togglePassword()">Show</button>
</div>

<div class="progress">
<div class="bar" id="bar"></div>
</div>

<div class="result" id="strength">
Strength :
</div>

<ul>
<li id="length">❌ At least 8 characters</li>
<li id="lower">❌ Lowercase Letter</li>
<li id="upper">❌ Uppercase Letter</li>
<li id="number">❌ Number</li>
<li id="special">❌ Special Character</li>
<li id="space">✅ No spaces allowed</li>
</ul>

<div class="time" id="crack">
Estimated Crack Time :
</div>

</div>

<script>

const input=document.getElementById("password");

// Remove all spaces automatically
input.addEventListener("input",function(){

    this.value=this.value.replace(/\s/g,"");

    checkPassword();

});

function togglePassword(){

    if(input.type==="password"){
        input.type="text";
    }else{
        input.type="password";
    }

}

function setStatus(id,condition,text){

    let item=document.getElementById(id);

    if(condition){
        item.innerHTML="✅ "+text;
        item.className="good";
    }else{
        item.innerHTML="❌ "+text;
        item.className="bad";
    }

}

function formatTime(seconds){

    if(seconds<60)
        return seconds.toFixed(2)+" seconds";

    if(seconds<3600)
        return (seconds/60).toFixed(2)+" minutes";

    if(seconds<86400)
        return (seconds/3600).toFixed(2)+" hours";

    if(seconds<31536000)
        return (seconds/86400).toFixed(2)+" days";

    return (seconds/31536000).toExponential(2)+" years";

}

function checkPassword(){

    let pass=input.value;

    let score=0;
    let charset=0;

    const lower=/[a-z]/.test(pass);
    const upper=/[A-Z]/.test(pass);
    const number=/[0-9]/.test(pass);
    const special=/[^A-Za-z0-9]/.test(pass);
    const length=pass.length>=8;
    const noSpace=!/\s/.test(pass);

    setStatus("length",length,"At least 8 characters");
    setStatus("lower",lower,"Lowercase Letter");
    setStatus("upper",upper,"Uppercase Letter");
    setStatus("number",number,"Number");
    setStatus("special",special,"Special Character");
    setStatus("space",noSpace,"No spaces allowed");

    if(length) score++;

    if(lower){
        score++;
        charset+=26;
    }

    if(upper){
        score++;
        charset+=26;
    }

    if(number){
        score++;
        charset+=10;
    }

    if(special){
        score++;
        charset+=32;
    }

    if(charset===0)
        charset=26;

    let strength,color,width;

    if(score<=2){
        strength="Weak";
        color="#ef4444";
        width="25%";
    }
    else if(score===3){
        strength="Medium";
        color="#f59e0b";
        width="50%";
    }
    else if(score===4){
        strength="Strong";
        color="#22c55e";
        width="75%";
    }
    else{
        strength="Very Strong";
        color="#2563eb";
        width="100%";
    }

    document.getElementById("strength").innerHTML="Strength : "+strength;

    const bar=document.getElementById("bar");
    bar.style.width=width;
    bar.style.background=color;

    let combinations=Math.pow(charset,pass.length);
    let guessesPerSecond=1000000000;

    let seconds=combinations/guessesPerSecond;

    document.getElementById("crack").innerHTML=
    "Estimated Crack Time : "+formatTime(seconds);

}

</script>

</body>
</html>
```
