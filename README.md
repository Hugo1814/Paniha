<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Planilha de Estudo da Júlia</title>

<style>
body{
  margin:0;
  font-family:'Segoe UI', sans-serif;
  background: linear-gradient(135deg,#ffd6ec,#d6ecff);
  display:flex;
  justify-content:center;
}

.container{
  width:90%;
  max-width:900px;
  background:white;
  margin:30px 0;
  padding:25px;
  border-radius:15px;
  box-shadow:0 10px 30px rgba(0,0,0,0.1);
}

h1{
  text-align:center;
  color:#ff4fa3;
}

.filtros{
  display:flex;
  gap:10px;
  justify-content:center;
  margin-bottom:20px;
  flex-wrap:wrap;
}

select,button{
  padding:10px;
  border-radius:8px;
  border:1px solid #ccc;
  cursor:pointer;
}

button{
  background:#4fa3ff;
  color:white;
  border:none;
}

.questao{
  margin-bottom:20px;
  padding:15px;
  background:#f9f9f9;
  border-radius:10px;
}

.opcao{
  display:block;
  margin:5px 0;
}

.resultado{
  font-weight:bold;
  text-align:center;
  margin-top:15px;
  font-size:18px;
}
.correta{color:green}
.errada{color:red}
</style>
</head>

<body>

<div class="container">

<h1>📘 Planilha de Estudo da Júlia</h1>

<div class="filtros">
<select id="ano">
<option value="todos">Ano</option>
<option value="2010">2010</option>
<option value="2015">2015</option>
<option value="2020">2020</option>
<option value="2023">2023</option>
</select>

<select id="materia">
<option value="todos">Matéria</option>
<option value="Matemática">Matemática</option>
<option value="Português">Português</option>
<option value="Biologia">Biologia</option>
<option value="História">História</option>
</select>

<button onclick="gerarSimulado()">Gerar Simulado</button>
</div>

<div id="prova"></div>

<button onclick="corrigir()">Finalizar</button>

<div class="resultado" id="resultado"></div>

</div>

<script>

// 🔥 Estrutura pronta para +1000 questões
const banco = [

{ano:"2010", materia:"Matemática",
pergunta:"Quanto é 10% de 200?",
opcoes:["10","20","30","40","50"],
resposta:1,
explicacao:"10% de 200 = 20."
},

{ano:"2015", materia:"Português",
pergunta:"Hipérbole significa:",
opcoes:["Exagero","Comparação","Oposição","Ironia","Metáfora"],
resposta:0,
explicacao:"Hipérbole é exagero."
},

{ano:"2020", materia:"Biologia",
pergunta:"A fotossíntese ocorre no:",
opcoes:["Núcleo","Mitocôndria","Cloroplasto","Ribossomo","Lisossomo"],
resposta:2,
explicacao:"O cloroplasto realiza a fotossíntese."
},

{ano:"2023", materia:"História",
pergunta:"A Constituição de 1988 é chamada de:",
opcoes:["Militar","Cidadã","Imperial","Nova República","Carta Magna"],
resposta:1,
explicacao:"É conhecida como Constituição Cidadã."
}

];

let simulado=[];

function gerarSimulado(){
  const anoFiltro=document.getElementById("ano").value;
  const materiaFiltro=document.getElementById("materia").value;

  let filtradas=banco.filter(q=>
    (anoFiltro==="todos"||q.ano===anoFiltro) &&
    (materiaFiltro==="todos"||q.materia===materiaFiltro)
  );

  simulado=filtradas.sort(()=>0.5-Math.random()).slice(0,5);

  const provaDiv=document.getElementById("prova");
  provaDiv.innerHTML="";
  document.getElementById("resultado").innerHTML="";

  simulado.forEach((q,i)=>{
    const div=document.createElement("div");
    div.className="questao";
    div.innerHTML=`<p>${i+1}) ${q.pergunta}</p>`;

    q.opcoes.forEach((op,index)=>{
      div.innerHTML+=`
      <label class="opcao">
      <input type="radio" name="q${i}" value="${index}">
      ${op}
      </label>`;
    });

    div.innerHTML+=`<div id="exp${i}"></div>`;
    provaDiv.appendChild(div);
  });
}

function corrigir(){
  let acertos=0;

  simulado.forEach((q,i)=>{
    const marcada=document.querySelector(`input[name="q${i}"]:checked`);
    const exp=document.getElementById(`exp${i}`);

    if(marcada){
      if(parseInt(marcada.value)===q.resposta){
        acertos++;
        exp.innerHTML="✔ Correto! "+q.explicacao;
        exp.className="correta";
      }else{
        exp.innerHTML="✘ Errado! "+q.explicacao;
        exp.className="errada";
      }
    }
  });

  document.getElementById("resultado").innerHTML=
  `Você acertou ${acertos} de ${simulado.length}`;
}

</script>

</body>
</html>
