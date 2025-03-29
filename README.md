# peluqueria
01// Archivo: service-worker.js
self.addEventListener('install', event => {
    event.waitUntil(
        caches.open('pwa-cache-v1').then(cache => {
            return cache.addAll([
                '/',
                '/index.html',
                '/styles.css',
                '/app.js'
            ]);
        })
    );
});

self.addEventListener('fetch', event => {
    event.respondWith(
        caches.match(event.request).then(response => {
            return response || fetch(event.request);
        })
    );
});

// Archivo: index.html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Peluquería PWA</title>
    <link rel="stylesheet" href="styles.css">
    <script defer src="app.js"></script>
</head>
<body>
    <h1>Gestión de Citas</h1>
    <form id="appointment-form">
        <label for="date">Fecha:</label>
        <input type="date" id="date" required>
        
        <label for="time">Hora:</label>
        <input type="time" id="time" required>
        
        <button type="submit">Agendar Cita</button>
    </form>
    
    <ul id="appointment-list"></ul>

    <script>
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('/service-worker.js')
                .then(() => console.log('Service Worker registrado.'))
                .catch(err => console.error('Error al registrar el Service Worker', err));
        }
    </script>
</body>
</html>

