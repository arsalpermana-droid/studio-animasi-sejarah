<!DOCTYPE html>
<html>
<head>
  <title>Studio Animasi Sejarah</title>
  <style>
    body {
      font-family: Arial;
      padding: 20px;
      background: #111;
      color: white;
    }
    input, textarea {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 8px;
      border: none;
    }
    button {
      padding: 12px;
      background: orange;
      border: none;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
    }
    pre {
      background: #222;
      padding: 15px;
      border-radius: 8px;
      white-space: pre-wrap;
    }
  </style>
</head>
<body>

<h2>🎬 Generator Prompt Cerita Animasi Sejarah</h2>

<input type="text" id="judul" placeholder="Judul Cerita">
<input type="text" id="tokoh" placeholder="Tokoh Utama">
<input type="text" id="tahun" placeholder="Tahun / Era">
<input type="text" id="konflik" placeholder="Konflik Utama">

<button onclick="generatePrompt()">Generate Prompt</button>

<h3>📜 Hasil Prompt:</h3>
<pre id="output"></pre>

<script>
function generatePrompt() {
  const judul = document.getElementById("judul").value;
  const tokoh = document.getElementById("tokoh").value;
  const tahun = document.getElementById("tahun").value;
  const konflik = document.getElementById("konflik").value;

  const prompt = `
Buatkan cerita animasi sejarah dengan detail berikut:

Judul: ${judul}
Tokoh utama: ${tokoh}
Tahun/era: ${tahun}
Konflik utama: ${konflik}

Format:
- 10 scene bersambung
- Hook 3 detik pertama
- Narasi sinematik
- Dialog konsisten
- Cocok untuk video 6 detik per scene
- Gaya animasi 3D cinematic seperti film Pixar
`;

  document.getElementById("output").innerText = prompt;
}
</script>

</body>
</html>
