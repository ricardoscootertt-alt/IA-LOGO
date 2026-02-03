<html lang="pt-pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Logo Studio Pro - Edição Final</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lucide/0.263.0/lucide.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #f1f5f9;
        }

        .canvas-card {
            background: white;
            border: 1px solid #e2e8f0;
            box-shadow: 0 10px 25px -5px rgba(0,0,0,0.05);
        }

        #drawing-canvas {
            cursor: crosshair;
            touch-action: none;
            background: #ffffff;
        }

        .style-btn.active {
            background-color: #4f46e5;
            color: white;
            border-color: #4f46e5;
        }

        .loader {
            border: 3px solid #f3f3f3;
            border-top: 3px solid #4f46e5;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        /* Estilização da área de resultado */
        .result-box {
            background-color: #ffffff;
            background-image: 
                linear-gradient(45deg, #fafafa 25%, transparent 25%), 
                linear-gradient(-45deg, #fafafa 25%, transparent 25%), 
                linear-gradient(45deg, transparent 75%, #fafafa 75%), 
                linear-gradient(-45deg, transparent 75%, #fafafa 75%);
            background-size: 20px 20px;
            background-position: 0 0, 0 10px, 10px 10px, 10px 0;
        }
    </style>
</head>
<body class="p-4 md:p-8">
    <div class="max-w-7xl mx-auto">
        <!-- Header -->
        <header class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-6">
            <div>
                <h1 class="text-3xl font-extrabold text-slate-900 flex items-center gap-3">
                    <span class="p-2 bg-indigo-600 rounded-xl text-white">
                        <i data-lucide="shield-check"></i>
                    </span>
                    Logo Studio IA
                </h1>
                <p class="text-slate-500 mt-1">Transformação limpa: Apenas o logótipo, sem textos indesejados.</p>
            </div>
            
            <div class="flex gap-2">
                <button id="clear-btn" class="px-4 py-2 bg-white border border-slate-200 rounded-xl hover:bg-rose-50 hover:text-rose-600 transition flex items-center gap-2 font-semibold">
                    <i data-lucide="eraser" class="w-4 h-4"></i> Limpar
                </button>
                <button id="transform-btn" class="px-6 py-2 bg-indigo-600 text-white rounded-xl font-bold hover:bg-indigo-700 transition shadow-lg flex items-center gap-2">
                    <i data-lucide="sparkles" class="w-4 h-4"></i> Gerar Logótipo
                </button>
            </div>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            <!-- Coluna de Desenho -->
            <div class="lg:col-span-5 space-y-6">
                <div class="canvas-card p-6 rounded-3xl">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="font-bold text-slate-800">Desenhe aqui</h3>
                        <div class="flex gap-2" id="color-picker">
                            <button class="w-6 h-6 rounded-full bg-slate-900 ring-2 ring-indigo-500" data-color="#0f172a"></button>
                            <button class="w-6 h-6 rounded-full bg-indigo-500" data-color="#4f46e5"></button>
                            <button class="w-6 h-6 rounded-full bg-rose-500" data-color="#f43f5e"></button>
                        </div>
                    </div>
                    <div class="aspect-square bg-white rounded-2xl overflow-hidden border-2 border-slate-200">
                        <canvas id="drawing-canvas"></canvas>
                    </div>
                </div>

                <div class="canvas-card p-6 rounded-3xl">
                    <h3 class="font-bold text-slate-800 mb-3">Estilo Visual</h3>
                    <div class="grid grid-cols-2 gap-2" id="style-grid">
                        <button class="style-btn active px-3 py-2 border border-slate-200 rounded-xl text-xs font-bold uppercase tracking-tight transition" data-style="Minimalist vector symbol, clean geometric lines, solid flat design. No text.">Minimalista</button>
                        <button class="style-btn px-3 py-2 border border-slate-200 rounded-xl text-xs font-bold uppercase tracking-tight transition" data-style="3D futuristic glassmorphism, glossy surfaces, depth and shadows. No text.">Moderno 3D</button>
                        <button class="style-btn px-3 py-2 border border-slate-200 rounded-xl text-xs font-bold uppercase tracking-tight transition" data-style="Premium luxury gold emblem, elegant high-end branding. No text.">Luxo</button>
                        <button class="style-btn px-3 py-2 border border-slate-200 rounded-xl text-xs font-bold uppercase tracking-tight transition" data-style="Abstract artistic brand mark, creative shapes, vibrant colors. No text.">Abstrato</button>
                    </div>
                </div>
            </div>

            <!-- Coluna de Resultado -->
            <div class="lg:col-span-7">
                <div class="canvas-card p-6 rounded-3xl h-full flex flex-col">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="font-bold text-slate-800">Resultado Final</h3>
                        <button id="download-btn" disabled class="px-4 py-2 bg-slate-100 text-slate-400 rounded-xl text-sm font-bold flex items-center gap-2 cursor-not-allowed transition">
                            <i data-lucide="download" class="w-4 h-4"></i> Guardar
                        </button>
                    </div>

                    <div id="result-area" class="result-box flex-1 min-h-[450px] rounded-2xl border-2 border-slate-100 relative flex items-center justify-center overflow-hidden">
                        <!-- Placeholder -->
                        <div id="placeholder" class="text-center p-10 opacity-40">
                            <i data-lucide="image" class="w-16 h-16 mx-auto mb-4"></i>
                            <p class="text-sm font-medium">O seu design aparecerá aqui</p>
                        </div>

                        <!-- Loading -->
                        <div id="loader" class="hidden absolute inset-0 bg-white/90 z-20 flex flex-col items-center justify-center">
                            <div class="loader mb-4"></div>
                            <p class="font-bold text-indigo-600">A processar...</p>
                        </div>

                        <!-- Final Image -->
                        <img id="result-img" class="hidden w-full h-full object-contain p-8 z-10" alt="Logótipo Gerado">
                    </div>
                    
                    <p class="text-[10px] text-slate-400 mt-4 text-center uppercase tracking-widest font-bold">
                        A IA irá ignorar os traços imperfeitos e criar uma forma geométrica pura.
                    </p>
                </div>
            </div>
        </div>
    </div>

    <script>
        const apiKey = "";
        const canvas = document.getElementById('drawing-canvas');
        const ctx = canvas.getContext('2d');
        const resultImg = document.getElementById('result-img');
        const loader = document.getElementById('loader');
        const placeholder = document.getElementById('placeholder');
        const downloadBtn = document.getElementById('download-btn');
        const transformBtn = document.getElementById('transform-btn');
        
        let isDrawing = false;
        let activeStyle = "Minimalist vector symbol, clean geometric lines, solid flat design. No text.";
        let activeColor = "#0f172a";

        function init() {
            lucide.createIcons();
            resizeCanvas();
            ctx.fillStyle = "white";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
        }

        function resizeCanvas() {
            const container = canvas.parentElement;
            canvas.width = container.clientWidth;
            canvas.height = container.clientHeight;
            ctx.lineCap = 'round';
            ctx.lineJoin = 'round';
            ctx.lineWidth = 5;
        }

        // Desenho
        canvas.addEventListener('mousedown', (e) => {
            isDrawing = true;
            ctx.beginPath();
            ctx.strokeStyle = activeColor;
            const rect = canvas.getBoundingClientRect();
            ctx.moveTo(e.clientX - rect.left, e.clientY - rect.top);
        });

        canvas.addEventListener('mousemove', (e) => {
            if (!isDrawing) return;
            const rect = canvas.getBoundingClientRect();
            ctx.lineTo(e.clientX - rect.left, e.clientY - rect.top);
            ctx.stroke();
        });

        window.addEventListener('mouseup', () => isDrawing = false);

        // Touch
        canvas.addEventListener('touchstart', (e) => {
            e.preventDefault();
            isDrawing = true;
            ctx.beginPath();
            ctx.strokeStyle = activeColor;
            const rect = canvas.getBoundingClientRect();
            ctx.moveTo(e.touches[0].clientX - rect.left, e.touches[0].clientY - rect.top);
        });

        canvas.addEventListener('touchmove', (e) => {
            e.preventDefault();
            if (!isDrawing) return;
            const rect = canvas.getBoundingClientRect();
            ctx.lineTo(e.touches[0].clientX - rect.left, e.touches[0].clientY - rect.top);
            ctx.stroke();
        });

        // UI
        document.querySelectorAll('#color-picker button').forEach(btn => {
            btn.addEventListener('click', () => {
                activeColor = btn.dataset.color;
                document.querySelectorAll('#color-picker button').forEach(b => b.classList.remove('ring-2', 'ring-indigo-500'));
                btn.classList.add('ring-2', 'ring-indigo-500');
            });
        });

        document.querySelectorAll('.style-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.style-btn').forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                activeStyle = btn.dataset.style;
            });
        });

        document.getElementById('clear-btn').addEventListener('click', () => {
            ctx.fillStyle = "white";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            resultImg.classList.add('hidden');
            placeholder.classList.remove('hidden');
            downloadBtn.disabled = true;
            downloadBtn.className = "px-4 py-2 bg-slate-100 text-slate-400 rounded-xl text-sm font-bold flex items-center gap-2 cursor-not-allowed transition";
        });

        // Geração IA
        transformBtn.addEventListener('click', async () => {
            // PROMPT REFORÇADO PARA IGNORAR TEXTO
            const systemPrompt = `You are a specialized image-to-logo generator.
            Your ONLY output must be a clean, professional logo image.
            STRICT RULES:
            - Use the user's sketch ONLY for shape inspiration.
            - DO NOT include the user's instruction text in the image.
            - DO NOT include labels, watermark, or UI elements.
            - STYLE: ${activeStyle}
            - Background must be solid white.
            - High-quality 2D vector appearance.
            - NO text should appear in the final image.`;

            loader.classList.remove('hidden');
            placeholder.classList.add('hidden');
            resultImg.classList.add('hidden');

            try {
                const base64Canvas = canvas.toDataURL('image/png').split(',')[1];
                
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-image-preview:generateContent?key=${apiKey}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{
                            parts: [
                                { text: systemPrompt },
                                { inlineData: { mimeType: "image/png", data: base64Canvas } }
                            ]
                        }],
                        generationConfig: { responseModalities: ['TEXT', 'IMAGE'] }
                    })
                });

                const data = await response.json();
                const base64Result = data.candidates?.[0]?.content?.parts?.find(p => p.inlineData)?.inlineData?.data;

                if (base64Result) {
                    resultImg.src = `data:image/png;base64,${base64Result}`;
                    resultImg.classList.remove('hidden');
                    downloadBtn.disabled = false;
                    downloadBtn.className = "px-4 py-2 bg-indigo-600 text-white rounded-xl text-sm font-bold flex items-center gap-2 hover:bg-indigo-700 transition shadow-md cursor-pointer";
                }
            } catch (err) {
                console.error(err);
                placeholder.classList.remove('hidden');
            } finally {
                loader.classList.add('hidden');
            }
        });

        downloadBtn.addEventListener('click', () => {
            const link = document.createElement('a');
            link.href = resultImg.src;
            link.download = `meu-logo-${Date.now()}.png`;
            link.click();
        });

        init();
    </script>
</body>
</html>
