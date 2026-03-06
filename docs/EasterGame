<!DOCTYPE html>
<html lang="hu">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vagyok, aki vagyok. De ki vagyok?</title>

<style>
body{
font-family:Lora;
text-align:center;
background:#f2f2f2;
margin:0;
padding:0;
}

h1{margin-top:20px;}

.grid{
display:grid;
grid-template-columns:repeat(4,150px);
gap:20px;
justify-content:center;
margin-top:20px;
}

.card{
background:#fff;
border-radius:10px;
padding:10px;
box-shadow:0 4px 10px rgba(0,0,0,0.2);
transition:0.5s;
height:90px;
}

.card.open{
transform:scale(1.05);
}

.card input{
width:100px;
padding:6px;
font-size:16px;
}

.hidden-word{
display:flex;
justify-content:center;
align-items:center;
font-weight:bold;
font-size:18px;
background:#66c8d0;
color:white;
border-radius:10px;
padding:8px;
margin-top:5px;
min-height:30px;
}

#sentence{
margin-top:30px;
min-height:70px;
border:2px dashed #aaa;
padding:10px;
display:flex;
flex-wrap:wrap;
justify-content:center;
}

.word{
background:#66c8d0;
margin:5px;
padding:10px 16px;
border-radius:6px;
cursor:pointer;
user-select:none;
color:#fff;
font-weight:bold;
font-size:18px;
}

#successModal{
display:none;
position:fixed;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:#fff;
border-radius:15px;
padding:30px;
box-shadow:0 10px 30px rgba(0,0,0,0.5);
z-index:1000;
width:350px;
text-align:center;
font-size:18px;
font-weight:bold;
}

canvas{
position:fixed;
top:0;
left:0;
width:100%;
height:100%;
pointer-events:none;
}

#finalSentence{
display:none;
margin-top:30px;
font-size:28px;
font-weight:bold;
background:#66c8d0;
color:white;
padding:20px;
border-radius:12px;
}
</style>
</head>

<body>

<h1>Vagyok, aki vagyok. De ki vagyok?</h1>
<p>Írd be a neveket és a könyveket, amikben szerepelnek!</p>

<h2>Szavak ablakai:</h2>
<div class="grid" id="game"></div>

<h2>Kirakott mondat:</h2>
<p>Rakd sorrendbe a szavakat!</p>
<div id="sentence"></div>

<div id="finalSentence"></div>

<button id="checkBtn">ELLENŐRZÉS</button>

<div id="successModal">🎉 Játszva megoldottátok! 🎉</div>

<canvas id="confetti"></canvas>

<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>

<script>
let words=[
{guess:"sámson",reveal:"Három"},
{guess:"judit",reveal:"kaput"},
{guess:"józsef",reveal:"őriznek"},
{guess:"betsabe",reveal:"rettenetes"},
{guess:"jónás",reveal:"szörnyek"},
{guess:"salome",reveal:"Jussatok"},
{guess:"teremtés",reveal:"át"},
{guess:"sámuel",reveal:"rajtuk"},
{guess:"máté",reveal:"s"},
{guess:"judit",reveal:"tiétek"},
{guess:"jónás",reveal:"a"},
{guess:"bírák",reveal:"kincsek"}
];

const correctOrder=["Három","kaput","őriznek","rettenetes","szörnyek","Jussatok","át","rajtuk","s","tiétek","a","kincsek"];

const container=document.getElementById("game");
const sentence=document.getElementById("sentence");
const checkBtn=document.getElementById("checkBtn");
const successModal=document.getElementById("successModal");
const finalSentence=document.getElementById("finalSentence");

let draggedWord=null;

// Kártyák létrehozása
for(let i=0;i<12;i++){
const card=document.createElement("div");
card.className="card";

card.innerHTML=`
<input placeholder="Írd ide">
<br>
<button>OK</button>
<div class="hidden-word"></div>
`;

container.appendChild(card);
}

const cards=document.querySelectorAll(".card");

// Kártya logika: szó bevitele
cards.forEach((card,index)=>{
const input=card.querySelector("input");
const btn=card.querySelector("button");
const hiddenDiv=card.querySelector(".hidden-word");

function checkWord(){
const val=input.value.toLowerCase().trim();
const wordIndex=words.findIndex(w=>w.guess===val);

if(wordIndex!==-1){
hiddenDiv.textContent=words[wordIndex].reveal;
hiddenDiv.classList.add("word");
hiddenDiv.setAttribute("draggable","true");
card.classList.add("open");
input.style.display="none";
btn.style.display="none";
words.splice(wordIndex,1);

// Következő mezőre ugrás
const nextCard=cards[index+1];
if(nextCard){
const nextInput=nextCard.querySelector("input");
if(nextInput) nextInput.focus();
}

}else{
alert("Whoopsie Daisy! Ez most nem lett jó.");
}
}

btn.addEventListener("click",checkWord);

// ENTER kezelés
input.addEventListener("keydown",function(e){
if(e.key==="Enter") checkWord();
});
});

// Ellenőrzés gomb
checkBtn.addEventListener("click",()=>{
const sentenceWords=Array.from(sentence.children).map(w=>w.textContent.trim());

if(sentenceWords.length!==correctOrder.length){
alert("Még nincs minden szó a mondatban!");
return;
}

let correct=true;
for(let i=0;i<correctOrder.length;i++){
if(sentenceWords[i]!==correctOrder[i]){
correct=false;
break;
}
}

if(correct){
sentence.style.display="none";
finalSentence.innerText=correctOrder.join(" ");
finalSentence.style.display="block";

const myConfetti=confetti.create(document.getElementById('confetti'),{resize:true,useWorker:true});

for(let i=0;i<5;i++){
myConfetti({particleCount:200,spread:160,origin:{y:0}});
}

successModal.style.display="block";

}else{
alert("Hmm... valami még nem jó.");
}
});

/* DRAG START */
document.addEventListener("dragstart",function(e){
if(!e.target.classList.contains("word")) return;
draggedWord=e.target;
});

/* MONDAT DROP TERÜLET */
sentence.addEventListener("dragover", function(e){ e.preventDefault(); });

sentence.addEventListener("drop", function(e){
e.preventDefault();
if(!draggedWord) return;

// Ha drop target maga a mondat, a végére kerül
if(e.target === sentence){
    sentence.appendChild(draggedWord);
} else if(e.target.classList.contains("word")){
    // Ha egy szó fölé dobjuk, beszúrjuk elé
    sentence.insertBefore(draggedWord, e.target);
}

draggedWord = null;
});

/* KÁRTYÁK DROP TERÜLET */
cards.forEach(card=>{
card.addEventListener("dragover", function(e){ e.preventDefault(); });
card.addEventListener("drop", function(e){
e.preventDefault();
if(!draggedWord) return;

const hidden = card.querySelector(".hidden-word");

// Ha üres vagy ugyanaz a szó visszakerül
if(hidden.textContent.trim() === "" || hidden.textContent === draggedWord.textContent){
    hidden.textContent = draggedWord.textContent;
    hidden.classList.add("word");
    hidden.setAttribute("draggable","true");

    // Ha a szó a mondatban volt, eltávolítjuk onnan
    if(draggedWord.parentElement === sentence){
        draggedWord.remove();
    }

    draggedWord = null;
}
});
});
</script>

</body>
</html>
