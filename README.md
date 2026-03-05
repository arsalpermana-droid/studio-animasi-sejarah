# studio-animasi-sejarah
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Studio Animasi Sejarah: AI Prompt Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        .chart-container { 
            position: relative; 
            width: 100%; 
            max-width: 800px; 
            margin-left: auto; 
            margin-right: auto; 
            height: 300px; 
            max-height: 400px; 
        }
        @media (min-width: 768px) { 
            .chart-container { height: 350px; } 
        }
        body {
            background-color: #faf8f5;
            color: #292524;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .glass-panel {
            background: rgba(255, 255, 255, 0.7);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(231, 229, 228, 0.5);
        }
        /* Custom scrollbar for output area */
        .output-scroll::-webkit-scrollbar {
            width: 8px;
        }
        .output-scroll::-webkit-scrollbar-track {
            background: #f5f5f4; 
            border-radius: 4px;
        }
        .output-scroll::-webkit-scrollbar-thumb {
            background: #d6d3d1; 
            border-radius: 4px;
        }
        .output-scroll::-webkit-scrollbar-thumb:hover {
            background: #a8a29e; 
        }
    </style>
</head>
<body class="antialiased min-h-screen flex flex-col">

    <!-- Chosen Palette: Warm Historical Neutrals (Stone-50 background, Stone-800 text, Amber-700/600 accents for a subtle vintage/historical feel without being overly dark) -->
    <!-- Application Structure Plan: The SPA is structured as an interactive "Studio Dashboard". Instead of just reading a static prompt template, users interact with it as a functional tool. 
         - Top: Introduction and Context (setting the stage).
         - Left Column (Input): A form to collect the 6 key variables from the source report (Judul, Tokoh, Tahun, Lokasi, Konflik, Moral).
         - Right Column Top (Analysis): A Chart.js visualization showing the narrative tension arc based on the 10-scene structure described in the report, helping users understand WHY the prompt is structured that way.
         - Right Column Bottom (Output): A real-time generated "Mega-Prompt" that combines the user's inputs with the strict instructions (like Facial Consistency) from the source report, ready to be copied. This transforms a reading task into an interactive creation task. -->
    <!-- Visualization & Content Choices: 
         - Report Info: 10 Scene Structure (Hook, Challenge, Climax, Resolution, Ending). -> Goal: Organize & Change. -> Viz: Line Chart (Chart.js). -> Interaction: Hover tooltips explaining each phase. -> Justification: Visually demonstrates the emotional pacing of the animation script requested in the prompt.
         - Report Info: Output Requirements (T2I, T2V, Audio). -> Goal: Inform & Act. -> Viz: Dynamic Text Area. -> Interaction: Auto-updates as user types, one-click copy button. -> Justification: The core utility of the source document is to create a prompt; an interactive builder is the ultimate expression of this. Confirming NO SVG/Mermaid used. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <header class="bg-stone-900 text-stone-50 py-6 px-4 md:px-8 shadow-md border-b-4 border-amber-700">
        <div class="max-w-6xl mx-auto flex flex-col md:flex-row items-center justify-between">
            <div>
                <h1 class="text-3xl font-bold tracking-tight">🎬 Studio Animasi Sejarah</h1>
                <p class="text-stone-400 mt-1 text-sm md:text-base">AI Mega-Prompt Generator untuk Midjourney, Runway, Pika & Sora</p>
            </div>
            <div class="mt-4 md:mt-0 text-amber-500 text-4xl">
                🏛️
            </div>
        </div>
    </header>

    <main class="flex-grow max-w-7xl mx-auto w-full p-4 md:p-8 grid grid-cols-1 lg:grid-cols-12 gap-8">

        <section class="lg:col-span-4 flex flex-col gap-6">
            <div class="glass-panel p-6 rounded-2xl shadow-sm">
                <h2 class="text-xl font-bold mb-2 text-amber-800 border-b-2 border-stone-200 pb-2">📋 Instruksi Sutradara</h2>
                <p class="text-stone-600 text-sm mb-4">
                    Aplikasi ini menerjemahkan rancangan produksi Anda menjadi instruksi komprehensif untuk AI. Masukkan detail cerita sejarah Anda di bawah ini, dan sistem akan merakit <strong>Mega-Prompt</strong> yang mencakup Sinopsis, 10 Scene Berurutan, Prompt Text-to-Image (dengan Konsistensi Wajah), Text-to-Video, dan Naskah.
                </p>

                <form id="promptForm" class="flex flex-col gap-4">
                    <div>
                        <label for="judul" class="block text-sm font-semibold text-stone-700 mb-1">Judul Cerita 🏷️</label>
                        <input type="text" id="judul" class="w-full p-2 border border-stone-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-amber-500 outline-none transition-all" placeholder="Contoh: Sumpah di Bawah Cahaya Purnama" value="Sumpah di Bawah Cahaya Purnama">
                    </div>
                    <div>
                        <label for="tokoh" class="block text-sm font-semibold text-stone-700 mb-1">Tokoh Utama 👤</label>
                        <textarea id="tokoh" rows="2" class="w-full p-2 border border-stone-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-amber-500 outline-none transition-all text-sm" placeholder="Contoh: Gajah Mada, pria 30 tahun, rahang tegas...">Gajah Mada, pria 30 tahun, rahang tegas, tatapan tajam, rambut gondrong diikat ke atas</textarea>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label for="tahun" class="block text-sm font-semibold text-stone-700 mb-1">Tahun/Era ⏳</label>
                            <input type="text" id="tahun" class="w-full p-2 border border-stone-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-amber-500 outline-none transition-all text-sm" placeholder="1336 M, Majapahit" value="Tahun 1336 Masehi, Era Majapahit">
                        </div>
                        <div>
                            <label for="lokasi" class="block text-sm font-semibold text-stone-700 mb-1">Lokasi 📍</label>
                            <input type="text" id="lokasi" class="w-full p-2 border border-stone-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-amber-500 outline-none transition-all text-sm" placeholder="Istana Trowulan" value="Istana Trowulan dan pesisir Nusantara">
                        </div>
                    </div>
                    <div>
                        <label for="konflik" class="block text-sm font-semibold text-stone-700 mb-1">Konflik Utama ⚔️</label>
                        <textarea id="konflik" rows="2" class="w-full p-2 border border-stone-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-amber-500 outline-none transition-all text-sm" placeholder="Jelaskan tantangan utama tokoh...">Gajah Mada harus meyakinkan para menteri yang meragukan Sumpah Palapa-nya</textarea>
                    </div>
                    <div>
                        <label for="moral" class="block text-sm font-semibold text-stone-700 mb-1">Nilai Moral 🕊️</label>
                        <input type="text" id="moral" class="w-full p-2 border border-stone-300 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-amber-500 outline-none transition-all text-sm" placeholder="Pesan yang ingin disampaikan..." value="Keteguhan hati dan visi besar membutuhkan pengorbanan">
                    </div>
                </form>
            </div>
            
            <div class="bg-amber-100 border border-amber-300 rounded-2xl p-4 shadow-sm">
                <h3 class="font-bold text-amber-800 text-sm mb-1">⚠️ Mode Konsistensi Wajah Aktif</h3>
                <p class="text-xs text-amber-700">Generator ini secara otomatis memasukkan parameter <strong>STRICT FACIAL CONSISTENCY</strong> ke dalam prompt gambar untuk memastikan identitas karakter sejarah Anda tidak berubah antar scene.</p>
            </div>
        </section>

        <section class="lg:col-span-8 flex flex-col gap-6">
            <div class="glass-panel p-6 rounded-2xl shadow-sm">
                <div class="mb-4">
                    <h2 class="text-xl font-bold text-stone-800">📈 Analisis Struktur Naratif (10 Scene)</h2>
                    <p class="text-sm text-stone-500">Visualisasi kurva ketegangan emosional berdasarkan arsitektur cerita yang disyaratkan.</p>
                </div>
                <div class="chart-container">
                    <canvas id="narrativeChart"></canvas>
                </div>
                <div class="mt-4 grid grid-cols-2 md:grid-cols-5 gap-2 text-center text-xs text-stone-600">
                    <div class="bg-stone-100 p-2 rounded"><strong>Sc 1-2</strong><br>Hook / Pengenalan</div>
                    <div class="bg-stone-100 p-2 rounded"><strong>Sc 3-4</strong><br>Tantangan Awal</div>
                    <div class="bg-stone-100 p-2 rounded border-b-2 border-amber-500"><strong>Sc 5-7</strong><br>Puncak Konflik</div>
                    <div class="bg-stone-100 p-2 rounded"><strong>Sc 8-9</strong><br>Resolusi</div>
                    <div class="bg-stone-100 p-2 rounded"><strong>Sc 10</strong><br>Penutup (Moral)</div>
                </div>
            </div>

            <div class="glass-panel p-6 rounded-2xl shadow-sm flex-grow flex flex-col h-full">
                <div class="flex justify-between items-center mb-4 border-b-2 border-stone-200 pb-2">
                    <div>
                        <h2 class="text-xl font-bold text-stone-800">✨ Mega-Prompt Final</h2>
                        <p class="text-sm text-stone-500">Salin teks di bawah ini ke AI Text-Generation Anda.</p>
                    </div>
                    <button id="copyBtn" class="bg-stone-800 hover:bg-amber-600 text-white font-bold py-2 px-4 rounded-lg transition-colors flex items-center gap-2 text-sm shadow-md">
                        <span>📋</span> Salin Prompt
                    </button>
                </div>
                
                <div class="bg-stone-800 rounded-xl p-1 flex-grow">
                    <textarea id="outputArea" class="w-full h-full min-h-[400px] bg-stone-900 text-stone-200 p-4 rounded-lg font-mono text-sm outline-none resize-none output-scroll" readonly></textarea>
                </div>
            </div>
        </section>

    </main>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const formElements = {
                judul: document.getElementById('judul'),
                tokoh: document.getElementById('tokoh'),
                tahun: document.getElementById('tahun'),
                lokasi: document.getElementById('lokasi'),
                konflik: document.getElementById('konflik'),
                moral: document.getElementById('moral')
            };
            const outputArea = document.getElementById('outputArea');
            const copyBtn = document.getElementById('copyBtn');

            function generatePrompt() {
                const data = {
                    judul: formElements.judul.value || "[Judul Belum Diisi]",
                    tokoh: formElements.tokoh.value || "[Tokoh Belum Diisi]",
                    tahun: formElements.tahun.value || "[Tahun Belum Diisi]",
                    lokasi: formElements.lokasi.value || "[Lokasi Belum Diisi]",
                    konflik: formElements.konflik.value || "[Konflik Belum Diisi]",
                    moral: formElements.moral.value || "[Moral Belum Diisi]"
                };

                const template = `Bertindaklah sebagai Sutradara Animasi Sejarah, Penulis Skenario ahli, dan AI Prompt Engineer tingkat lanjut (Midjourney, Runway, Pika, Sora). Tugas Anda adalah membuat rancangan produksi animasi sejarah yang lengkap, imersif, dan siap dieksekusi.

# INPUT DATA
Gunakan informasi berikut sebagai dasar cerita:
- Judul cerita     : ${data.judul}
- Tokoh utama      : ${data.tokoh}
- Tahun kejadian   : ${data.tahun}
- Lokasi           : ${data.lokasi}
- Konflik utama    : ${data.konflik}
- Nilai moral      : ${data.moral}

# OUTPUT REQUIREMENTS
Berikan output dengan struktur bernomor berikut ini:

## 1. Sinopsis
Buat sinopsis cerita yang dramatis, menggugah emosi, dan akurat secara historis (atau sesuai konteks era) dalam 2-3 paragraf.

## 2. 10 Scene Berurutan
Pecah cerita menjadi 10 adegan (scene) berurutan. Gunakan struktur naratif ini:
- Scene 1-2: Pengenalan tokoh, era, dan lokasi (Hook).
- Scene 3-4: Munculnya tantangan awal.
- Scene 5-7: Puncak konflik (Climax build-up).
- Scene 8-9: Resolusi / penyelesaian konflik.
- Scene 10: Penutup dan penyampaian nilai moral (Ending).
Tuliskan deskripsi visual singkat (1-2 kalimat) untuk setiap scene.

## 3. Prompt Text-to-Image (T2I) Tiap Scene
Buat prompt gambar untuk ke-10 scene di atas.
*Aturan Prompt T2I:*
- Tulis prompt dalam **Bahasa Inggris**.
- **CRITICAL - STRICT FACIAL CONSISTENCY MODE:** Pada setiap prompt yang menampilkan tokoh utama, cantumkan: "Identical facial features, consistent character identity, core facial structure remains unchanged".
- Format prompt: [Shot type], [Character details & facial consistency], [Action/Pose], [Setting/Background/Year details], [Lighting], [Cinematic style, historical animation style, 8k, highly detailed].

## 4. Prompt Text-to-Video (T2V) 6 Detik Tiap Scene
Buat prompt video untuk menganimasikan gambar dari Scene 1 hingga 10.
*Aturan Prompt T2V:*
- Tulis prompt dalam **Bahasa Inggris**.
- Fokus pada *motion* (pergerakan) selama 6 detik.
- Format prompt: [Camera movement], [Subtle character motion], [Atmospheric effects], [Length: 6 seconds], [Maintain strict facial consistency].

## 5. Narasi + Dialog Konsisten
Buat naskah Voice Over (VO) atau Dialog (Scene 1 - 10).
*Aturan Naskah:*
- Tulis dalam Bahasa Indonesia dengan gaya bahasa yang sesuai zaman/era kejadian.
- Durasi dialog maksimal 1-2 kalimat pendek per scene agar sesuai dengan durasi 6 detik.`;

                outputArea.value = template;
            }

            // Event Listeners for form inputs
            Object.values(formElements).forEach(el => {
                el.addEventListener('input', generatePrompt);
            });

            // Initial generation
            generatePrompt();

            // Copy Button Logic
            copyBtn.addEventListener('click', () => {
                outputArea.select();
                document.execCommand('copy');
                
                const originalText = copyBtn.innerHTML;
                copyBtn.innerHTML = '<span>✅</span> Tersalin!';
                copyBtn.classList.remove('bg-stone-800');
                copyBtn.classList.add('bg-green-600');
                
                setTimeout(() => {
                    copyBtn.innerHTML = originalText;
                    copyBtn.classList.add('bg-stone-800');
                    copyBtn.classList.remove('bg-green-600');
                }, 2000);
            });

            // Chart.js Setup
            const ctx = document.getElementById('narrativeChart').getContext('2d');
            
            // Gradient for the line chart
            const gradient = ctx.createLinearGradient(0, 0, 0, 300);
            gradient.addColorStop(0, 'rgba(217, 119, 6, 0.5)'); // amber-600
            gradient.addColorStop(1, 'rgba(217, 119, 6, 0.0)');

            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: ['Sc 1', 'Sc 2', 'Sc 3', 'Sc 4', 'Sc 5', 'Sc 6', 'Sc 7', 'Sc 8', 'Sc 9', 'Sc 10'],
                    datasets: [{
                        label: 'Tingkat Ketegangan Emosional / Konflik',
                        data: [1, 2, 4, 5, 7, 9, 10, 6, 3, 1],
                        borderColor: '#d97706', // amber-600
                        backgroundColor: gradient,
                        borderWidth: 3,
                        pointBackgroundColor: '#292524', // stone-800
                        pointBorderColor: '#fff',
                        pointBorderWidth: 2,
                        pointRadius: 5,
                        pointHoverRadius: 7,
                        fill: true,
                        tension: 0.4 // Smooth curves
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false },
                        tooltip: {
                            backgroundColor: 'rgba(41, 37, 36, 0.9)',
                            titleFont: { size: 14, family: 'Segoe UI' },
                            bodyFont: { size: 13, family: 'Segoe UI' },
                            padding: 10,
                            displayColors: false,
                            callbacks: {
                                title: function(context) {
                                    return context[0].label;
                                },
                                label: function(context) {
                                    const index = context.dataIndex;
                                    const phases = [
                                        "Hook: Pengenalan dunia & tokoh",
                                        "Hook: Setup situasi normal",
                                        "Tantangan: Insiden memicu konflik",
                                        "Tantangan: Rintangan pertama",
                                        "Climax Build-up: Tekanan meningkat",
                                        "Climax Build-up: Titik krisis",
                                        "Puncak Konflik: Pertaruhan tertinggi",
                                        "Resolusi: Penyelesaian masalah",
                                        "Resolusi: Dampak dari puncak konflik",
                                        "Ending: Pesan moral & penutup"
                                    ];
                                    return phases[index];
                                }
                            }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 11,
                            ticks: { display: false },
                            grid: { color: 'rgba(231, 229, 228, 0.5)', drawBorder: false }
                        },
                        x: {
                            grid: { display: false },
                            ticks: { color: '#57534e', font: { weight: 'bold' } }
                        }
                    },
                    interaction: {
                        mode: 'index',
                        intersect: false,
                    },
                }
            });
        });
    </script>
<script>
async function generatePrompt() {
  const judul = document.getElementById("judul").value;
  const tokoh = document.getElementById("tokoh").value;
  const tahun = document.getElementById("tahun").value;
  const konflik = document.getElementById("konflik").value;

  const output = document.getElementById("output");

  output.innerText = "Generating...";

  const apiKey = "PASTE_API_KEY_KAMU_DI_SINI";

  const response = await fetch(
    "https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=" + apiKey,
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        contents: [{
          parts: [{
            text: `Buatkan cerita animasi sejarah dengan struktur 10 scene lengkap dengan hook 3 detik pertama, dialog konsisten, dan narasi sinematik.
            
Judul: ${judul}
Tokoh: ${tokoh}
Tahun: ${tahun}
Konflik: ${konflik}`
          }]
        }]
      }),
    }
  );

  const data = await response.json();
  output.innerText = data.candidates[0].content.parts[0].text;
}
</script></body>
</html>
