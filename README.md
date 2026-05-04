<!DOCTYPE html>
<html lang="uz">  
<head>  
<meta charset="UTF-8" /> 
<title>Uyga vazifa yordamchisi</title> 
<style>
  body {  
    font-family: Arial, sans-serif; 
    padding: 20px;
    background: #f0f4f8; 
  }
  h1 {
    text-align: center;
  }
  .container {
    max-width: 600px; 
    margin: auto;
    background: white;
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }
  select, input, button {
    font-size: 16px;
    padding: 8px;
    margin: 10px 0;
    width: 100%;
    border-radius: 6px;
    border: 1px solid #ccc;
  }
  button {
    background: #0077cc;
    color: white;
    border: none;
    cursor: pointer;
  }
  button:hover {
    background: #005fa3;
  }
  #output {
    background: #e7f0fd;
    padding: 15px;
    margin-top: 20px;
    border-radius: 6px;
    font-family: monospace;
    white-space: pre-wrap;
  }
  #imagePreview {
    margin-top: 20px;
    text-align: center;
  }
  #imagePreview img {
    max-width: 100%;
    max-height: 300px;
    border-radius: 10px;
  }
</style>
</head>
<body>

<h1>Uyga vazifa yordamchisi</h1>

<div class="container">
  <label for="problemType">Masala turini tanlang:</label>
  <select id="problemType">
    <option value="simple">Oddiy kvadrat tenglama ildizi</option>
    <option value="complex">Murakkab kvadrat tenglama misoli</option>
  </select>

  <label for="equationInput">Tenglamani kiriting (kvadrat shakl: ax^2 + bx + c = 0):</label>
  <input type="text" id="equationInput" placeholder="masalan, 1x^2 - 3x + 2 = 0" />

  <button onclick="solveEquation()">Hisoblash</button>

  <div id="output"></div>

  <!-- Rasm yuklash -->
  <hr>
  <label>Rasm tanlang:</label>
  <input type="file" id="imageInput" accept="image/*" />

  <button onclick="uploadImage()">Yuklash</button>

  <div id="imagePreview"></div>
</div>

<script>
// Kvadrat tenglama parseri (oddiy)
function parseQuadratic(equation) {
  const eq = equation.replace(/\s+/g, '').replace(/-/g, '+-').toLowerCase();

  let a = 0, b = 0, c = 0;
  const regexA = /([+-]?[\d\.]*)x\^2/;
  const regexB = /([+-]?[\d\.]*)x(?!\^)/;
  const regexC = /([+-]?[\d\.]+)(?![x])/g;

  let matchA = eq.match(regexA);
  if (matchA) {
    a = parseFloat(matchA[1])  (matchA[1] === '-' ? -1 : 1);
  }

  let matchB = eq.match(regexB);
  if (matchB) {
    b = parseFloat(matchB[1])  (matchB[1] === '-' ? -1 : 1);
  }

  let constants = [];
  let m;
  while ((m = regexC.exec(eq)) !== null) {
    if (!m[0].includes('x')) {
      constants.push(parseFloat(m[1]));
    }
  }
  if(constants.length) c = constants.reduce((acc,val)=>acc+val,0);

  return { a, b, c };
}

function solveQuadratic(a, b, c) {
  if (a === 0) return "Bu kvadrat tenglama emas (a=0).";

  const d = b*b - 4*a*c;
  let output = Tenglama: ${a}x² + ${b}x + ${c} = 0\nDiskriminant (D): ${d}\n;

  if (d > 0) {
    const root1 = ((-b + Math.sqrt(d)) / (2*a)).toFixed(5);
    const root2 = ((-b - Math.sqrt(d)) / (2*a)).toFixed(5);
    output += Ikkita haqiqiy ildiz:\n x₁ = ${root1}\n x₂ = ${root2};
  } else if (d === 0) {
    const root = (-b/(2*a)).toFixed(5);
    output += Bitta haqiqiy ildiz:\n x = ${root};
  } else {
    const realPart = (-b/(2*a)).toFixed(5);
    const imagPart = (Math.sqrt(-d)/(2*a)).toFixed(5);
    output += Murakkab ildizlar:\n x₁ = ${realPart} + ${imagPart}i\n x₂ = ${realPart} - ${imagPart}i;
  }
  return output;
}

function solveEquation() {
  const type = document.getElementById('problemType').value;
  let inputEq = document.getElementById('equationInput').value.trim();

  let output = '';

  if(type === 'simple') {
    if (!inputEq) {
      inputEq = "1x^2 - 3x + 2 = 0";
      document.getElementById('equationInput').value = inputEq;
    }
    const {a,b,c} = parseQuadratic(inputEq);
    output = solveQuadratic(a,b,c);

  } else if (type === 'complex') {
    output = Murakkab misol:\nTenglama:\n2x² - 4x + 5 = 0\n\n;
    output += solveQuadratic(2, -4, 5);
  }

  document.getElementById('output').innerText = output;
}
// Rasm yuklash funksiyasi
function uploadImage() {
  const input = document.getElementById('imageInput');
  const preview = document.getElementById('imagePreview');

  if (input.files && input.files[0]) {
    const reader = new FileReader();

    reader.onload = function(e) {
      preview.innerHTML = <img src="${e.target.result}" alt="Yuklangan rasm">;
    };

    reader.readAsDataURL(input.files[0]);
  } else {
    alert("Iltimos, rasmni tanlang.");
  }
}
</script>

</body>
</html>
