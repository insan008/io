<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>NewsCard Studio Pro - Bangla & Modern Editorial Generator</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Google Fonts: Top Bangla & Modern Editorial Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Anek+Bangla:wght@600;700;800&family=Hind+Siliguri:wght@500;600;700&family=Inter:wght@400;600;800;900&family=Noto+Serif+Bengali:wght@600;700;900&family=Playfair+Display:ital,wght@0,700;0,900;1,600&family=Syne:wght@700;800&display=swap" rel="stylesheet">
  <!-- Client-side In-Browser AI Background Remover -->
  <script src="https://cdn.jsdelivr.net/npm/@imgly/background-removal@1.4.5/dist/bundle.js"></script>
  
  <style>
    body { font-family: 'Hind Siliguri', 'Inter', sans-serif; }
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    /* Drag over animated highlight */
    .drag-active {
      outline: 4px dashed #ef4444 !important;
      outline-offset: -8px;
    }
  </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col select-none">

  <!-- TOP NAVBAR -->
  <header class="border-b border-slate-800 bg-slate-900/90 backdrop-blur px-6 py-3 flex items-center justify-between sticky top-0 z-50">
    <div class="flex items-center gap-3">
      <div class="w-9 h-9 rounded-lg bg-red-600 flex items-center justify-center font-black text-white text-base shadow-lg shadow-red-600/30">
        NC
      </div>
      <div>
        <h1 class="text-base font-bold flex items-center gap-2">
          NewsCard <span class="text-red-500">Studio Pro</span>
          <span class="text-[10px] bg-red-500/20 text-red-400 border border-red-500/30 px-1.5 py-0.5 rounded font-mono">Drag & Logo v2.5</span>
        </h1>
        <p class="text-[11px] text-slate-400">Bangla & English News Card Generator</p>
      </div>
    </div>

    <!-- Aspect Ratio Switcher -->
    <div class="flex items-center gap-1 bg-slate-800/80 p-1 rounded-lg border border-slate-700/60 text-xs">
      <button onclick="setRatio('1:1')" id="btn-1-1" class="ratio-btn px-3 py-1.5 rounded-md font-semibold bg-red-600 text-white transition">1:1 Square</button>
      <button onclick="setRatio('4:5')" id="btn-4-5" class="ratio-btn px-3 py-1.5 rounded-md font-semibold text-slate-400 hover:text-white transition">4:5 Portrait</button>
      <button onclick="setRatio('16:9')" id="btn-16-9" class="ratio-btn px-3 py-1.5 rounded-md font-semibold text-slate-400 hover:text-white transition">16:9 Landscape</button>
    </div>

    <!-- Export Action -->
    <button onclick="downloadCard()" class="flex items-center gap-2 bg-gradient-to-r from-red-600 to-rose-600 hover:from-red-500 hover:to-rose-500 text-white font-bold px-4 py-2 rounded-lg text-sm transition shadow-lg shadow-red-600/30">
      <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path></svg>
      Export 4K Card
    </button>
  </header>

  <!-- MAIN APP CONTAINER -->
  <div class="flex-1 flex flex-col lg:flex-row overflow-hidden">
    
    <!-- LEFT PANEL: CONTROLS & LOGO -->
    <aside class="w-full lg:w-[480px] border-r border-slate-800 bg-slate-900/50 p-5 overflow-y-auto max-h-[calc(100vh-65px)] no-scrollbar space-y-5">
      
      <!-- 1. CUSTOM LOGO UPLOAD -->
      <section class="bg-slate-900/90 p-4 rounded-xl border border-red-500/30 space-y-3">
        <div class="flex justify-between items-center">
          <h2 class="text-xs font-bold uppercase tracking-wider text-red-400 flex items-center gap-1.5">
            🏷️ Channel / Media Logo
          </h2>
          <button id="btn-remove-logo" onclick="removeLogo()" class="text-[10px] text-slate-400 hover:text-rose-400 underline hidden">Remove Logo</button>
        </div>

        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Upload Transparent PNG Logo</label>
          <input type="file" id="logoInput" accept="image/png,image/webp,image/svg+xml" class="block w-full text-xs text-slate-400 file:mr-2 file:py-1.5 file:px-3 file:rounded-md file:border-0 file:text-xs file:font-semibold file:bg-red-600/20 file:text-red-300 hover:file:bg-red-600/30 cursor-pointer"/>
        </div>

        <div id="logo-controls" class="grid grid-cols-2 gap-2 pt-1 hidden">
          <div>
            <label class="text-[10px] text-slate-400 block mb-1">Logo Position</label>
            <select id="logoPos" onchange="render()" class="w-full bg-slate-950 border border-slate-800 rounded p-1.5 text-xs outline-none">
              <option value="top-left" selected>Top Left</option>
              <option value="top-right">Top Right</option>
              <option value="bottom-right">Footer Right</option>
            </select>
          </div>
          <div>
            <label class="text-[10px] text-slate-400 block mb-1">Logo Size (<span id="val-logo-size">55px</span>)</label>
            <input type="range" id="logoSize" min="30" max="120" value="55" oninput="updateVal('logo-size', this.value+'px'); render()" class="w-full mt-1.5 accent-red-500">
          </div>
        </div>
      </section>

      <!-- 2. AI TOOLS -->
      <section class="bg-gradient-to-br from-indigo-950/40 via-slate-900 to-slate-900 p-4 rounded-xl border border-indigo-900/50 space-y-3">
        <div class="flex justify-between items-center">
          <h2 class="text-xs font-bold uppercase tracking-wider text-indigo-400 flex items-center gap-1.5">
            ✨ AI Photo Tools
          </h2>
          <span id="ai-status" class="text-[10px] text-slate-400 italic">Ready</span>
        </div>

        <div class="grid grid-cols-2 gap-2">
          <button onclick="applyAutoEnhance()" class="flex items-center justify-center gap-2 bg-indigo-600/30 hover:bg-indigo-600/50 border border-indigo-500/50 text-indigo-200 text-xs py-2 px-3 rounded-lg font-semibold transition active:scale-95">
            🪄 AI Auto-Correction
          </button>
          
          <button onclick="triggerRemoveBg()" id="btn-remove-bg" class="flex items-center justify-center gap-2 bg-rose-600/30 hover:bg-rose-600/50 border border-rose-500/50 text-rose-200 text-xs py-2 px-3 rounded-lg font-semibold transition active:scale-95">
            ✂️ AI Remove BG
          </button>
        </div>

        <!-- Color Presets -->
        <div>
          <label class="text-[10px] text-slate-400 block mb-1">Color Grading Filter</label>
          <div class="grid grid-cols-4 gap-1.5 text-[11px]">
            <button onclick="setLUT('normal')" class="bg-slate-800 hover:bg-slate-700 py-1 rounded text-slate-300">Natural</button>
            <button onclick="setLUT('breaking')" class="bg-slate-800 hover:bg-slate-700 py-1 rounded text-red-400 font-semibold">News Red</button>
            <button onclick="setLUT('cinema')" class="bg-slate-800 hover:bg-slate-700 py-1 rounded text-amber-300 font-semibold">Cinema</button>
            <button onclick="setLUT('noir')" class="bg-slate-800 hover:bg-slate-700 py-1 rounded text-slate-400">B&W</button>
          </div>
        </div>
      </section>

      <!-- 3. PHOTO UPLOAD & POSITIONING -->
      <section class="bg-slate-900/80 p-4 rounded-xl border border-slate-800 space-y-3">
        <div class="flex justify-between items-center">
          <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400">Photo Layers & Positioning</h2>
          <span class="text-[10px] text-amber-400">💡 Drag directly on frame</span>
        </div>
        
        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Background Image (or Drag & Drop onto Frame)</label>
          <input type="file" id="imageInput" accept="image/*" class="block w-full text-xs text-slate-400 file:mr-2 file:py-1.5 file:px-3 file:rounded-md file:border-0 file:text-xs file:font-semibold file:bg-slate-800 file:text-slate-200 hover:file:bg-slate-700 cursor-pointer"/>
        </div>

        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Subject Cutout Layer (Optional Person PNG)</label>
          <input type="file" id="subjectInput" accept="image/png,image/webp" class="block w-full text-xs text-slate-400 file:mr-2 file:py-1.5 file:px-3 file:rounded-md file:border-0 file:text-xs file:font-semibold file:bg-slate-800 file:text-slate-200 hover:file:bg-slate-700 cursor-pointer"/>
        </div>

        <div class="grid grid-cols-3 gap-2 pt-1 text-xs">
          <div>
            <label class="text-[10px] text-slate-400">Zoom</label>
            <input type="range" id="zoom" min="1" max="2.5" step="0.05" value="1" oninput="render()" class="w-full accent-red-500">
          </div>
          <div>
            <label class="text-[10px] text-slate-400">Pan X</label>
            <input type="range" id="panX" min="-600" max="600" step="2" value="0" oninput="render()" class="w-full accent-red-500">
          </div>
          <div>
            <label class="text-[10px] text-slate-400">Pan Y</label>
            <input type="range" id="panY" min="-600" max="600" step="2" value="0" oninput="render()" class="w-full accent-red-500">
          </div>
        </div>
      </section>

      <!-- 4. MANUAL ADJUSTMENTS -->
      <section class="bg-slate-900/80 p-4 rounded-xl border border-slate-800 space-y-3">
        <div class="flex justify-between items-center">
          <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400">Adjustments</h2>
          <button onclick="resetSliders()" class="text-[10px] text-slate-400 hover:text-white underline">Reset</button>
        </div>

        <div class="space-y-2 text-xs">
          <div class="grid grid-cols-2 gap-2">
            <div>
              <div class="flex justify-between text-[10px] text-slate-400 mb-0.5">
                <span>Brightness</span><span id="val-bright">100%</span>
              </div>
              <input type="range" id="brightness" min="40" max="180" value="100" oninput="updateVal('bright', this.value+'%'); render()" class="w-full accent-red-500">
            </div>
            <div>
              <div class="flex justify-between text-[10px] text-slate-400 mb-0.5">
                <span>Contrast</span><span id="val-contrast">105%</span>
              </div>
              <input type="range" id="contrast" min="50" max="200" value="105" oninput="updateVal('contrast', this.value+'%'); render()" class="w-full accent-red-500">
            </div>
          </div>

          <div class="grid grid-cols-2 gap-2">
            <div>
              <div class="flex justify-between text-[10px] text-slate-400 mb-0.5">
                <span>Saturation</span><span id="val-saturate">100%</span>
              </div>
              <input type="range" id="saturation" min="0" max="200" value="100" oninput="updateVal('saturate', this.value+'%'); render()" class="w-full accent-red-500">
            </div>
            <div>
              <div class="flex justify-between text-[10px] text-slate-400 mb-0.5">
                <span>Blur BG</span><span id="val-blur">0px</span>
              </div>
              <input type="range" id="blur" min="0" max="25" value="0" oninput="updateVal('blur', this.value+'px'); render()" class="w-full accent-red-500">
            </div>
          </div>

          <div class="pt-1">
            <div class="flex justify-between text-[10px] text-slate-400 mb-0.5">
              <span>Text Legibility Gradient Overlay</span><span id="val-darkness">85%</span>
            </div>
            <input type="range" id="darkness" min="30" max="100" value="85" oninput="updateVal('darkness', this.value+'%'); render()" class="w-full accent-red-500">
          </div>
        </div>
      </section>

      <!-- 5. BANGLA / EDITORIAL TYPOGRAPHY -->
      <section class="bg-slate-900/80 p-4 rounded-xl border border-slate-800 space-y-3">
        <div class="flex justify-between items-center">
          <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400">Editorial Typography</h2>
          <button onclick="setSample('bn')" class="text-[10px] text-red-400 hover:underline">Bangla Sample</button>
        </div>

        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Font Family</label>
          <select id="fontStyle" onchange="render()" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-xs outline-none">
            <optgroup label="Bangla Fonts">
              <option value="'Hind Siliguri', sans-serif" selected>হিন্দ শিলিগুড়ি (Hind Siliguri - Editorial)</option>
              <option value="'Anek Bangla', sans-serif">অনেক বাংলা (Anek Bangla - Bold News)</option>
              <option value="'Noto Serif Bengali', serif">নোটো সেরিফ (Noto Serif - Classic)</option>
            </optgroup>
            <optgroup label="English Fonts">
              <option value="'Inter', sans-serif">Inter (Modern Clean)</option>
              <option value="'Syne', sans-serif">Syne (Heavy Display)</option>
              <option value="'Playfair Display', serif">Playfair Display (Luxury Editorial)</option>
            </optgroup>
          </select>
        </div>

        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Badge Tag</label>
          <div class="flex gap-2">
            <input type="text" id="tagText" value="ব্রেকিং নিউজ" oninput="render()" class="flex-1 bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-xs focus:ring-1 focus:ring-red-500 outline-none font-bold text-red-400">
            <input type="color" id="tagBgColor" value="#dc2626" oninput="render()" class="w-9 h-8 p-1 bg-slate-950 border border-slate-800 rounded-lg cursor-pointer">
          </div>
        </div>

        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Headline</label>
          <textarea id="headlineText" rows="3" oninput="render()" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2.5 text-sm focus:ring-1 focus:ring-red-500 outline-none font-bold">আন্তর্জাতিক বাজারে রেকর্ড দরপতন, নতুন দাম নির্ধারণ করল কেন্দ্রীয় সংস্থা</textarea>
        </div>

        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Headline Size</label>
          <input type="range" id="headlineSize" min="36" max="76" value="54" oninput="render()" class="w-full accent-red-500">
        </div>

        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Sub-headline / Note</label>
          <input type="text" id="subText" value="গতকাল সন্ধ্যায় বিশেষ বৈঠকের পর মুখপাত্র এই তথ্য নিশ্চিত করেছেন।" oninput="render()" class="w-full bg-slate-950 border border-slate-800 rounded-lg px-3 py-1.5 text-xs outline-none text-slate-300">
        </div>
      </section>

      <!-- 6. BRANDING & FOOTER -->
      <section class="bg-slate-900/80 p-4 rounded-xl border border-slate-800 space-y-3">
        <h2 class="text-xs font-bold uppercase tracking-wider text-slate-400">Media Branding</h2>

        <div class="grid grid-cols-2 gap-2">
          <div>
            <label class="text-[11px] text-slate-400 block mb-1">Brand Name Text</label>
            <input type="text" id="brandName" value="প্রথম সংবাদ" oninput="render()" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-xs outline-none font-bold">
          </div>
          <div>
            <label class="text-[11px] text-slate-400 block mb-1">Social Handle</label>
            <input type="text" id="handleText" value="@prothomsangbad" oninput="render()" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-xs outline-none text-slate-400">
          </div>
        </div>

        <div>
          <label class="text-[11px] text-slate-400 block mb-1">Date Stamp</label>
          <input type="text" id="dateText" value="" oninput="render()" class="w-full bg-slate-950 border border-slate-800 rounded-lg p-2 text-xs outline-none text-slate-400">
        </div>
      </section>

    </aside>

    <!-- RIGHT PANEL: INTERACTIVE CANVAS WITH DRAG-AND-DROP -->
    <main class="flex-1 bg-slate-950/70 p-6 flex flex-col items-center justify-center relative overflow-hidden">
      
      <!-- Interactive Frame Drop Zone -->
      <div id="dropZone" class="relative shadow-2xl rounded-xl overflow-hidden border border-slate-800 bg-black flex items-center justify-center max-h-[85vh] max-w-[85vw] cursor-grab active:cursor-grabbing transition-all">
        <!-- Floating Drop Hint -->
        <div id="dropNotice" class="absolute inset-0 bg-red-950/80 backdrop-blur-sm border-2 border-dashed border-red-500 rounded-xl flex flex-col items-center justify-center z-50 pointer-events-none opacity-0 transition-opacity">
          <svg class="w-16 h-16 text-red-400 animate-bounce mb-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path></svg>
          <p class="text-white text-lg font-bold">Drop photo directly into card frame!</p>
        </div>

        <!-- The Canvas -->
        <canvas id="cardCanvas" class="max-h-[82vh] max-w-full w-auto h-auto object-contain"></canvas>
      </div>

      <div class="flex items-center gap-4 text-[11px] text-slate-400 mt-3 font-medium">
        <span>✨ <b>Drag & drop</b> image onto frame</span>
        <span>•</span>
        <span>🖱️ <b>Click & drag</b> on card to reposition photo</span>
      </div>
    </main>

  </div>

  <!-- SCRIPT LOGIC -->
  <script>
    const canvas = document.getElementById('cardCanvas');
    const ctx = canvas.getContext('2d');
    const dropZone = document.getElementById('dropZone');
    const dropNotice = document.getElementById('dropNotice');

    let canvasWidth = 1080;
    let canvasHeight = 1080;

    // Background Image
    let bgImage = new Image();
    bgImage.crossOrigin = "anonymous";
    bgImage.src = "https://images.unsplash.com/photo-1541872703-74c5e44368f9?w=1200&auto=format&fit=crop&q=80";

    // Cutout & Logo Image states
    let subjectImage = null;
    let logoImage = null;

    bgImage.onload = () => render();

    // Default Bangla Date
    const dateOptions = { day: 'numeric', month: 'long', year: 'numeric' };
    document.getElementById('dateText').value = new Date().toLocaleDateString('bn-BD', dateOptions);

    /* ========================================================
       1. DIRECT DRAG AND DROP ONTO FRAME
       ======================================================== */
    ['dragenter', 'dragover'].forEach(eventName => {
      dropZone.addEventListener(eventName, (e) => {
        e.preventDefault();
        e.stopPropagation();
        dropZone.classList.add('drag-active');
        dropNotice.classList.remove('opacity-0');
      }, false);
    });

    ['dragleave', 'drop'].forEach(eventName => {
      dropZone.addEventListener(eventName, (e) => {
        e.preventDefault();
        e.stopPropagation();
        dropZone.classList.remove('drag-active');
        dropNotice.classList.add('opacity-0');
      }, false);
    });

    dropZone.addEventListener('drop', (e) => {
      const dt = e.dataTransfer;
      const file = dt.files[0];
      if (file && file.type.startsWith('image/')) {
        const reader = new FileReader();
        reader.onload = (event) => {
          bgImage = new Image();
          bgImage.onload = () => render();
          bgImage.src = event.target.result;
        };
        reader.readAsDataURL(file);
      }
    });

    /* ========================================================
       2. CLICK & DRAG PHOTO TO MOVE / PAN DIRECTLY
       ======================================================== */
    let isDragging = false;
    let dragStartX = 0;
    let dragStartY = 0;
    let currentPanX = 0;
    let currentPanY = 0;

    canvas.addEventListener('mousedown', (e) => {
      isDragging = true;
      dragStartX = e.clientX;
      dragStartY = e.clientY;
      currentPanX = parseFloat(document.getElementById('panX').value);
      currentPanY = parseFloat(document.getElementById('panY').value);
    });

    window.addEventListener('mousemove', (e) => {
      if (!isDragging) return;
      const deltaX = (e.clientX - dragStartX) * (canvasWidth / canvas.clientWidth);
      const deltaY = (e.clientY - dragStartY) * (canvasHeight / canvas.clientHeight);

      const newPanX = Math.round(currentPanX + deltaX);
      const newPanY = Math.round(currentPanY + deltaY);

      document.getElementById('panX').value = newPanX;
      document.getElementById('panY').value = newPanY;
      render();
    });

    window.addEventListener('mouseup', () => { isDragging = false; });

    // Touch support for mobile devices
    canvas.addEventListener('touchstart', (e) => {
      if (e.touches.length === 1) {
        isDragging = true;
        dragStartX = e.touches[0].clientX;
        dragStartY = e.touches[0].clientY;
        currentPanX = parseFloat(document.getElementById('panX').value);
        currentPanY = parseFloat(document.getElementById('panY').value);
      }
    });

    window.addEventListener('touchmove', (e) => {
      if (!isDragging || e.touches.length !== 1) return;
      const deltaX = (e.touches[0].clientX - dragStartX) * (canvasWidth / canvas.clientWidth);
      const deltaY = (e.touches[0].clientY - dragStartY) * (canvasHeight / canvas.clientHeight);

      document.getElementById('panX').value = Math.round(currentPanX + deltaX);
      document.getElementById('panY').value = Math.round(currentPanY + deltaY);
      render();
    });

    window.addEventListener('touchend', () => { isDragging = false; });

    /* ========================================================
       3. CUSTOM LOGO UPLOAD & MANAGEMENT
       ======================================================== */
    document.getElementById('logoInput').addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (event) => {
          logoImage = new Image();
          logoImage.onload = () => {
            document.getElementById('logo-controls').classList.remove('hidden');
            document.getElementById('btn-remove-logo').classList.remove('hidden');
            render();
          };
          logoImage.src = event.target.result;
        };
        reader.readAsDataURL(file);
      }
    });

    function removeLogo() {
      logoImage = null;
      document.getElementById('logoInput').value = '';
      document.getElementById('logo-controls').classList.add('hidden');
      document.getElementById('btn-remove-logo').classList.add('hidden');
      render();
    }

    // Input handlers for file inputs
    document.getElementById('imageInput').addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (event) => {
          bgImage = new Image();
          bgImage.onload = () => render();
          bgImage.src = event.target.result;
        };
        reader.readAsDataURL(file);
      }
    });

    document.getElementById('subjectInput').addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (event) => {
          subjectImage = new Image();
          subjectImage.onload = () => render();
          subjectImage.src = event.target.result;
        };
        reader.readAsDataURL(file);
      }
    });

    // AI BG Removal
    async function triggerRemoveBg() {
      const status = document.getElementById('ai-status');
      const btn = document.getElementById('btn-remove-bg');
      status.innerText = "⏳ Removing BG...";
      status.className = "text-[10px] text-amber-400 font-semibold";
      btn.disabled = true;

      try {
        if (!window.imglyRemoveBackground) {
          alert("AI Model is still initializing. Please wait 5 seconds.");
          return;
        }
        const blob = await window.imglyRemoveBackground(bgImage);
        subjectImage = new Image();
        subjectImage.onload = () => {
          status.innerText = "✅ BG Removed!";
          status.className = "text-[10px] text-emerald-400 font-semibold";
          btn.disabled = false;
          render();
        };
        subjectImage.src = URL.createObjectURL(blob);
      } catch (err) {
        status.innerText = "⚠️ Failed (Use transparent PNG)";
        status.className = "text-[10px] text-rose-400";
        btn.disabled = false;
      }
    }

    function applyAutoEnhance() {
      document.getElementById('brightness').value = 108;
      document.getElementById('contrast').value = 118;
      document.getElementById('saturation').value = 125;
      document.getElementById('darkness').value = 88;
      updateVal('bright', '108%');
      updateVal('contrast', '118%');
      updateVal('saturate', '125%');
      updateVal('darkness', '88%');
      document.getElementById('ai-status').innerText = "✨ AI Enhanced!";
      render();
    }

    function setLUT(type) {
      if (type === 'normal') {
        document.getElementById('brightness').value = 100;
        document.getElementById('contrast').value = 105;
        document.getElementById('saturation').value = 100;
      } else if (type === 'breaking') {
        document.getElementById('brightness').value = 95;
        document.getElementById('contrast').value = 125;
        document.getElementById('saturation').value = 140;
      } else if (type === 'cinema') {
        document.getElementById('brightness').value = 92;
        document.getElementById('contrast').value = 115;
        document.getElementById('saturation').value = 85;
      } else if (type === 'noir') {
        document.getElementById('brightness').value = 105;
        document.getElementById('contrast').value = 135;
        document.getElementById('saturation').value = 0;
      }
      render();
    }

    function setRatio(ratio) {
      document.querySelectorAll('.ratio-btn').forEach(btn => {
        btn.classList.remove('bg-red-600', 'text-white');
        btn.classList.add('text-slate-400');
      });

      if (ratio === '1:1') {
        canvasWidth = 1080; canvasHeight = 1080;
        document.getElementById('btn-1-1').classList.add('bg-red-600', 'text-white');
      } else if (ratio === '4:5') {
        canvasWidth = 1080; canvasHeight = 1350;
        document.getElementById('btn-4-5').classList.add('bg-red-600', 'text-white');
      } else if (ratio === '16:9') {
        canvasWidth = 1280; canvasHeight = 720;
        document.getElementById('btn-16-9').classList.add('bg-red-600', 'text-white');
      }
      render();
    }

    function updateVal(id, val) {
      document.getElementById(`val-${id}`).innerText = val;
    }

    function resetSliders() {
      document.getElementById('brightness').value = 100;
      document.getElementById('contrast').value = 105;
      document.getElementById('saturation').value = 100;
      document.getElementById('blur').value = 0;
      document.getElementById('darkness').value = 85;
      document.getElementById('zoom').value = 1;
      document.getElementById('panX').value = 0;
      document.getElementById('panY').value = 0;
      render();
    }

    function setSample(lang) {
      if (lang === 'bn') {
        document.getElementById('tagText').value = "ব্রেকিং নিউজ";
        document.getElementById('headlineText').value = "আন্তর্জাতিক বাজারে কমেছে জ্বালানির দাম, নতুন সিদ্ধান্তের ঘোষণা";
        document.getElementById('subText').value = "মন্ত্রণালয়ের এক প্রজ্ঞাপনে বিষয়টি নিশ্চিত করা হয়েছে।";
        document.getElementById('brandName').value = "প্রথম সংবাদ";
        document.getElementById('fontStyle').value = "'Hind Siliguri', sans-serif";
      }
      render();
    }

    /* ========================================================
       4. CANVAS RENDERING ENGINE
       ======================================================== */
    function render() {
      canvas.width = canvasWidth;
      canvas.height = canvasHeight;

      const bright = document.getElementById('brightness').value;
      const contrast = document.getElementById('contrast').value;
      const saturate = document.getElementById('saturation').value;
      const blur = document.getElementById('blur').value;
      const zoom = parseFloat(document.getElementById('zoom').value);
      const panX = parseFloat(document.getElementById('panX').value);
      const panY = parseFloat(document.getElementById('panY').value);

      // Layer 1: Background Image
      ctx.save();
      ctx.filter = `brightness(${bright}%) contrast(${contrast}%) saturate(${saturate}%) blur(${blur}px)`;

      const imgRatio = bgImage.width / bgImage.height;
      const cRatio = canvasWidth / canvasHeight;
      let drawW, drawH;

      if (imgRatio > cRatio) {
        drawH = canvasHeight * zoom;
        drawW = (canvasHeight * imgRatio) * zoom;
      } else {
        drawW = canvasWidth * zoom;
        drawH = (canvasWidth / imgRatio) * zoom;
      }

      const drawX = (canvasWidth - drawW) / 2 + panX;
      const drawY = (canvasHeight - drawH) / 2 + panY;

      ctx.drawImage(bgImage, drawX, drawY, drawW, drawH);
      ctx.restore();

      // Layer 2: Subject Cutout
      if (subjectImage) {
        ctx.save();
        const sRatio = subjectImage.width / subjectImage.height;
        const sH = canvasHeight * 0.85;
        const sW = sH * sRatio;
        ctx.drawImage(subjectImage, canvasWidth - sW - 30, canvasHeight - sH, sW, sH);
        ctx.restore();
      }

      // Layer 3: Legibility Gradient
      const darkness = document.getElementById('darkness').value / 100;
      const grad = ctx.createLinearGradient(0, canvasHeight * 0.25, 0, canvasHeight);
      grad.addColorStop(0, 'rgba(0, 0, 0, 0)');
      grad.addColorStop(0.4, `rgba(5, 7, 15, ${darkness * 0.6})`);
      grad.addColorStop(1, `rgba(4, 6, 12, ${darkness})`);

      ctx.fillStyle = grad;
      ctx.fillRect(0, 0, canvasWidth, canvasHeight);

      // Top vignette
      const topGrad = ctx.createLinearGradient(0, 0, 0, 160);
      topGrad.addColorStop(0, 'rgba(0, 0, 0, 0.7)');
      topGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');
      ctx.fillStyle = topGrad;
      ctx.fillRect(0, 0, canvasWidth, 160);

      // Layer 4: Brand Text & Custom Logo
      const brand = document.getElementById('brandName').value;
      const date = document.getElementById('dateText').value;
      const font = document.getElementById('fontStyle').value;
      const logoPos = document.getElementById('logoPos').value;
      const logoSize = parseInt(document.getElementById('logoSize').value);

      let textStartX = 60;

      // Draw Logo at Top-Left if selected
      if (logoImage && logoPos === 'top-left') {
        const lRatio = logoImage.width / logoImage.height;
        const lH = logoSize;
        const lW = lH * lRatio;
        ctx.drawImage(logoImage, 60, 42, lW, lH);
        textStartX = 60 + lW + 20;
      }

      // Brand Text
      ctx.fillStyle = "#ffffff";
      ctx.font = `800 26px ${font}`;
      ctx.fillText(brand, textStartX, 70);

      ctx.fillStyle = "#dc2626";
      ctx.beginPath();
      ctx.arc(textStartX + ctx.measureText(brand).width + 15, 62, 5, 0, Math.PI * 2);
      ctx.fill();

      // Top-Right: Date or Logo
      if (logoImage && logoPos === 'top-right') {
        const lRatio = logoImage.width / logoImage.height;
        const lH = logoSize;
        const lW = lH * lRatio;
        ctx.drawImage(logoImage, canvasWidth - 60 - lW, 42, lW, lH);
      } else {
        ctx.fillStyle = "#cbd5e1";
        ctx.font = `600 17px ${font}`;
        ctx.textAlign = "right";
        ctx.fillText(date, canvasWidth - 60, 70);
        ctx.textAlign = "left";
      }

      // Layer 5: News Content & Footer
      const tag = document.getElementById('tagText').value;
      const tagBg = document.getElementById('tagBgColor').value;
      const headline = document.getElementById('headlineText').value;
      const sub = document.getElementById('subText').value;
      const handle = document.getElementById('handleText').value;
      const headlineSize = parseInt(document.getElementById('headlineSize').value);

      let cursorY = canvasHeight - 110;

      // Footer divider
      ctx.strokeStyle = "rgba(255, 255, 255, 0.25)";
      ctx.lineWidth = 1.5;
      ctx.beginPath();
      ctx.moveTo(60, cursorY);
      ctx.lineTo(canvasWidth - 60, cursorY);
      ctx.stroke();

      ctx.fillStyle = "#94a3b8";
      ctx.font = `600 18px 'Inter', ${font}`;
      ctx.fillText(handle, 60, cursorY + 38);

      // Draw Logo at Footer Right if selected
      if (logoImage && logoPos === 'bottom-right') {
        const lRatio = logoImage.width / logoImage.height;
        const lH = 34;
        const lW = lH * lRatio;
        ctx.drawImage(logoImage, canvasWidth - 60 - lW, cursorY + 16, lW, lH);
      } else {
        ctx.textAlign = "right";
        ctx.fillText(brand, canvasWidth - 60, cursorY + 38);
        ctx.textAlign = "left";
      }

      // Sub-headline
      cursorY -= 35;
      if (sub.trim() !== '') {
        ctx.fillStyle = "#cbd5e1";
        ctx.font = `500 24px ${font}`;
        cursorY = drawWrappedText(ctx, sub, 60, cursorY, canvasWidth - 120, 34) - 20;
      }

      // Headline
      ctx.fillStyle = "#ffffff";
      ctx.font = `800 ${headlineSize}px ${font}`;
      cursorY = drawWrappedText(ctx, headline, 60, cursorY, canvasWidth - 120, headlineSize * 1.35) - 25;

      // Tag Badge
      if (tag.trim() !== '') {
        ctx.font = `800 18px ${font}`;
        const tagWidth = ctx.measureText(tag).width + 34;
        const tagHeight = 38;
        const tagY = cursorY - tagHeight + 5;

        ctx.fillStyle = tagBg;
        roundRect(ctx, 60, tagY, tagWidth, tagHeight, 6);
        ctx.fill();

        ctx.fillStyle = "#ffffff";
        ctx.fillText(tag, 77, tagY + 26);
      }
    }

    function drawWrappedText(ctx, text, x, y, maxWidth, lineHeight) {
      const words = text.split(' ');
      const lines = [];
      let currentLine = words[0];

      for (let i = 1; i < words.length; i++) {
        const testLine = currentLine + " " + words[i];
        if (ctx.measureText(testLine).width > maxWidth) {
          lines.push(currentLine);
          currentLine = words[i];
        } else {
          currentLine = testLine;
        }
      }
      lines.push(currentLine);

      let currentY = y;
      for (let i = lines.length - 1; i >= 0; i--) {
        ctx.fillText(lines[i], x, currentY);
        currentY -= lineHeight;
      }
      return currentY;
    }

    function roundRect(ctx, x, y, width, height, radius) {
      ctx.beginPath();
      ctx.moveTo(x + radius, y);
      ctx.lineTo(x + width - radius, y);
      ctx.quadraticCurveTo(x + width, y, x + width, y + radius);
      ctx.lineTo(x + width, y + height - radius);
      ctx.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
      ctx.lineTo(x + radius, y + height);
      ctx.quadraticCurveTo(x, y + height, x, y + height - radius);
      ctx.lineTo(x, y + radius);
      ctx.quadraticCurveTo(x, y, x + radius, y);
      ctx.closePath();
    }

    function downloadCard() {
      const link = document.createElement('a');
      link.download = `newscard-${Date.now()}.png`;
      link.href = canvas.toDataURL('image/png', 1.0);
      link.click();
    }
  </script>
</body>
</html>
