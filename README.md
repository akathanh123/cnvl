# <!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hệ và Môi Trường - Mô Phỏng Tương Tác</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;800&display=swap');
        body { font-family: 'Montserrat', sans-serif; }
        
        /* Animations */
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }
        .animate-float { animation: float 3s ease-in-out infinite; }

        @keyframes steam {
            0% { opacity: 0; transform: translateY(0) scaleX(1); }
            50% { opacity: 0.6; transform: translateY(-10px) scaleX(1.2); }
            100% { opacity: 0; transform: translateY(-25px) scaleX(1.5); }
        }
        .steam-particle {
            position: absolute;
            background: rgba(148, 163, 184, 0.4); /* Darker steam for light bg */
            border-radius: 50%;
            pointer-events: none;
        }

        @keyframes energyFlow {
            0% { transform: scale(1); opacity: 0.5; }
            50% { transform: scale(1.1); opacity: 0.8; box-shadow: 0 0 15px rgba(255, 165, 0, 0.6); }
            100% { transform: scale(1); opacity: 0.5; }
        }

        /* Glassmorphism - Pure White Background Optimized */
        .glass {
            background: rgba(255, 255, 255, 0.95); /* Tăng độ chắn sáng lên gần như tuyệt đối */
            backdrop-filter: blur(12px);
            border: 1px solid rgba(226, 232, 240, 1); /* Border rõ hơn */
            box-shadow: 0 10px 40px -10px rgba(0, 0, 0, 0.05);
        }

        .active-btn {
            background: linear-gradient(to right, #3b82f6, #2563eb);
            border-color: #60a5fa;
            box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
            transform: scale(1.02);
            color: white !important;
        }
        .active-btn i {
            color: white !important;
        }
        .active-btn p {
            color: rgba(255, 255, 255, 0.8) !important;
        }

        /* Diagonal lines pattern for insulation */
        .pattern-diagonal-lines {
            background-image: repeating-linear-gradient(45deg, transparent, transparent 5px, rgba(0,0,0,0.1) 5px, rgba(0,0,0,0.1) 10px);
        }
    </style>
</head>
<body class="bg-white text-slate-800 min-h-screen selection:bg-purple-200 selection:text-purple-900">

    <!-- Background Elements - Very subtle now -->
    <div class="fixed inset-0 overflow-hidden -z-10 opacity-30">
        <div class="absolute top-[-10%] left-[-10%] w-[600px] h-[600px] bg-purple-100 rounded-full blur-[100px]"></div>
        <div class="absolute bottom-[-10%] right-[-10%] w-[500px] h-[500px] bg-blue-100 rounded-full blur-[80px]"></div>
    </div>

    <main class="container mx-auto px-4 py-8 max-w-6xl">
        
        <!-- Header -->
        <header class="text-center mb-10">
            <span class="inline-block py-1 px-3 rounded-full bg-slate-50 text-slate-600 text-xs font-bold tracking-widest uppercase mb-3 border border-slate-200">
                Nhiệt động hóa học - Phần 1.1.2
            </span>
            <h1 class="text-4xl md:text-6xl font-black mb-4 bg-clip-text text-transparent bg-gradient-to-r from-purple-600 via-pink-600 to-blue-600 drop-shadow-sm">
                HỆ VÀ MÔI TRƯỜNG
            </h1>
            <p class="text-slate-500 max-w-2xl mx-auto font-medium">
                Khám phá sự khác biệt giữa các loại hệ thông qua khả năng trao đổi 
                <span class="text-yellow-600 font-bold bg-yellow-50 px-1 rounded border border-yellow-100">Năng lượng</span> và 
                <span class="text-blue-600 font-bold bg-blue-50 px-1 rounded border border-blue-100">Vật chất</span>.
            </p>
        </header>

        <!-- Main Interactive Area -->
        <div class="grid lg:grid-cols-12 gap-8">
            
            <!-- Left: Controls & Definitions -->
            <div class="lg:col-span-4 space-y-4">
                <div class="glass p-6 rounded-2xl">
                    <h3 class="text-xl font-bold mb-4 flex items-center text-slate-700">
                        <i class="fas fa-hand-pointer mr-2 text-purple-500"></i> Chọn loại hệ
                    </h3>
                    
                    <div class="space-y-3">
                        <button onclick="setSystem('open')" id="btn-open" class="w-full text-left p-4 rounded-xl border border-slate-200 bg-white transition-all duration-300 hover:shadow-md active-btn group">
                            <div class="flex justify-between items-center">
                                <span class="font-bold text-lg text-slate-700 group-hover:text-blue-600 transition-colors">Hệ Hở (Open)</span>
                                <i class="fas fa-door-open text-slate-400 group-hover:text-blue-600 transition-colors"></i>
                            </div>
                            <p class="text-xs text-slate-500 mt-1 transition-colors">Trao đổi cả vật chất & năng lượng</p>
                        </button>

                        <button onclick="setSystem('closed')" id="btn-closed" class="w-full text-left p-4 rounded-xl border border-slate-200 bg-white transition-all duration-300 hover:shadow-md group">
                            <div class="flex justify-between items-center">
                                <span class="font-bold text-lg text-slate-700 group-hover:text-green-600 transition-colors">Hệ Kín (Closed)</span>
                                <i class="fas fa-box text-slate-400 group-hover:text-green-600 transition-colors"></i>
                            </div>
                            <p class="text-xs text-slate-500 mt-1 transition-colors">Chỉ trao đổi năng lượng</p>
                        </button>

                        <button onclick="setSystem('isolated')" id="btn-isolated" class="w-full text-left p-4 rounded-xl border border-slate-200 bg-white transition-all duration-300 hover:shadow-md group">
                            <div class="flex justify-between items-center">
                                <span class="font-bold text-lg text-slate-700 group-hover:text-red-600 transition-colors">Hệ Cô Lập (Isolated)</span>
                                <i class="fas fa-shield-alt text-slate-400 group-hover:text-red-600 transition-colors"></i>
                            </div>
                            <p class="text-xs text-slate-500 mt-1 transition-colors">Không trao đổi gì cả</p>
                        </button>
                    </div>
                </div>

                <!-- Info Box -->
                <div class="glass p-6 rounded-2xl bg-slate-50 border-slate-200 transition-all duration-500" id="info-box">
                    <h4 class="text-blue-600 font-bold mb-2 flex items-center text-lg">
                        <i class="fas fa-info-circle mr-2"></i> <span id="info-title">Hệ Hở</span>
                    </h4>
                    <p class="text-sm text-slate-600 leading-relaxed font-medium" id="info-desc">
                        Là hệ có sự trao đổi cả vật chất và năng lượng với môi trường ngoài. Ví dụ: Một cốc nước nóng để ngoài không khí (nước bay hơi, nhiệt tỏa ra).
                    </p>
                </div>
            </div>

            <!-- Right: Visualization (Canvas) -->
            <div class="lg:col-span-8">
                <!-- UPDATED CONTAINER: bg-white for pure white background -->
                <div class="glass p-1 rounded-2xl relative h-[500px] overflow-hidden flex items-center justify-center bg-white shadow-lg border border-slate-100">
                    
                    <!-- Environment Label -->
                    <div class="absolute top-4 right-4 text-slate-400 font-bold text-sm tracking-widest uppercase z-20">Môi Trường (Surroundings)</div>

                    <!-- The System Visual Container -->
                    <div class="relative w-64 h-80 transition-all duration-500 z-10">
                        
                        <!-- Energy Arrows (Heat) -->
                        <div id="energy-exchange" class="absolute -inset-16 z-0 flex justify-between items-center transition-opacity duration-500">
                            <div class="text-yellow-500 text-5xl animate-pulse filter drop-shadow-sm"><i class="fas fa-angle-double-right"></i></div> <!-- Heat In -->
                            <div class="text-yellow-500 text-5xl animate-pulse filter drop-shadow-sm"><i class="fas fa-angle-double-left"></i></div> <!-- Heat In -->
                            <!-- Top/Bottom Heat -->
                            <div class="absolute top-0 left-1/2 -translate-x-1/2 -mt-12 text-yellow-500 text-5xl animate-pulse rotate-90 filter drop-shadow-sm"><i class="fas fa-angle-double-right"></i></div>
                        </div>

                        <!-- Insulation Layer (For Isolated System) -->
                        <div id="insulation" class="absolute -inset-4 bg-slate-100 rounded-b-3xl rounded-t-lg opacity-0 transition-opacity duration-500 border-4 border-slate-300 pattern-diagonal-lines z-0 shadow-xl"></div>

                        <!-- The Flask/Container -->
                        <div class="w-full h-full bg-blue-50/50 border-2 border-blue-200 rounded-b-3xl rounded-t-lg relative z-10 backdrop-blur-sm overflow-hidden shadow-2xl">
                            
                            <!-- Liquid/Gas Content -->
                            <div class="absolute bottom-0 w-full h-3/4 bg-gradient-to-t from-blue-400/20 to-blue-200/5"></div>
                            
                            <!-- Particles (Canvas) -->
                            <canvas id="particleCanvas" class="absolute inset-0 w-full h-full"></canvas>

                            <!-- System Label -->
                            <div class="absolute bottom-4 left-0 w-full text-center text-blue-800 font-bold tracking-wider text-sm pointer-events-none">HỆ (SYSTEM)</div>
                        </div>

                        <!-- Lid (For Closed/Isolated) -->
                        <div id="lid" class="absolute -top-3 left-0 w-full h-6 bg-slate-200 rounded-t-md shadow-lg transform -translate-y-10 opacity-0 transition-all duration-500 z-20 border-b-4 border-slate-300"></div>

                        <!-- Matter Exchange Visualization (Steam/Particles leaving) -->
                        <div id="matter-exchange" class="absolute -top-20 left-0 w-full h-20 z-0 transition-opacity duration-500">
                            <!-- JS will generate steam particles here -->
                        </div>

                    </div>
                    
                    <!-- Legend -->
                    <div class="absolute bottom-4 left-4 flex gap-4 text-xs font-bold bg-white p-3 rounded-xl shadow border border-slate-100 z-20">
                        <div class="flex items-center text-yellow-600">
                            <i class="fas fa-fire mr-1.5"></i> Năng Lượng
                        </div>
                        <div class="flex items-center text-blue-600">
                            <i class="fas fa-circle mr-1.5 text-[8px]"></i> Vật Chất
                        </div>
                    </div>

                </div>
            </div>
        </div>

        <!-- Comparison Table -->
        <section class="mt-12 glass rounded-2xl p-8 overflow-x-auto bg-white border border-slate-200 shadow-sm">
            <h3 class="text-2xl font-bold mb-6 text-center text-slate-800">Bảng So Sánh Tóm Tắt</h3>
            <table class="w-full text-left border-collapse">
                <thead>
                    <tr class="text-slate-500 border-b border-slate-300 text-sm uppercase tracking-wider">
                        <th class="p-4">Loại Hệ</th>
                        <th class="p-4 text-center">Trao đổi Vật Chất ($\Delta m$)</th>
                        <th class="p-4 text-center">Trao đổi Năng Lượng ($Q, A$)</th>
                        <th class="p-4">Ví dụ thực tế</th>
                    </tr>
                </thead>
                <tbody class="text-slate-700">
                    <tr class="border-b border-slate-200 hover:bg-slate-50 transition rounded-lg" id="row-open">
                        <td class="p-4 font-bold text-blue-600">Hệ Hở</td>
                        <td class="p-4 text-center text-green-600 bg-green-50 rounded-lg m-1"><i class="fas fa-check mr-1"></i> Có</td>
                        <td class="p-4 text-center text-green-600 bg-green-50 rounded-lg m-1"><i class="fas fa-check mr-1"></i> Có</td>
                        <td class="p-4">Cốc nước mở nắp, cơ thể người.</td>
                    </tr>
                    <tr class="border-b border-slate-200 hover:bg-slate-50 transition" id="row-closed">
                        <td class="p-4 font-bold text-green-600">Hệ Kín</td>
                        <td class="p-4 text-center text-red-500 bg-red-50 rounded-lg m-1"><i class="fas fa-times mr-1"></i> Không</td>
                        <td class="p-4 text-center text-green-600 bg-green-50 rounded-lg m-1"><i class="fas fa-check mr-1"></i> Có</td>
                        <td class="p-4">Xi lanh kín, bóng đèn điện.</td>
                    </tr>
                    <tr class="hover:bg-slate-50 transition" id="row-isolated">
                        <td class="p-4 font-bold text-red-600">Hệ Cô Lập</td>
                        <td class="p-4 text-center text-red-500 bg-red-50 rounded-lg m-1"><i class="fas fa-times mr-1"></i> Không</td>
                        <td class="p-4 text-center text-red-500 bg-red-50 rounded-lg m-1"><i class="fas fa-times mr-1"></i> Không</td>
                        <td class="p-4">Bình thủy (phích) lý tưởng, vũ trụ (theo lý thuyết).</td>
                    </tr>
                </tbody>
            </table>
        </section>

    </main>

    <script>
        // --- DATA & STATE ---
        const systemData = {
            open: {
                title: "Hệ Hở (Open System)",
                desc: "Hệ hở trao đổi cả vật chất và năng lượng với môi trường xung quanh. Ranh giới hệ cho phép các phân tử khí/lỏng thoát ra ngoài hoặc đi vào.",
                lid: false,
                insulation: false,
                energy: true,
                matter: true,
                color: "blue"
            },
            closed: {
                title: "Hệ Kín (Closed System)",
                desc: "Hệ kín chỉ trao đổi năng lượng (nhiệt, công) nhưng KHÔNG trao đổi vật chất. Ranh giới ngăn cản sự di chuyển của vật chất qua lại.",
                lid: true,
                insulation: false,
                energy: true,
                matter: false,
                color: "green"
            },
            isolated: {
                title: "Hệ Cô Lập (Isolated System)",
                desc: "Hệ cô lập không trao đổi vật chất lẫn năng lượng. Đây là hệ lý tưởng hóa, được bao bọc bởi lớp cách nhiệt tuyệt đối.",
                lid: true,
                insulation: true,
                energy: false,
                matter: false,
                color: "red"
            }
        };

        let currentSystem = 'open';
        let animationId;

        // --- DOM ELEMENTS ---
        const lidEl = document.getElementById('lid');
        const insulationEl = document.getElementById('insulation');
        const energyEl = document.getElementById('energy-exchange');
        const matterEl = document.getElementById('matter-exchange');
        const infoTitle = document.getElementById('info-title');
        const infoDesc = document.getElementById('info-desc');
        const infoBox = document.getElementById('info-box');

        // --- PARTICLE SYSTEM ---
        const canvas = document.getElementById('particleCanvas');
        const ctx = canvas.getContext('2d');
        let particles = [];
        
        // Resize canvas
        function resizeCanvas() {
            canvas.width = canvas.parentElement.offsetWidth;
            canvas.height = canvas.parentElement.offsetHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        setTimeout(resizeCanvas, 100); // Initial delay to ensure container size

        class Particle {
            constructor() {
                this.reset();
            }

            reset() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height;
                this.vx = (Math.random() - 0.5) * 1.5;
                this.vy = (Math.random() - 0.5) * 1.5;
                this.size = Math.random() * 3 + 1;
                this.alpha = Math.random() * 0.5 + 0.3;
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;

                // Bounce off walls (Left/Right/Bottom)
                if (this.x < 0 || this.x > canvas.width) this.vx *= -1;
                if (this.y > canvas.height) this.vy *= -1;

                // Top boundary behavior depends on System Type
                if (this.y < 0) {
                    if (currentSystem === 'open') {
                        // Allow to leave (and reset at bottom to simulate new matter or cycle)
                        if (this.y < -20) {
                            this.y = canvas.height;
                            this.x = Math.random() * canvas.width;
                        }
                    } else {
                        // Bounce back (Closed/Isolated)
                        this.y = 0;
                        this.vy *= -1;
                    }
                }
            }

            draw() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                // Use a darker blue for visibility on light background
                ctx.fillStyle = `rgba(37, 99, 235, ${this.alpha})`; // Blue-600 equivalent
                ctx.fill();
            }
        }

        // Initialize particles
        for(let i=0; i<60; i++) particles.push(new Particle());

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            particles.forEach(p => {
                p.update();
                p.draw();
            });

            // Steam/Matter Visualization for Open System
            if(currentSystem === 'open' && Math.random() > 0.9) {
                createSteam();
            }

            animationId = requestAnimationFrame(animate);
        }

        // --- VISUAL LOGIC ---
        function setSystem(type) {
            currentSystem = type;
            const data = systemData[type];

            // 1. Update Buttons Styling
            ['open', 'closed', 'isolated'].forEach(t => {
                const btn = document.getElementById(`btn-${t}`);
                if (t === type) {
                    btn.classList.add('active-btn');
                } else {
                    btn.classList.remove('active-btn');
                }
            });

            // 2. Update Info Text
            infoTitle.innerText = data.title;
            infoDesc.innerText = data.desc;
            
            // Info box border color update - Adapted for Light Mode
            // Using generic slate-600 text base, specific color for title
            infoBox.className = `glass p-6 rounded-2xl bg-${data.color}-50 border-${data.color}-200 transition-all duration-500`;
            infoTitle.className = `text-${data.color}-600 font-bold mb-2 flex items-center text-lg`;

            // 3. Lid Animation
            if (data.lid) {
                lidEl.classList.remove('opacity-0', '-translate-y-10');
                lidEl.classList.add('translate-y-0');
            } else {
                lidEl.classList.add('opacity-0', '-translate-y-10');
                lidEl.classList.remove('translate-y-0');
            }

            // 4. Insulation Animation
            if (data.insulation) {
                insulationEl.classList.remove('opacity-0');
            } else {
                insulationEl.classList.add('opacity-0');
            }

            // 5. Energy Exchange (Heat arrows)
            if (data.energy) {
                energyEl.classList.remove('opacity-0');
            } else {
                energyEl.classList.add('opacity-0');
            }

            // 6. Matter Exchange (Steam)
            if (data.matter) {
                matterEl.classList.remove('opacity-0');
            } else {
                matterEl.classList.add('opacity-0');
                matterEl.innerHTML = ''; // Clear existing steam
            }

            // 7. Table Highlight
            ['open', 'closed', 'isolated'].forEach(t => {
                const row = document.getElementById(`row-${t}`);
                if (t === type) row.classList.add('bg-blue-50');
                else row.classList.remove('bg-blue-50');
            });
        }

        // Helper to create DOM Steam elements (visual flair outside canvas)
        function createSteam() {
            const steam = document.createElement('div');
            steam.classList.add('steam-particle');
            
            const size = Math.random() * 10 + 5;
            steam.style.width = `${size}px`;
            steam.style.height = `${size}px`;
            
            // Random position at top of container
            steam.style.left = `${Math.random() * 100}%`;
            steam.style.bottom = '0px';
            
            // Animation
            steam.animate([
                { transform: 'translateY(0) scale(1)', opacity: 0.4 },
                { transform: 'translateY(-60px) scale(1.5)', opacity: 0 }
            ], {
                duration: 2000 + Math.random() * 1000,
                easing: 'ease-out'
            }).onfinish = () => steam.remove();

            matterEl.appendChild(steam);
        }

        // Start
        setSystem('open');
        animate();

    </script>
</body>
</html>
