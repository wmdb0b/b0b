<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>3D Dev Portfolio · GitHub Profile</title>

    <!-- Three.js from CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js">
    </script>

    <style>
        /* ─── Reset & Base ─── */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #0b0d15;
            font-family: 'Inter', 'Segoe UI', system-ui, -apple-system, sans-serif;
            color: #e8edf5;
            display: flex;
            justify-content: center;
            padding: 2rem 1rem;
            min-height: 100vh;
            line-height: 1.6;
        }

        /* ─── Main Card ─── */
        .profile-card {
            max-width: 1100px;
            width: 100%;
            background: rgba(18, 22, 36, 0.85);
            backdrop-filter: blur(18px);
            -webkit-backdrop-filter: blur(18px);
            border-radius: 2.5rem;
            padding: 2.8rem 2.8rem 3.2rem;
            box-shadow: 0 25px 60px -10px rgba(0, 0, 0, 0.8),
                        0 0 0 1px rgba(255, 255, 255, 0.04) inset;
            border: 1px solid rgba(255, 255, 255, 0.05);
            transition: all 0.3s ease;
        }

        /* ─── 3D Scene Container ─── */
        .scene-wrapper {
            position: relative;
            width: 100%;
            height: 320px;
            border-radius: 1.8rem;
            overflow: hidden;
            background: radial-gradient(ellipse at 30% 40%, #131a2e, #07090f);
            margin-bottom: 2.4rem;
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.6);
            border: 1px solid rgba(255, 255, 255, 0.04);
        }

        #three-canvas {
            display: block;
            width: 100%;
            height: 100%;
            cursor: grab;
        }
        #three-canvas:active {
            cursor: grabbing;
        }

        /* ─── Overlay text on 3D scene ─── */
        .scene-overlay {
            position: absolute;
            bottom: 1.8rem;
            left: 2.2rem;
            pointer-events: none;
            z-index: 2;
        }
        .scene-overlay .badge {
            display: inline-block;
            background: rgba(0, 20, 255, 0.25);
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            padding: 0.35rem 1.2rem;
            border-radius: 40px;
            font-size: 0.7rem;
            font-weight: 600;
            letter-spacing: 0.08em;
            text-transform: uppercase;
            color: #8ab4ff;
            border: 1px solid rgba(100, 160, 255, 0.15);
            box-shadow: 0 4px 15px rgba(0, 80, 255, 0.15);
        }

        .scene-overlay h1 {
            font-size: 1.8rem;
            font-weight: 700;
            margin-top: 0.3rem;
            background: linear-gradient(135deg, #f0f6ff 0%, #7aaaff 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: -0.02em;
            text-shadow: 0 0 40px rgba(70, 130, 255, 0.15);
        }

        /* ─── Typography ─── */
        h2 {
            font-size: 1.3rem;
            font-weight: 600;
            letter-spacing: -0.01em;
            margin: 2.4rem 0 0.8rem 0;
            display: flex;
            align-items: center;
            gap: 0.6rem;
        }
        h2 .accent-line {
            flex: 1;
            height: 1px;
            background: linear-gradient(90deg, rgba(100, 160, 255, 0.25), transparent);
        }

        .subhead {
            font-size: 0.95rem;
            color: #8899bb;
            margin-bottom: 1.8rem;
            max-width: 70%;
        }

        /* ─── Grid Layout ─── */
        .grid-2 {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1.8rem;
            margin-top: 0.6rem;
        }

        /* ─── Skill Tags ─── */
        .skill-tags {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem 0.6rem;
        }
        .skill-tag {
            background: rgba(60, 100, 200, 0.12);
            border: 1px solid rgba(100, 160, 255, 0.08);
            padding: 0.3rem 1rem;
            border-radius: 40px;
            font-size: 0.78rem;
            font-weight: 500;
            color: #b8ceff;
            transition: all 0.2s ease;
            letter-spacing: 0.02em;
        }
        .skill-tag:hover {
            background: rgba(60, 100, 200, 0.25);
            border-color: rgba(100, 160, 255, 0.25);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 80, 255, 0.1);
        }

        /* ─── Project Cards ─── */
        .project-card {
            background: rgba(255, 255, 255, 0.02);
            border-radius: 1.2rem;
            padding: 1.4rem 1.6rem;
            border: 1px solid rgba(255, 255, 255, 0.04);
            transition: all 0.25s ease;
            backdrop-filter: blur(4px);
        }
        .project-card:hover {
            background: rgba(255, 255, 255, 0.05);
            border-color: rgba(100, 160, 255, 0.12);
            transform: translateY(-3px);
            box-shadow: 0 12px 30px -10px rgba(0, 0, 0, 0.5);
        }
        .project-card h3 {
            font-size: 1rem;
            font-weight: 600;
            margin-bottom: 0.2rem;
            color: #d6e4ff;
        }
        .project-card p {
            font-size: 0.85rem;
            color: #8899bb;
            margin: 0.3rem 0 0.6rem;
        }
        .project-card .lang {
            font-size: 0.7rem;
            font-weight: 500;
            color: #6a8bc0;
            background: rgba(60, 100, 200, 0.08);
            padding: 0.15rem 0.8rem;
            border-radius: 20px;
            display: inline-block;
            border: 1px solid rgba(100, 160, 255, 0.06);
        }

        /* ─── Stats Row ─── */
        .stats-row {
            display: flex;
            gap: 1.2rem;
            flex-wrap: wrap;
            margin: 1.2rem 0 0.6rem;
        }
        .stat-item {
            background: rgba(255, 255, 255, 0.02);
            padding: 0.4rem 1.2rem 0.4rem 1rem;
            border-radius: 40px;
            border: 1px solid rgba(255, 255, 255, 0.04);
            font-size: 0.8rem;
            color: #99aacd;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        .stat-item strong {
            color: #d6e4ff;
            font-weight: 600;
        }
        .stat-item .num {
            color: #8ab4ff;
            font-weight: 700;
        }

        /* ─── Glow Divider ─── */
        .divider-glow {
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(100, 160, 255, 0.15), transparent);
            margin: 2rem 0 0.6rem;
        }

        /* ─── Footer ─── */
        .footer-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 1rem;
            margin-top: 2.4rem;
            font-size: 0.78rem;
            color: #556688;
            border-top: 1px solid rgba(255, 255, 255, 0.03);
            padding-top: 1.8rem;
        }
        .footer-meta a {
            color: #7aaaff;
            text-decoration: none;
            transition: color 0.2s;
        }
        .footer-meta a:hover {
            color: #b8ceff;
            text-decoration: underline;
        }
        .footer-meta .badge-group {
            display: flex;
            gap: 0.8rem;
            flex-wrap: wrap;
        }
        .footer-meta .badge-group span {
            background: rgba(255, 255, 255, 0.03);
            padding: 0.2rem 0.9rem;
            border-radius: 30px;
            border: 1px solid rgba(255, 255, 255, 0.04);
        }

        /* ─── Responsive ─── */
        @media (max-width: 720px) {
            .profile-card {
                padding: 1.6rem 1.2rem 2rem;
                border-radius: 1.6rem;
            }
            .grid-2 {
                grid-template-columns: 1fr;
                gap: 1.2rem;
            }
            .scene-wrapper {
                height: 220px;
            }
            .scene-overlay h1 {
                font-size: 1.3rem;
            }
            .subhead {
                max-width: 100%;
                font-size: 0.85rem;
            }
            .stats-row {
                gap: 0.6rem;
            }
            .stat-item {
                font-size: 0.7rem;
                padding: 0.25rem 0.8rem;
            }
            .footer-meta {
                flex-direction: column;
                align-items: flex-start;
            }
        }

        @media (max-width: 450px) {
            .scene-wrapper {
                height: 170px;
            }
            .scene-overlay {
                bottom: 1rem;
                left: 1.2rem;
            }
            .scene-overlay h1 {
                font-size: 1rem;
            }
            .scene-overlay .badge {
                font-size: 0.55rem;
                padding: 0.2rem 0.8rem;
            }
            h2 {
                font-size: 1.05rem;
            }
        }

        /* ─── Scrollbar ─── */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0d15;
        }
        ::-webkit-scrollbar-thumb {
            background: #2a3a5a;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #3a4a6a;
        }
    </style>
