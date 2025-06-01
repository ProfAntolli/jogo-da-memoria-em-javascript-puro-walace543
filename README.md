/jogo-memoria/
  ├── index.html
  ├── style.css
  └── script.js
  └── /img/
       ├── img1.png
       ├── img2.png
       └── ...
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Jogo da Memória</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>🎴 Jogo da Memória 🎴</h1>
  <div class="game-board"></div>
  <button onclick="reiniciarJogo()">🔄 Reiniciar</button>
  
  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  background: #202020;
  color: #fff;
  text-align: center;
  margin: 0;
  padding: 20px;
}

h1 {
  margin-bottom: 20px;
}

.game-board {
  display: grid;
  grid-template-columns: repeat(4, 100px);
  grid-gap: 15px;
  justify-content: center;
  margin: 20px auto;
}

.card {
  width: 100px;
  height: 100px;
  perspective: 600px;
  cursor: pointer;
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 0.5s;
}

.card.flip .card-inner {
  transform: rotateY(180deg);
}

.card-front, .card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 8px;
  backface-visibility: hidden;
}

.card-front {
  background: #333;
}

.card-back {
  transform: rotateY(180deg);
  background-size: cover;
  background-position: center;
}

button {
  padding: 10px 20px;
  background: #ff4081;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
const imagens = [
  'img/img1.png',
  'img/img2.png',
  'img/img3.png',
  'img/img4.png',
  'img/img5.png',
  'img/img6.png',
  'img/img7.png',
  'img/img8.png'
];

let cartasViradas = [];
let travarJogo = false;

function embaralhar(array) {
  return array.concat(array).sort(() => Math.random() - 0.5);
}

function criarTabuleiro() {
  const tabuleiro = document.querySelector('.game-board');
  tabuleiro.innerHTML = '';
  const cartas = embaralhar(imagens);

  cartas.forEach(src => {
    const card = document.createElement('div');
    card.classList.add('card');
    card.innerHTML = `
      <div class="card-inner">
        <div class="card-front"></div>
        <div class="card-back" style="background-image: url(${src})"></div>
      </div>
    `;
    card.addEventListener('click', virarCarta);
    tabuleiro.appendChild(card);
  });
}

function virarCarta() {
  if (travarJogo) return;
  if (this.classList.contains('flip')) return;

  this.classList.add('flip');
  cartasViradas.push(this);

  if (cartasViradas.length === 2) {
    checarPar();
  }
}

function checarPar() {
  const [carta1, carta2] = cartasViradas;
  const img1 = carta1.querySelector('.card-back').style.backgroundImage;
  const img2 = carta2.querySelector('.card-back').style.backgroundImage;

  if (img1 === img2) {
    cartasViradas = [];
  } else {
    travarJogo = true;
    setTimeout(() => {
      carta1.classList.remove('flip');
      carta2.classList.remove('flip');
      cartasViradas = [];
      travarJogo = false;
    }, 1000);
  }
}

function reiniciarJogo() {
  cartasViradas = [];
  travarJogo = false;
  criarTabuleiro();
}

criarTabuleiro();
