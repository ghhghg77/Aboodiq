<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>A - Video Optimizer</title>
    <script src="https://unpkg.com/@ffmpeg/ffmpeg@0.11.6/dist/ffmpeg.min.js"></script>
    <style>
        body { background: #000; color: #fff; font-family: sans-serif; text-align: center; padding-top: 50px; }
        .logo { font-size: 120px; font-weight: 900; background: linear-gradient(45deg, #a855f7, #3b82f6); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .btn { background: #3b82f6; color: #white; padding: 15px 30px; border-radius: 10px; cursor: pointer; display: inline-block; margin-top: 20px; text-decoration: none; font-weight: bold; }
        #downloadBtn { background: #22c55e; display: none; } /* زر التحميل أخضر */
        #status { margin-top: 20px; color: #888; }
        .loader { border: 4px solid #f3f3f3; border-top: 4px solid #3498db; border-radius: 50%; width: 30px; height: 30px; animation: spin 2s linear infinite; display: none; margin: 10px auto; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body>

    <div class="logo">A</div>
    <h2>معالج تيك توك الذكي</h2>
    
    <input type="file" id="uploader" accept="video/*" style="display:none">
    <label for="uploader" class="btn">اختر فيديو (4K/120fps)</label>
    
    <div class="loader" id="loader"></div>
    <div id="status">جاهز للمعالجة...</div>

    <a id="downloadBtn" class="btn">تحميل الفيديو المعدل جاهز ✅</a>

    <script>
        const { createFFmpeg, fetchFile } = FFmpeg;
        const ffmpeg = createFFmpeg({ log: true });

        const uploader = document.getElementById('uploader');
        const status = document.getElementById('status');
        const loader = document.getElementById('loader');
        const downloadBtn = document.getElementById('downloadBtn');

        uploader.onchange = async (e) => {
            const file = e.target.files[0];
            if (!file) return;

            status.innerText = "جاري تحميل المحرك (قد يستغرق ثواني)...";
            loader.style.display = "block";
            downloadBtn.style.display = "none";

            if (!ffmpeg.isLoaded()) await ffmpeg.load();

            status.innerText = "جاري المعالجة الخداعية (1080p / 30fps)...";
            ffmpeg.FS('writeFile', 'input.mp4', await fetchFile(file));

            // الأمر السحري لتحويل الفريمات والجودة لخدعة تيك توك
            // -vf scale=1920:1080 (تغيير الدقة لـ Full HD)
            // -r 30 (تغيير الفريمات لـ 30fps)
            await ffmpeg.run('-i', 'input.mp4', '-vf', 'scale=1920:1080', '-r', '30', '-c:v', 'libx264', '-crf', '18', 'output.mp4');

            status.innerText = "اكتملت المعالجة!";
            loader.style.display = "none";

            const data = ffmpeg.FS('readFile', 'output.mp4');
            const url = URL.createObjectURL(new Blob([data.buffer], { type: 'video/mp4' }));
            
            // إظهار خيار التحميل
            downloadBtn.href = url;
            downloadBtn.download = "TikTok_Ready_A.mp4";
            downloadBtn.style.display = "inline-block";
        };
    </script>
</body>
</html>