</head>
<body>

    <div class="profile-card">

        <!-- ═══ 3D SCENE ═══ -->
        <div class="scene-wrapper">
            <div id="three-canvas"></div>
            <div class="scene-overlay">
                <span class="badge">✦ interactive 3D</span>
                <h1>Advanced Programming<br />Portfolio</h1>
            </div>
        </div>

        <!-- ═══ INTRO ═══ -->
        <div style="display:flex; justify-content:space-between; align-items:flex-start; flex-wrap:wrap; gap:1rem;">
            <div>
                <h2 style="margin-top:0;">
                    <span>🚀</span> Alex Rivera
                    <span class="accent-line"></span>
                </h2>
                <p class="subhead">
                    Senior Software Engineer · 8+ years crafting high‑performance
                    systems, 3D visualisations &amp; developer tooling.
                </p>
            </div>
            <div style="display:flex; gap:0.6rem; flex-wrap:wrap;">
                <span style="background:rgba(60,200,120,0.08); border:1px solid rgba(60,200,120,0.08); padding:0.25rem 1rem; border-radius:40px; font-size:0.7rem; color:#8cd4a8;">● Open to work</span>
                <span style="background:rgba(255,200,60,0.06); border:1px solid rgba(255,200,60,0.06); padding:0.25rem 1rem; border-radius:40px; font-size:0.7rem; color:#e8c86a;">📍 Remote · UTC‑4</span>
            </div>
        </div>

        <!-- ═══ STATS ═══ -->
        <div class="stats-row">
            <span class="stat-item"><span class="num">12</span> years coding</span>
            <span class="stat-item"><span class="num">47</span> public repos</span>
            <span class="stat-item"><span class="num">2.3k</span> ⭐ stars</span>
            <span class="stat-item"><span class="num">6</span> languages</span>
        </div>

        <!-- ═══ SKILLS + PROJECTS ═══ -->
        <div class="grid-2">

            <!-- ── SKILLS ── -->
            <div>
                <h2>⚡ Core Stack <span class="accent-line"></span></h2>
                <div class="skill-tags">
                    <span class="skill-tag">C++</span>
                    <span class="skill-tag">Rust</span>
                    <span class="skill-tag">Python</span>
                    <span class="skill-tag">TypeScript</span>
                    <span class="skill-tag">Go</span>
                    <span class="skill-tag">WebGL</span>
                    <span class="skill-tag">Three.js</span>
                    <span class="skill-tag">CUDA</span>
                    <span class="skill-tag">TensorRT</span>
                    <span class="skill-tag">Docker</span>
                    <span class="skill-tag">Kubernetes</span>
                    <span class="skill-tag">Redis</span>
                    <span class="skill-tag">Postgres</span>
                    <span class="skill-tag">WebAssembly</span>
                    <span class="skill-tag">LLVM</span>
                    <span class="skill-tag">OpenGL</span>
                    <span class="skill-tag">Vulkan</span>
                    <span class="skill-tag">Nix</span>
                </div>

                <h2 style="margin-top:1.8rem;">🧰 Tooling <span class="accent-line"></span></h2>
                <div class="skill-tags">
                    <span class="skill-tag">VS Code</span>
                    <span class="skill-tag">Neovim</span>
                    <span class="skill-tag">Git</span>
                    <span class="skill-tag">GitHub Actions</span>
                    <span class="skill-tag">Terraform</span>
                    <span class="skill-tag">Prometheus</span>
                    <span class="skill-tag">Grafana</span>
                </div>
            </div>

            <!-- ── PROJECTS ── -->
            <div>
                <h2>📌 Featured <span class="accent-line"></span></h2>

                <div class="project-card">
                    <h3>voxel‑engine</h3>
                    <p>Real‑time voxel raytracer with global illumination &amp; LOD streaming.</p>
                    <span class="lang">C++ / Vulkan</span>
                </div>

                <div class="project-card" style="margin-top:0.8rem;">
                    <h3>schedulr</h3>
                    <p>Distributed task scheduler with low‑latency dispatch &amp; fault tolerance.</p>
                    <span class="lang">Rust / Redis</span>
                </div>

                <div class="project-card" style="margin-top:0.8rem;">
                    <h3>webgl‑sandbox</h3>
                    <p>Interactive 3D shader editor with real‑time GLSL compilation.</p>
                    <span class="lang">TypeScript / WebGL</span>
                </div>

                <div style="margin-top:1rem; font-size:0.78rem; color:#556688;">
                    <a href="#" style="color:#7aaaff; text-decoration:none;">→ view all projects</a>
                </div>
            </div>

        </div>

        <!-- ═══ DIVIDER ═══ -->
        <div class="divider-glow"></div>

        <!-- ═══ BOTTOM ROW ═══ -->
        <div style="display:flex; flex-wrap:wrap; gap:1.2rem 2.2rem; margin-top:0.2rem;">

            <div style="flex:1; min-width:160px;">
                <div style="font-size:0.7rem; text-transform:uppercase; letter-spacing:0.08em; color:#445577; font-weight:600;">Recent Activity</div>
                <ul style="list-style:none; margin-top:0.3rem; font-size:0.85rem; color:#99aacd;">
                    <li style="padding:0.2rem 0;">▸ Merged PR #142 in <span style="color:#b8ceff;">voxel-engine</span></li>
                    <li style="padding:0.2rem 0;">▸ Released v2.1.0 of <span style="color:#b8ceff;">schedulr</span></li>
                    <li style="padding:0.2rem 0;">▸ Opened RFC on WebAssembly GC</li>
                </ul>
            </div>

            <div style="flex:1; min-width:140px;">
                <div style="font-size:0.7rem; text-transform:uppercase; letter-spacing:0.08em; color:#445577; font-weight:600;">Connect</div>
                <div style="display:flex; flex-wrap:wrap; gap:0.6rem; margin-top:0.3rem; font-size:0.85rem;">
                    <a href="#" style="color:#7aaaff; text-decoration:none;">GitHub</a>
                    <a href="#" style="color:#7aaaff; text-decoration:none;">LinkedIn</a>
                    <a href="#" style="color:#7aaaff; text-decoration:none;">StackOverflow</a>
                    <a href="#" style="color:#7aaaff; text-decoration:none;">Dev.to</a>
                </div>
            </div>

        </div>

        <!-- ═══ FOOTER ═══ -->
        <div class="footer-meta">
            <span>© 2026 · Alex Rivera · <span style="color:#445577;">Advanced Programming</span></span>
            <div class="badge-group">
                <span>⚡ 3D Portfolio</span>
                <span>#built‑with‑threejs</span>
                <span>🚧 WIP</span>
            </div>
        </div>

    </div>

    <!-- ════════════════════════════════════════════════════════════ -->
    <!--  THREE.JS  –  3D ROTATING CUBE WITH NEON EDGES & PARTICLES  -->
    <!-- ════════════════════════════════════════════════════════════ -->
    <script>
        (function() {

            // ─── Container ───
            const container = document.getElementById('three-canvas');
            const rect = container.getBoundingClientRect();
            const width = container.clientWidth;
            const height = container.clientHeight;

            // ─── Scene ───
            const scene = new THREE.Scene();
            scene.background = null; // transparent

            // ─── Camera ───
            const camera = new THREE.PerspectiveCamera(40, width / height, 0.1, 100);
            camera.position.set(4.2, 2.8, 5.6);
            camera.lookAt(0, 0, 0);

            // ─── Renderer ───
            const renderer = new THREE.WebGLRenderer({
                antialias: true,
                alpha: true,
            });
            renderer.setSize(width, height);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
            renderer.shadowMap.enabled = true;
            renderer.shadowMap.type = THREE.PCFSoftShadowMap;
            renderer.toneMapping = THREE.ACESFilmicToneMapping;
            renderer.toneMappingExposure = 1.2;
            container.appendChild(renderer.domElement);

            // ─── Lights ───
            const ambient = new THREE.AmbientLight(0x223366, 0.6);
            scene.add(ambient);

            const keyLight = new THREE.DirectionalLight(0x8ab4ff, 1.8);
            keyLight.position.set(5, 8, 6);
            keyLight.castShadow = true;
            keyLight.shadow.mapSize.width = 512;
            keyLight.shadow.mapSize.height = 512;
            scene.add(keyLight);

            const fillLight = new THREE.DirectionalLight(0x4466aa, 0.6);
            fillLight.position.set(-4, 1, -3);
            scene.add(fillLight);

            const rimLight = new THREE.DirectionalLight(0x88ccff, 0.4);
            rimLight.position.set(-2, -3, 5);
            scene.add(rimLight);

            // ─── The Cube ───
            const cubeGroup = new THREE.Group();
            scene.add(cubeGroup);

            // -- Main box (translucent) --
            const boxGeo = new THREE.BoxGeometry(1.8, 1.8, 1.8);
            const boxMat = new THREE.MeshPhysicalMaterial({
                color: 0x1a2a4a,
                metalness: 0.2,
                roughness: 0.15,
                transparent: true,
                opacity: 0.35,
                envMapIntensity: 0.6,
                clearcoat: 0.4,
                clearcoatRoughness: 0.3,
                side: THREE.DoubleSide,
            });
            const box = new THREE.Mesh(boxGeo, boxMat);
            box.castShadow = true;
            box.receiveShadow = true;
            cubeGroup.add(box);

            // -- Inner core (glow) --
            const coreGeo = new THREE.BoxGeometry(1.3, 1.3, 1.3);
            const coreMat = new THREE.MeshPhysicalMaterial({
                color: 0x2a5aaa,
                emissive: 0x1a3a8a,
                emissiveIntensity: 0.25,
                metalness: 0.6,
                roughness: 0.1,
                transparent: true,
                opacity: 0.5,
                side: THREE.DoubleSide,
            });
            const core = new THREE.Mesh(coreGeo, coreMat);
            core.castShadow = false;
            cubeGroup.add(core);

            // -- Neon edges --
            const edgesGeo = new THREE.EdgesGeometry(boxGeo);
            const edgesMat = new THREE.LineBasicMaterial({
                color: 0x4a8aff,
                transparent: true,
                opacity: 0.7,
            });
            const edges = new THREE.LineSegments(edgesGeo, edgesMat);
            cubeGroup.add(edges);

            // -- Glow edges (wider) --
            const glowEdgesMat = new THREE.LineBasicMaterial({
                color: 0x1a5aff,
                transparent: true,
                opacity: 0.2,
            });
            const glowEdges = new THREE.LineSegments(edgesGeo.clone(), glowEdgesMat);
            glowEdges.scale.set(1.08, 1.08, 1.08);
            cubeGroup.add(glowEdges);

            // -- Floating text labels (sprites) --
            function makeLabel(text, color = '#7aaaff', size = 0.6) {
                const canvas = document.createElement('canvas');
                canvas.width = 256;
                canvas.height = 128;
                const ctx = canvas.getContext('2d');

                ctx.clearRect(0, 0, canvas.width, canvas.height);

                // Glow
                ctx.shadowColor = 'rgba(70, 130, 255, 0.3)';
                ctx.shadowBlur = 20;

                ctx.font = 'bold 56px "Inter", "Segoe UI", system-ui, sans-serif';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';

                // Gradient text
                const grad = ctx.createLinearGradient(0, 20, 0, 100);
                grad.addColorStop(0, '#f0f6ff');
                grad.addColorStop(0.5, color);
                grad.addColorStop(1, '#4a7acc');
                ctx.fillStyle = grad;
                ctx.shadowColor = 'rgba(70, 130, 255, 0.4)';
                ctx.shadowBlur = 25;
                ctx.fillText(text, canvas.width / 2, canvas.height / 2 + 4);

                // Second pass for crispness
                ctx.shadowBlur = 0;
                ctx.fillStyle = color;
                ctx.globalAlpha = 0.9;
                ctx.fillText(text, canvas.width / 2, canvas.height / 2 + 2);

                const texture = new THREE.CanvasTexture(canvas);
                texture.needsUpdate = true;

                const spriteMat = new THREE.SpriteMaterial({
                    map: texture,
                    transparent: true,
                    depthWrite: false,
                    blending: THREE.AdditiveBlending,
                });
                const sprite = new THREE.Sprite(spriteMat);
                sprite.scale.set(size * 1.4, size * 0.7, 1);
                return sprite;
            }

            const labels = [
                { text: 'C++', pos: [1.0, 0, 0], color: '#6aaaff' },
                { text: 'Rust', pos: [-1.0, 0, 0], color: '#ff8a5a' },
                { text: 'WebGL', pos: [0, 1.0, 0], color: '#4ad4aa' },
                { text: 'CUDA', pos: [0, -1.0, 0], color: '#76c7ff' },
                { text: 'WASM', pos: [0, 0, 1.0], color: '#aa8aff' },
                { text: 'AI/ML', pos: [0, 0, -1.0], color: '#ff6a8a' },
            ];

            labels.forEach(({ text, pos, color }) => {
                const sprite = makeLabel(text, color, 0.75);
                sprite.position.set(pos[0] * 1.15, pos[1] * 1.15, pos[2] * 1.15);
                cubeGroup.add(sprite);
            });

            // ─── Particle System ───
            const particleCount = 600;
            const positions = new Float32Array(particleCount * 3);
            const sizes = new Float32Array(particleCount);
            const speeds = new Float32Array(particleCount);

            for (let i = 0; i < particleCount; i++) {
                const r = 2.8 + Math.random() * 3.2;
                const theta = Math.random() * Math.PI * 2;
                const phi = Math.acos(2 * Math.random() - 1);
                positions[i * 3] = r * Math.sin(phi) * Math.cos(theta);
                positions[i * 3 + 1] = r * Math.cos(phi);
                positions[i * 3 + 2] = r * Math.sin(phi) * Math.sin(theta);
                sizes[i] = 0.015 + Math.random() * 0.035;
                speeds[i] = 0.2 + Math.random() * 0.5;
            }

            const particleGeo = new THREE.BufferGeometry();
            particleGeo.setAttribute('position', new THREE.BufferAttribute(positions, 3));
            particleGeo.setAttribute('size', new THREE.BufferAttribute(sizes, 1));

            const particleMat = new THREE.PointsMaterial({
                color: 0x6a9aff,
                size: 0.04,
                transparent: true,
                opacity: 0.6,
                blending: THREE.AdditiveBlending,
                depthWrite: false,
                sizeAttenuation: true,
            });
            const particles = new THREE.Points(particleGeo, particleMat);
            scene.add(particles);

            // ─── Mouse Interaction ───
            let mouseX = 0,
                mouseY = 0;
            let targetRotX = 0,
                targetRotY = 0;
            let isDragging = false;
            let prevMouseX = 0,
                prevMouseY = 0;

            container.addEventListener('mousedown', (e) => {
                isDragging = true;
                prevMouseX = e.clientX;
                prevMouseY = e.clientY;
            });

            window.addEventListener('mouseup', () => { isDragging = false; });

            container.addEventListener('mousemove', (e) => {
                const rect = container.getBoundingClientRect();
                const x = (e.clientX - rect.left) / rect.width - 0.5;
                const y = (e.clientY - rect.top) / rect.height - 0.5;

                if (isDragging) {
                    const dx = e.clientX - prevMouseX;
                    const dy = e.clientY - prevMouseY;
                    targetRotY += dx * 0.005;
                    targetRotX += dy * 0.005;
                    prevMouseX = e.clientX;
                    prevMouseY = e.clientY;
                } else {
                    // subtle parallax
                    targetRotY = x * 0.8;
                    targetRotX = y * 0.4;
                }
            });

            // Touch support
            let touchId = null;
            container.addEventListener('touchstart', (e) => {
                const touch = e.changedTouches[0];
                touchId = touch.identifier;
                isDragging = true;
                prevMouseX = touch.clientX;
                prevMouseY = touch.clientY;
            }, { passive: true });

            container.addEventListener('touchmove', (e) => {
                e.preventDefault();
                const touch = Array.from(e.changedTouches).find(t => t.identifier === touchId);
                if (!touch) return;
                const dx = touch.clientX - prevMouseX;
                const dy = touch.clientY - prevMouseY;
                targetRotY += dx * 0.005;
                targetRotX += dy * 0.005;
                prevMouseX = touch.clientX;
                prevMouseY = touch.clientY;
            }, { passive: false });

            container.addEventListener('touchend', () => {
                isDragging = false;
                touchId = null;
            }, { passive: true });

            // ─── Resize ───
            function resize() {
                const w = container.clientWidth;
                const h = container.clientHeight;
                if (w === 0 || h === 0) return;
                camera.aspect = w / h;
                camera.updateProjectionMatrix();
                renderer.setSize(w, h);
            }

            window.addEventListener('resize', resize);
            // Also observe container size changes
            if (window.ResizeObserver) {
                const ro = new ResizeObserver(() => resize());
                ro.observe(container);
            }

            // ─── Animation Loop ───
            let time = 0;

            function animate() {
                requestAnimationFrame(animate);
                time += 0.005;

                // Smooth rotation
                cubeGroup.rotation.x += (targetRotX - cubeGroup.rotation.x) * 0.06;
                cubeGroup.rotation.y += (targetRotY - cubeGroup.rotation.y) * 0.06;

                // Idle float
                if (!isDragging) {
                    cubeGroup.position.y = Math.sin(time * 0.6) * 0.12;
                }

                // Rotate particles slowly
                particles.rotation.x += 0.0003;
                particles.rotation.y += 0.0005;

                // Pulse edges
                const pulse = 0.6 + 0.4 * Math.sin(time * 1.2);
                edges.material.opacity = 0.4 + 0.3 * pulse;
                glowEdges.material.opacity = 0.1 + 0.15 * pulse;

                // Core pulse
                core.material.emissiveIntensity = 0.15 + 0.2 * Math.sin(time * 0.8);

                renderer.render(scene, camera);
            }

            animate();

            // ─── Initial auto-rotation ───
            targetRotY = 0.4;
            targetRotX = 0.15;

            // expose for debugging
            window.__scene = scene;

        })();
    </script>

</body>
</html>
