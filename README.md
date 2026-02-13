<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Will You Be My Valentine? 🖤</title>

<style>
* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: system-ui, sans-serif;
  color: white;
  background: black;
  overflow-x: hidden;
}

.background {
  position: fixed;
  inset: 0;
  z-index: -1;
  background-size: cover;
  background-position: center;
  filter: brightness(0.35);
  transition: background-image 1s ease-in-out;
}

main {
  padding: 2rem;
  text-align: center;
}

button {
  padding: 0.7rem 1.2rem;
  border-radius: 10px;
  border: none;
  background: #ff4d6d;
  color: white;
  cursor: pointer;
  margin: 0.5rem;
}

.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 1rem;
  margin-top: 2rem;
}

.gallery img {
  width: 100%;
  border-radius: 12px;
  cursor: pointer;
  transition: transform 0.3s;
}

.gallery img:hover {
  transform: scale(1.05);
}

.modal {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.9);
  display: none;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal img {
  max-width: 90%;
  border-radius: 12px;
}

video {
  max-width: 90%;
  border-radius: 12px;
  display: none;
}

input[type="range"] {
  width: 80%;
}
</style>
</head>

<body>

<div class="background" id="bg"></div>

<main>
  <h1 id="question"></h1>
  <div id="buttons"></div>

  <div id="meterBox" style="display:none;">
    <p id="meterText"></p>
    <input type="range" min="0" max="6000" value="100" id="meter">
    <br>
    <button id="nextBtn">Next 🖤</button>
  </div>

  <video id="video" controls></video>

  <div class="gallery" id="gallery"></div>
</main>

<div class="modal" id="modal">
  <img id="modalImg">
</div>

<audio id="music" src="audio/bg.mp3" loop></audio>

<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
<script>
/* ---------- CONFIG ---------- */
const images = Array.from({length:10},(_,i)=>`images/${i+1}.jpg`);
const videos = ["videos/answer1.mp4","videos/answer2.mp4","videos/answer3.mp4"];
const bgMusic = document.getElementById("music");

/* ---------- BACKGROUND SLIDESHOW ---------- */
let bgIndex = 0;
const bg = document.getElementById("bg");
setInterval(()=>{
  bg.style.backgroundImage = `url(${images[bgIndex]})`;
  bgIndex = (bgIndex+1)%images.length;
},5000);

/* ---------- GALLERY ---------- */
const gallery = document.getElementById("gallery");
const modal = document.getElementById("modal");
const modalImg = document.getElementById("modalImg");

images.forEach(src=>{
  const img = document.createElement("img");
  img.src = src;
  img.loading = "lazy";
  img.onclick = ()=>{
    modal.style.display="flex";
    modalImg.src = src;
  };
  gallery.appendChild(img);
});
modal.onclick = ()=>modal.style.display="none";

/* ---------- QUESTIONS ---------- */
const q = document.getElementById("question");
const btns = document.getElementById("buttons");
const meterBox = document.getElementById("meterBox");
const meter = document.getElementById("meter");
const meterText = document.getElementById("meterText");
const nextBtn = document.getElementById("nextBtn");
const video = document.getElementById("video");

function dodge(btn){
  btn.style.position="absolute";
  btn.style.left=Math.random()*80+"%";
  btn.style.top=Math.random()*80+"%";
}

function playVideo(src,cb){
  video.src = src;
  video.style.display="block";
  bgMusic.pause();
  video.play();
  video.onended=()=>{
    video.style.display="none";
    bgMusic.play();
    cb && cb();
  };
}

function stage1(){
  q.textContent="Am I the love of your life?";
  btns.innerHTML=`<button id="yes">Yes 🖤</button><button id="no">No</button>`;
  document.getElementById("yes").onclick=()=>{
    alert("SHISHEMEEEEEEE ❤️");
    playVideo(videos[0],stage2);
  };
  document.getElementById("no").onmouseover=e=>dodge(e.target);
}

function stage2(){
  btns.innerHTML="";
  meterBox.style.display="block";
  meter.oninput=()=>{
    const v=meter.value;
    meterText.textContent = v>5000?"That’s crazy love 😭🖤":v>1000?"I feel it 🖤":"Hmm… try harder 😌";
  };
  nextBtn.onclick=()=>playVideo(videos[1],stage3);
}

function stage3(){
  meterBox.style.display="none";
  q.textContent="Will you be my Valentine? 🖤";
  btns.innerHTML=`<button id="yes">Yes 🖤</button><button id="no">No</button>`;
  document.getElementById("yes").onclick=()=>{
    playVideo(videos[2],()=>{
      confetti({particleCount:250,spread:100});
      q.textContent="I’m the luckiest person alive 🖤";
      btns.innerHTML="";
    });
  };
  document.getElementById("no").onmouseover=e=>dodge(e.target);
}

/* ---------- START ---------- */
bg.style.backgroundImage = `url(${images[0]})`;
stage1();
document.body.onclick=()=>bgMusic.play();
</script>

</body>
</html>
