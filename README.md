<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Air Drawing App</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial,sans-serif;
}

body{
    background:#111;
    color:white;
    display:flex;
    flex-direction:column;
    height:100vh;
}

.toolbar{
    background:#222;
    padding:10px;
    display:flex;
    gap:10px;
    align-items:center;
    flex-wrap:wrap;
}

button{
    padding:8px 15px;
    border:none;
    border-radius:5px;
    cursor:pointer;
    background:#444;
    color:white;
}

button:hover{
    background:#666;
}

input[type="color"]{
    width:50px;
    height:40px;
    border:none;
    cursor:pointer;
}

canvas{
    flex:1;
    background:white;
    touch-action:none;
}
</style>
</head>
<body>

<div class="toolbar">
    <label>Color:</label>
    <input type="color" id="colorPicker" value="#ff0000">

    <label>Brush:</label>
    <input type="range" id="brushSize" min="1" max="30" value="5">

    <button id="clearBtn">Clear</button>
    <button id="saveBtn">Save</button>
</div>

<canvas id="canvas"></canvas>

<script>
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight - 60;
}
resizeCanvas();
window.addEventListener("resize", resizeCanvas);

let drawing = false;

ctx.lineCap = "round";
ctx.lineJoin = "round";

function startDraw(e){
    drawing = true;
    draw(e);
}

function stopDraw(){
    drawing = false;
    ctx.beginPath();
}

function draw(e){
    if(!drawing) return;

    const rect = canvas.getBoundingClientRect();

    let x,y;

    if(e.touches){
        x = e.touches[0].clientX - rect.left;
        y = e.touches[0].clientY - rect.top;
    }else{
        x = e.clientX - rect.left;
        y = e.clientY - rect.top;
    }

    ctx.lineWidth =
        document.getElementById("brushSize").value;
    ctx.strokeStyle =
        document.getElementById("colorPicker").value;

    ctx.lineTo(x,y);
    ctx.stroke();

    ctx.beginPath();
    ctx.moveTo(x,y);
}

// Mouse Events
canvas.addEventListener("mousedown", startDraw);
canvas.addEventListener("mouseup", stopDraw);
canvas.addEventListener("mouseleave", stopDraw);
canvas.addEventListener("mousemove", draw);

// Touch Events
canvas.addEventListener("touchstart", startDraw);
canvas.addEventListener("touchend", stopDraw);
canvas.addEventListener("touchmove", draw);

// Clear
document.getElementById("clearBtn").addEventListener("click",()=>{
    ctx.clearRect(0,0,canvas.width,canvas.height);
});

// Save Image
document.getElementById("saveBtn").addEventListener("click",()=>{
    const link = document.createElement("a");
    link.download = "air-drawing.png";
    link.href = canvas.toDataURL();
    link.click();
});
</script>

</body>
</html>
