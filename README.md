<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Surat Rasi Bintang Hati - Simetris</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body, html {
            background: radial-gradient(circle at center, #0d0d26 0%, #030308 100%);
            overflow: hidden;
            width: 100%;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        canvas {
            display: block;
            width: 100vw;
            height: 100vh;
        }
    </style>
</head>
<body>

    <canvas id="starryCanvas"></canvas>

    <script>
        const canvas = document.getElementById('starryCanvas');
        const ctx = canvas.getContext('2d');

        let width = canvas.width = window.innerWidth;
        let height = canvas.height = window.innerHeight;

        // ========================================================
        // SILAHKAN UBAH ATAU TAMBAH SURAT CINTA KAMU DI SINI
        // ========================================================
        const loveLetter = "KAU ADALAH RASI BINTANG TERINDAH DI SEMESTAKU YANG SELALU MENERANGI KEGELAPAN JALANKU SETIAP DETAK JANTUNG DAN EMBUSAN NAFAS INI ADALAH BENTUK SYUKUR ATAS HADIRMU DI DUNIA INI AKU MENCINTAIMU LEBIH DARI LUASNYA GALAXY DAN BINTANG BINTANG DI LANGIT SEPANJANG MASA SELAMANYA";
        
        // Memecah kalimat menjadi deretan kata
        const words = loveLetter.split(" ");
        const totalWords = words.length;

        // --- 1. SETUP BINTANG LATAR BELAKANG (STARFIELD) ---
        const bgStars = [];
        const numBgStars = 250;

        for (let i = 0; i < numBgStars; i++) {
            bgStars.push({
                x: Math.random() * width,
                y: Math.random() * height,
                size: Math.random() * 1.5,
                twinkleSpeed: 0.01 + Math.random() * 0.02,
                brightness: Math.random()
            });
        }

        // --- 2. RUMUS MATEMATIKA KURVA HATI ---
        // Menghasilkan koordinat relatif (X, Y) dari pusat (0,0)
        function getHeartPoint(t, scale) {
            const x = scale * 16 * Math.pow(Math.sin(t), 3);
            const y = -scale * (13 * Math.cos(t) - 5 * Math.cos(2*t) - 2 * Math.cos(3*t) - Math.cos(4*t));
            return { x, y };
        }

        let time = 0;

        // --- 3. LOOP ANIMASI UTAMA ---
        function animate() {
            // Bersihkan layar dengan warna kosmik gelap
            ctx.fillStyle = '#030308';
            ctx.fillRect(0, 0, width, height);

            // Efek Nebula Halus di Tengah Layar
            const nebula = ctx.createRadialGradient(width/2, height/2, 10, width/2, height/2, Math.min(width, height)/2);
            nebula.addColorStop(0, 'rgba(26, 13, 61, 0.3)');
            nebula.addColorStop(1, 'rgba(3, 3, 8, 0)');
            ctx.fillStyle = nebula;
            ctx.fillRect(0, 0, width, height);

            // A. Gambar Bintang Latar Belakang
            bgStars.forEach(star => {
                star.brightness += star.twinkleSpeed;
                if (star.brightness > 1 || star.brightness < 0) {
                    star.twinkleSpeed = -star.twinkleSpeed;
                }
                ctx.fillStyle = `rgba(255, 255, 255, ${Math.abs(star.brightness)})`;
                ctx.beginPath();
                ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2);
                ctx.fill();
            });

            // B. Gambar Rasi Teks Berbentuk Hati (Presisi di Tengah)
            time += 0.015; // Kecepatan denyut jantung

            // Skala dinamis responsif agar pas di layar apa saja
            const baseScale = Math.min(width, height) / 48;
            const pulseScale = baseScale + Math.sin(time * 1.5) * 0.4; // Efek denyutan

            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.font = `bold ${Math.max(11, baseScale * 0.45)}px 'Courier New', Courier, monospace`;

            words.forEach((word, index) => {
                // Sebarkan kata secara merata mengelilingi kurva hati
                const t = (index / totalWords) * Math.PI * 2 - Math.PI / 2;
                const pos = getHeartPoint(t, pulseScale);

                // Titik koordinat absolut dikunci total dari tengah layar (width / 2 dan height / 2)
                const targetX = width / 2 + pos.x;
                const targetY = height / 2 + pos.y;

                // Efek kerlipan acak antar kata rasi bintang
                const wordTwinkle = 0.7 + Math.sin(time + index) * 0.3;

                // Gambar Efek Cahaya Neon Putih-Kebiruan (Glow)
                ctx.shadowBlur = 10;
                ctx.shadowColor = 'rgba(255, 255, 255, 0.8)';
                ctx.fillStyle = `rgba(255, 255, 255, ${wordTwinkle})`;
                
                // Cetak Kata ke Canvas
                ctx.fillText(word, targetX, targetY);
                
                // Reset shadow agar tidak merusak performa
                ctx.shadowBlur = 0; 
            });

            requestAnimationFrame(animate);
        }

        // --- 4. HANDLING RESPONSIVE LAYAR ---
        window.addEventListener('resize', () => {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
            
            // Re-generasi bintang background agar merata ulang saat resize
            bgStars.length = 0;
            for (let i = 0; i < numBgStars; i++) {
                bgStars.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    size: Math.random() * 1.5,
                    twinkleSpeed: 0.01 + Math.random() * 0.02,
                    brightness: Math.random()
                });
            }
        });

        // Jalankan Animasi
        animate();
    </script>
</body>
</html>
