<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Planilha de Estudos da Júlia</title>

<style>
body{
    margin:0;
    font-family: Arial, sans-serif;
    background: linear-gradient(180deg,#ffe6f2,#e6f7ff);
    display:flex;
    flex-direction:column;
    height:100vh;
}

header{
    background: linear-gradient(90deg,#ff66b2,#66ccff);
    color:white;
    padding:15px;
    text-align:center;
    font-size:22px;
    font-weight:bold;
    position:relative;
}

header span{
    display:block;
    font-size:14px;
    font-weight:normal;
    margin-top:5px;
    animation: brilho 2s infinite alternate;
}

@keyframes brilho{
    from{opacity:0.6;}
    to{opacity:1;}
}

.main{
    flex:1;
    display:flex;
    justify-content:center;
    align-items:center;
    position:relative;
}

.card{
    background:white;
    width:90%;
    max-width:600px;
    padding:25px;
    border-radius:15px;
    box-shadow:0 8px 20px rgba(0,0,0,0.15);
    text-align:center;
}

.option{
    display:block;
    background:#f2f2f2;
    padding:12px;
    border-radius:8px;
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

button{
    background:#ff66b2;
    color:white;
    border:none;
    padding:10px;
    border-radius:8px;
    width:100%;
    font-weight:bold;
    cursor:pointer;
    margin-top:10px;
}

button:hover{
    background:#ff3385;
}

#score{
    font-weight:bold;
    margin-bottom:10px;
}

/* Aba de anotações */
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
    height:80%;
    border-radius:8px;
    padding:10px;
    resize:none;
    font-size:14px;
}

#openNotes{
    position:fixed;
    right:10px;
    bottom:20px;
    background:#66ccff;
    border:none;
    color:white;
    padding:15px;
    border-radius:50%;
    font-size:18px;
    cursor:pointer;
    box-shadow:0 5px 15px rgba(0,0,0,0.3);
}
</style>
</head>

<body>

<header>
Planilha de Estudos da Júlia
<span>Hugo te ama 💕</span>
</header>

<div class="main">
<div class="card">
<div id="score">Pontuação: 0</div>
<div id="quiz"></div>
</div>
</div>

<div id="notesTab">
<h3>Anotações</h3>
<textarea id="notesArea" placeholder="Escreva suas anotações aqui..."></textarea>
<button onclick="fecharNotas()">Fechar</button>
</div>

<button id="openNotes" onclick="abrirNotas()">📝</button>

<script>
let perguntas = [
{
materia:"Matemática",
pergunta:"Uma escola recebeu 240 livros para distribuir igualmente entre 8 turmas. Quantos livros cada turma recebeu?",
opcoes:["20","25","30","35"],
resposta:2,
explicacao:"240 ÷ 8 = 30 livros por turma."
}
];

let atual=0;
let pontos=0;
let selecionado=null;

function carregar(){
let q=perguntas[atual];
document.getElementById("quiz").innerHTML=`
<h3>${q.materia}</h3>
<p>${q.pergunta}</p>
${q.opcoes.map((op,i)=>`<div class="option" onclick="selecionar(${i})">${op}</div>`).join("")}
<button onclick="verificar()">Verificar</button>
<p id="feedback"></p>
`;
}

function selecionar(i){
selecionado=i;
let op=document.querySelectorAll(".option");
op.forEach(o=>o.style.border="none");
op[i].style.border="2px solid #ff66b2";
}

function verificar(){
let q=perguntas[atual];
let op=document.querySelectorAll(".option");
let fb=document.getElementById("feedback");

if(selecionado===null){
fb.innerHTML="⚠️ Selecione uma opção!";
return;
}

if(selecionado===q.resposta){
op[selecionado].classList.add("correct");
fb.innerHTML="✅ Correto! "+q.explicacao;
pontos++;
}else{
op[selecionado].classList.add("wrong");
op[q.resposta].classList.add("correct");
fb.innerHTML="❌ Incorreto. "+q.explicacao;
}

document.getElementById("score").innerHTML="Pontuação: "+pontos;
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
}
</script>

</body>
</html>
