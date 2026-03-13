<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>漢字ホームランゲーム</title>

<style>

body{
font-family:sans-serif;
text-align:center;
background:#f4f8ff;
}

h1{
font-size:36px;
}

#question{
font-size:24px;
margin:20px;
}

.grid{
display:grid;
grid-template-columns:repeat(3,120px);
gap:10px;
justify-content:center;
margin:20px;
}

.cell{
background:white;
border:2px solid #333;
font-size:40px;
height:100px;
display:flex;
align-items:center;
justify-content:center;
cursor:pointer;
}

.cell:hover{
background:#dbe9ff;
}

.correct{
background:#7cff8a;
}

.wrong{
background:#ff9a9a;
}

button{
font-size:18px;
margin:5px;
padding:8px 15px;
}

</style>
</head>

<body>

<h1>⚾ 漢字ホームランゲーム</h1>

学年
<select id="grade">
<option value="1">1年</option>
<option value="2">2年</option>
<option value="3">3年</option>
<option value="4">4年</option>
<option value="5">5年</option>
<option value="6">6年</option>
</select>

モード
<select id="mode">
<option value="kanji">漢字当て</option>
<option value="reading">読み当て</option>
</select>

<br><br>

<button onclick="startGame()">試合開始</button>

<div id="question"></div>

<div class="grid" id="grid"></div>

<h2 id="result"></h2>
<h3 id="score"></h3>

<script>

const kanjiData = {

1:["山","川","空","花","木","石","犬","森","月"],
2:["海","風","雪","星","船","朝","鳥","魚","電"],
3:["駅","橋","湖","旅","祭","島","農","館","港"],
4:["愛","勇","鏡","競","願","芸","象","焼","働"],
5:["夢","護","導","燃","築","銅","独","境","貿"],
6:["難","優","論","憲","糖","臓","郵","模","訳"]

}

const readingData = {

山:"やま",
川:"かわ",
空:"そら",
花:"はな",
木:"き",
石:"いし",
犬:"いぬ",
森:"もり",
月:"つき",

海:"うみ",
風:"かぜ",
雪:"ゆき",
星:"ほし",
船:"ふね",
朝:"あさ",
鳥:"とり",
魚:"さかな",
電:"でん",

駅:"えき",
橋:"はし",
湖:"みずうみ",
旅:"たび",
祭:"まつり",
島:"しま",
農:"のう",
館:"かん",
港:"みなと",

愛:"あい",
勇:"ゆう",
鏡:"かがみ",
競:"きょう",
願:"ねが",
芸:"げい",
象:"ぞう",
焼:"や",
働:"はたら",

夢:"ゆめ",
護:"ご",
導:"どう",
燃:"も",
築:"きず",
銅:"どう",
独:"どく",
境:"きょう",
貿:"ぼう",

難:"なん",
優:"ゆう",
論:"ろん",
憲:"けん",
糖:"とう",
臓:"ぞう",
郵:"ゆう",
模:"も",
訳:"やく"

}

let answer=""
let bases=0

function startGame(){

const grade=document.getElementById("grade").value
const mode=document.getElementById("mode").value

const list=kanjiData[grade]

answer=list[Math.floor(Math.random()*list.length)]

let question=""

if(mode=="kanji"){
question="この文の<strong>？？</strong>に入る漢字は？<br>"
question+="わたしは <b>？？</b> を見に行きました。"
}else{
question="この漢字の読みは？<br><b>"+answer+"</b>"
}

document.getElementById("question").innerHTML=question

createGrid(mode)
}

function createGrid(mode){

const grid=document.getElementById("grid")
grid.innerHTML=""

let choices=[]

if(mode=="kanji"){

const grade=document.getElementById("grade").value
choices=[...kanjiData[grade]]

}else{

choices=Object.values(readingData)

}

choices=choices.sort(()=>Math.random()-0.5).slice(0,8)

if(mode=="kanji"){
choices.push(answer)
}else{
choices.push(readingData[answer])
}

choices=choices.sort(()=>Math.random()-0.5)

choices.forEach(c=>{

const div=document.createElement("div")
div.className="cell"
div.textContent=c

div.onclick=()=>checkAnswer(c,div)

grid.appendChild(div)

})

}

function checkAnswer(choice,cell){

const mode=document.getElementById("mode").value

let correct=false

if(mode=="kanji" && choice==answer) correct=true
if(mode=="reading" && choice==readingData[answer]) correct=true

if(correct){

cell.classList.add("correct")

bases++

if(bases>=3){
document.getElementById("result").innerText="🎉 ホームラン！"
bases=0
}else{
document.getElementById("result").innerText="ヒット！"
}

}else{

cell.classList.add("wrong")
document.getElementById("result").innerText="アウト！"

}

document.getElementById("score").innerText="ランナー: "+bases+" 塁"

setTimeout(startGame,1200)

}

</script>

</body>
</html>
