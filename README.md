<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Planilha de Estudos da Júlia</title>

<style>
body{
    margin:0;
    font-family:Arial, sans-serif;
    background:linear-gradient(180deg,#ffe6f2,#e6f7ff);
    display:flex;
    flex-direction:column;
    height:100vh;
    overflow:hidden;
}

header{
    background:linear-gradient(90deg,#ff66b2,#66ccff);
    color:white;
    padding:15px;
    text-align:center;
    font-size:20px;
    font-weight:bold;
    position:relative;
}

header span{
    display:block;
    font-size:13px;
    font-weight:normal;
}

#notesBtn{
    position:absolute;
    right:15px;
    top:15px;
    background:white;
    color:#ff66b2;
    border:none;
    padding:6px 10px;
    border-radius:8px;
    cursor:pointer;
    font-size:14px;
}

.main{
    flex:1;
    display:flex;
    justify-content:center;
    align-items:center;
}

.card{
    background:white;
    width:90%;
    max-width:600px;
    padding:25px;
    border-radius:20px;
    box-shadow:0 10px 25px rgba(0,0,0,0.15);
    text-align:center;
    transition:transform 0.4s ease, opacity 0.4s ease;
}

.option{
    display:block;
    background:#f2f2f2;
    padding:12px;
    border-radius:10px;
    margin:10px 0;
    cursor:pointer;
    transition:0.3s;
}

.option:hover{
    background:#d9f2ff;
}

.correct{
    background:#c8f7c5 !important;
}

.wrong{
    background:#ffc4c4 !important;
}

#notesTab{
    position:fixed;
    right:0;
    top:0;
    height:100%;
    width:0;
    background:white;
    box-shadow:-5px 0 15px rgba(0,0,0,0.2);
    overflow:hidden;
    transition:0.4s;
    padding:20px;
}

#notesTab textarea{
    width:100%;
    height:85%;
    border-radius:8px;
    padding:10px;
    resize:none;
}
</style>
</head>

<body>

<header>
Planilha de Estudos da Júlia
<span>Hugo te ama 💕</span>
<button id="notesBtn" onclick="abrirNotas()">📝</button>
</header>

<div class="main">
<div class="card" id="card"></div>
</div>

<div id="notesTab">
<h3>Anotações</h3>
<textarea id="notesArea" placeholder="Escreva suas anotações..."></textarea>
<button onclick="fecharNotas()">Fechar</button>
</div>

<script>
let perguntas = [

{
materia:"Linguagens",
pergunta:"Um anúncio publicitário afirma: 'Beba água, seu corpo agradece'. O recurso de linguagem predominante é:",
opcoes:["Metáfora","Personificação","Hipérbole","Ironia","Antítese"],
resposta:1,
explicacao:"Há atribuição de ação humana ao corpo, caracterizando personificação."
},

{
materia:"Matemática",
pergunta:"Uma loja oferece 20% de desconto em um produto que custa R$ 250. O valor final será:",
opcoes:["R$ 200","R$ 210","R$ 220","R$ 230","R$ 240"],
resposta:0,
explicacao:"20% de 250 é 50. 250 - 50 = 200."
},

{
materia:"Ciências Humanas",
pergunta:"A urbanização acelerada no Brasil durante o século XX ocorreu principalmente devido:",
opcoes:["À expansão agrícola","À industrialização","À diminuição populacional","À migração internacional","À reforma agrária"],
resposta:1,
explicacao:"A industrialização atraiu população para os centros urbanos."
},

{
materia:"Ciências da Natureza",
pergunta:"O aumento da concentração de CO₂ na atmosfera contribui para:",
opcoes:["Resfriamento global","Diminuição da pressão atmosférica","Efeito estufa intensificado","Formação de terremotos","Redução da umidade"],
resposta:2,
explicacao:"O CO₂ intensifica o efeito estufa."
},

{
materia:"Matemática",
pergunta:"Um reservatório de 500 litros está com 60% de sua capacidade ocupada. Quantos litros há nele?",
opcoes:["200","250","300","350","400"],
resposta:2,
explicacao:"60% de 500 = 300 litros."
},

{
materia:"Linguagens",
pergunta:"No trecho 'O silêncio gritava na sala', ocorre:",
opcoes:["Metáfora","Paradoxo","Ironia","Eufemismo","Comparação"],
resposta:1,
explicacao:"Silêncio não pode gritar; há contradição aparente."
},

{
materia:"Ciências Humanas",
pergunta:"A globalização intensificou principalmente:",
opcoes:["Isolamento cultural","Trocas econômicas internacionais","Fim das tecnologias","Desaparecimento das fronteiras físicas","Redução do comércio"],
resposta:1,
explicacao:"A globalização ampliou as trocas econômicas."
},

{
materia:"Ciências da Natureza",
pergunta:"A fotossíntese é importante porque:",
opcoes:["Produz gás carbônico","Consome oxigênio","Produz glicose e oxigênio","Destrói energia solar","Elimina água"],
resposta:2,
explicacao:"Plantas produzem glicose e liberam oxigênio."
},

{
materia:"Matemática",
pergunta:"A média aritmética dos números 6, 8 e 10 é:",
opcoes:["7","8","9","10","12"],
resposta:1,
explicacao:"(6+8+10)/3 = 24/3 = 8."
},

{
materia:"Ciências Humanas",
pergunta:"A democracia caracteriza-se principalmente por:",
opcoes:["Centralização do poder","Participação popular nas decisões","Governo militar","Ausência de leis","Monarquia absoluta"],
resposta:1,
explicacao:"Democracia envolve participação popular."
}

];

let atual=0;
let startX=0;

function carregar(){
let q=perguntas[atual];
document.getElementById("card").innerHTML=`
<h3>${q.materia}</h3>
<p>${q.pergunta}</p>
${q.opcoes.map((op,i)=>`<div class="option" onclick="responder(${i})">${op}</div>`).join("")}
`;
}

function responder(i){
let q=perguntas[atual];
let op=document.querySelectorAll(".option");

if(i===q.resposta){
op[i].classList.add("correct");
}else{
op[i].classList.add("wrong");
op[q.resposta].classList.add("correct");
}

setTimeout(()=>{
proximaPergunta();
},1200);
}

function proximaPergunta(){
let card=document.getElementById("card");
card.style.opacity="0";
card.style.transform="translateX(-50px)";

setTimeout(()=>{
atual++;
if(atual<perguntas.length){
carregar();
card.style.opacity="1";
card.style.transform="translateX(0)";
}else{
card.innerHTML="<h3>Simulado Finalizado 💖</h3>";
}
},400);
}

function abrirNotas(){
document.getElementById("notesTab").style.width="300px";
}

function fecharNotas(){
document.getElementById("notesTab").style.width="0";
}

document.getElementById("notesArea").addEventListener("input",function(){
localStorage.setItem("anotacoes",this.value);
});

window.onload=function(){
carregar();
document.getElementById("notesArea").value=localStorage.getItem("anotacoes")||"";
};

document.addEventListener("touchstart",function(e){
startX=e.touches[0].clientX;
});

document.addEventListener("touchend",function(e){
let endX=e.changedTouches[0].clientX;
if(startX-endX>50){
proximaPergunta();
}
});
</script>

</body>
</html>
