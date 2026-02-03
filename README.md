
<html lang="pt-pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Logo Designer Pro</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lucide/0.263.0/lucide.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        
        :root {
            --primary: #6366f1;
            --primary-dark: #4f46e5;
        }

        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #f8fafc;
            color: #0f172a;
        }

        .sketch-canvas {
            background-color: #ffffff;
            background-image: radial-gradient(#e2e8f0 1px, transparent 1px);
            background-size: 24px 24px;
            cursor: crosshair;
            touch-action: none;
        }

        .btn-glow {
            transition: all 0.3s ease;
            box-shadow: 0 0 0 0 rgba(99, 102, 241, 0.4);
        }

        .btn-glow:hover {
            box-shadow: 0 0 20px 5px rgba(99, 102, 241, 0.2);
            transform: translateY(-1px);
        }

        .loader-ring {
            border: 3px solid #f3f3f3;
            border-top: 3px solid var(--primary);
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        .result-view {
            background: #ffffff;
            background-image: 
                linear-gradient(45deg, #f1f5f9 25%, transparent 25%), 
                linear-gradient(-45deg, #f1f5f9 25%, transparent 25%), 
                linear-gradient(45deg, transparent 75%, #f1f5f9 75%), 
                linear-gradient(-45deg, transparent 75%, #f1f5f9 75%);
            background-size: 20px 20px;
            background-position: 0 0, 0 10px, 10px 10px, 10px 0;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Navegação Superior -->
    <nav class="bg-white border-b border-slate-200 px-6 py-4 flex justify-between items-center sticky top-0 z-50">
        <div class="flex items-center gap-2">
            <div class="bg-indigo-600 p-2 rounded-lg text-white">
                <i data-lucide="zap" class="w-5 h-5"></i>
            </div>
            <span class="text-xl font-extrabold tracking-tight">Logo<span class="text-indigo-600">Forge</span> AI</span>
        </div>
        <div class="flex items-center gap-4">
            <button id="clear-btn" class="text-slate-500 hover:text-rose-500 transition-colors p-2 rounded-lg hover:bg-rose-50">
                <i data-lucide="trash-2" class="w-5 h-5"></i>
            </button>
            <button id="generate-btn" class="bg-slate-900 text-white px-6 py-2.5 rounded-xl font-bold flex items-center gap-2 btn-glow">
                <i data-lucide="sparkles" class="w-4 h-4"></i>
                Transformar
            </button>
        </div>
    </nav>

    <main class="flex-1 grid grid-cols-1 lg:grid-cols-2">
        
        <!-- Editor de Rascunho -->
        <div class="p-6 md:p-10 flex flex-col gap-6 bg-white border-r border-slate-200">
            <div class="flex items-center justify-between">
                <div>
                    <h2 class="text-lg font-bold">Rascunho de Conceito</h2>
                    <p class="text-sm text-slate-500">Desenhe a forma básica do seu logótipo abaixo.</p>
                </div>
                <div class="flex gap-2">
                    <button class="w-8 h-8 rounded-full bg-slate-900 ring-2 ring-indigo-500 ring-offset-2 color-tool" data-color="#0f172a"></button>
                    <button class="w-8 h-8 rounded-full bg-indigo-500 color-tool" data-color="#6366f1"></button>
                    <button class="w-8 h-8 rounded-full bg-rose-500 color-tool" data-color="#f43f5e"></button>
                </div>
            </div>

            <div class="flex-1 min-h-[400px] rounded-[2rem] overflow-hidden border-2 border-slate-100 sketch-canvas relative">
                <canvas id="drawing-canvas"></canvas>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div>
                    <label class="block text-xs font-bold text-slate-400 uppercase mb-2 tracking-widest">Nome da Marca</label>
                    <input type="text" id="brand-name" placeholder="Ex: Lumina Tech" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-indigo-500 outline-none transition-all">
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-400 uppercase mb-2 tracking-widest">Estilo Visual</label>
                    <select id="style-select" class="w-full px-4 py-3 bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-indigo-500 outline-none appearance-none">
                        <option value="minimalist vector, flat design, geometric">Minimalista</option>
                        <option value="3D futuristic, glassmorphism, depth">Moderno 3D</option>
                        <option value="luxury, elegant, gold accents, premium">Luxo / Premium</option>
                        <option value="vintage, retro badge, textured">Vintage / Retro</option>
                        <option value="playful, colorful, organic shapes">Divertido / Criativo</option>
                    </select>
                </div>
            </div>
        </div>

        <!-- Visualização do Resultado -->
        <div class="p-6 md:p-10 flex flex-col gap-6">
            <div class="flex items-center justify-between">
                <h2 class="text-lg font-bold">Identidade Gerada</h2>
                <button id="download-btn" disabled class="text-indigo-600 font-bold text-sm flex items-center gap-2 opacity-30 cursor-not-allowed">
                    <i data-lucide="download" class="w-4 h-4"></i> Exportar PNG
                </button>
            </div>

            <div id="result-area" class="flex-1 min-h-[400px] rounded-[2rem] border-2 border-dashed border-slate-200 result-view relative flex items-center justify-center overflow-hidden">
                
                <!-- Estado Inicial -->
                <div id="placeholder" class="text-center p-8 max-w-sm">
                    <div class="w-20 h-20 bg-white rounded-3xl shadow-sm flex items-center justify-center mx-auto mb-6">
                        <i data-lucide="image" class="w-10 h-10 text-slate-200"></i>
                    </div>
                    <h3 class="text-slate-900 font-bold mb-2">Pronto para Criar</h3>
                    <p class="text-slate-500 text-sm italic">"A IA não apenas limpa o desenho, ela interpreta a sua alma criativa."</p>
                </div>

                <!-- Carregamento -->
                <div id="loader" class="hidden absolute inset-0 bg-white/90 backdrop-blur-sm z-20 flex flex-col items-center justify-center gap-4">
                    <div class="loader-ring"></div>
                    <div class="text-center">
                        <p class="font-bold text-slate-900 tracking-tight">Renderizando Conceito...</p>
                        <p class="text-xs text-slate-500">Isso pode levar alguns segundos.</p>
                    </div>
                </div>

                <!-- Imagem do Resultado -->
                <img id="result-img" class="hidden w-full h-full object-contain p-10 z-10 drop-shadow-2xl" alt="Logo Final">
            </div>

            <div class="bg-indigo-50 p-5 rounded-2xl border border-indigo-100">
                <h4 class="text-indigo-900 font-bold text-xs uppercase mb-2 tracking-widest flex items-center gap-2">
                    <i data-lucide="help-circle" class="w-3 h-3"></i> Como Funciona?
                </h4>
                <p class="text-indigo-800 text-xs leading-relaxed font-medium">
                    A nossa IA analisa a estrutura geométrica do seu rascunho. Não tente desenhar perfeitamente; foque apenas na forma básica. O sistema irá converter esses traços em curvas vetoriais matemáticas e aplicar a paleta de cores do estilo selecionado.
                </p>
            </div>
        </div>
    </main>

    <script>
        // Configuração Inicial
        const apiKey = ""; // Chave injetada automaticamente no ambiente Canvas
        const canvas = document.getElementById('drawing-canvas');
        const ctx = canvas.getContext('2d');
        const generateBtn = document.getElementById('generate-btn');
        const resultImg = document.getElementById('result-img');
        const loader = document.getElementById('loader');
        const placeholder = document.getElementById('placeholder');
        const downloadBtn = document.getElementById('download-btn');
        const brandInput = document.getElementById('brand-name');
        const styleSelect = document.getElementById('style-select');

        let isDrawing = false;
        let currentColor = "#0f172a";

        function setupCanvas() {
            lucide.createIcons();
            const parent = canvas.parentElement;
            canvas.width = parent.clientWidth;
            canvas.height = parent.clientHeight;
            
            ctx.fillStyle = "white";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.lineCap = 'round';
            ctx.lineJoin = 'round';
            ctx.lineWidth = 5;
        }

        // Lógica de Desenho
        function getCoords(e) {
            const rect = canvas.getBoundingClientRect();
            const x = (e.clientX || e.touches?.[0].clientX) - rect.left;
            const y = (e.clientY || e.touches?.[0].clientY) - rect.top;
            return { x, y };
        }

        function startDrawing(e) {
            isDrawing = true;
            ctx.beginPath();
            const { x, y } = getCoords(e);
            ctx.moveTo(x, y);
        }

        function draw(e) {
            if (!isDrawing) return;
            if (e.touches) e.preventDefault();
            const { x, y } = getCoords(e);
            ctx.lineTo(x, y);
            ctx.strokeStyle = currentColor;
            ctx.stroke();
        }

        function stopDrawing() { isDrawing = false; }

        canvas.addEventListener('mousedown', startDrawing);
        canvas.addEventListener('mousemove', draw);
        window.addEventListener('mouseup', stopDrawing);
        canvas.addEventListener('touchstart', startDrawing, {passive: false});
        canvas.addEventListener('touchmove', draw, {passive: false});
        canvas.addEventListener('touchend', stopDrawing);

        // UI de Cores
        document.querySelectorAll('.color-tool').forEach(btn => {
            btn.addEventListener('click', () => {
                currentColor = btn.dataset.color;
                document.querySelectorAll('.color-tool').forEach(b => b.classList.remove('ring-2', 'ring-indigo-500', 'ring-offset-2'));
                btn.classList.add('ring-2', 'ring-indigo-500', 'ring-offset-2');
            });
        });

        document.getElementById('clear-btn').addEventListener('click', () => {
            ctx.fillStyle = "white";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            resultImg.classList.add('hidden');
            placeholder.classList.remove('hidden');
            downloadBtn.disabled = true;
            downloadBtn.classList.add('opacity-30', 'cursor-not-allowed');
        });

        // GERAÇÃO MÁGICA
        generateBtn.addEventListener('click', async () => {
            const brand = brandInput.value.trim();
            const style = styleSelect.value;
            
            loader.classList.remove('hidden');
            placeholder.classList.add('hidden');
            resultImg.classList.add('hidden');

            const systemPrompt = `Você é um Designer de Identidade Visual de renome mundial.
            TAREFA: Transformar o rascunho rudimentar fornecido em um logótipo PROFISSIONAL de alta fidelidade.
            
            DIRETRIZES:
            1. Use o rascunho APENAS como base estrutural e geométrica. 
            2. Ignore os traços trêmulos; interprete-os como curvas vetoriais perfeitas e formas matemáticas.
            3. Estilo Visual: ${style}.
            4. Fundo: Branco sólido e limpo.
            5. Se um nome for fornecido ("${brand}"), integre-o ao design com tipografia moderna e elegante que combine com o estilo.
            6. O resultado final deve parecer uma marca real pronta para uso comercial.
            7. SAÍDA: Apenas a imagem do logo centralizada, sem textos de instrução ou menus.`;

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
                const generatedImage = data.candidates?.[0]?.content?.parts?.find(p => p.inlineData)?.inlineData?.data;

                if (generatedImage) {
                    resultImg.src = `data:image/png;base64,${generatedImage}`;
                    resultImg.classList.remove('hidden');
                    downloadBtn.disabled = false;
                    downloadBtn.classList.remove('opacity-30', 'cursor-not-allowed');
                }
            } catch (err) {
                console.error("Erro na geração:", err);
                placeholder.innerHTML = `<p class="text-rose-500 font-bold">Falha ao gerar logo. Verifique a ligação.</p>`;
                placeholder.classList.remove('hidden');
            } finally {
                loader.classList.add('hidden');
            }
        });

        downloadBtn.addEventListener('click', () => {
            const link = document.createElement('a');
            link.href = resultImg.src;
            link.download = `logo-${Date.now()}.png`;
            link.click();
        });

        // Iniciar
        window.onload = setupCanvas;
        window.onresize = setupCanvas;
    </script>
</body>
</html>
