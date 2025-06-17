# AI-OBJECT-DETECTION-
<!DOCTYPE html>
<html lang="en">

<head>
    <title>AI Object Detection with Analysis</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            text-align: center;
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: white;
            margin: 0;
            padding: 20px;
            min-height: 100vh;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
        }

        h1 {
            color: #4CAF50;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        .main-content {
            display: grid;
            grid-template-columns: 1fr 400px;
            gap: 20px;
            align-items: start;
        }

        .camera-section {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .analysis-panel {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 20px;
            height: fit-content;
            min-height: 500px;
        }

        #status {
            padding: 15px;
            margin: 20px 0;
            border-radius: 8px;
            font-size: 16px;
            font-weight: bold;
        }

        .status-loading {
            background: rgba(255, 193, 7, 0.2);
            border: 2px solid #FFC107;
            color: #FFC107;
        }

        .status-ready {
            background: rgba(76, 175, 80, 0.2);
            border: 2px solid #4CAF50;
            color: #4CAF50;
        }

        .status-error {
            background: rgba(244, 67, 54, 0.2);
            border: 2px solid #F44336;
            color: #F44336;
        }

        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255, 255, 255, .3);
            border-radius: 50%;
            border-top-color: #FFC107;
            animation: spin 1s ease-in-out infinite;
            margin-right: 10px;
        }

        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }

        video {
            display: none;
        }

        #canvas {
            border: 2px solid #4CAF50;
            border-radius: 10px;
            max-width: 100%;
            height: auto;
            background: #000;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            min-height: 300px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: rgba(255, 255, 255, 0.5);
        }

        .controls {
            margin: 20px 0;
            padding: 20px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
        }

        .control-group {
            margin: 15px 0;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }

        .control-group label {
            font-weight: bold;
            min-width: 120px;
            text-align: right;
        }

        button {
            background: #4CAF50;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
        }

        button:hover {
            background: #45a049;
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }

        button:disabled {
            background: #666;
            cursor: not-allowed;
            transform: none;
        }

        .toggle-btn {
            background: #f44336;
        }

        .toggle-btn.active {
            background: #4CAF50;
        }

        input[type="range"] {
            width: 200px;
            margin: 0 10px;
        }

        .stats {
            margin: 20px 0;
            padding: 15px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
        }

        .stat-item {
            text-align: center;
            padding: 10px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
        }

        .analysis-header {
            font-size: 1.2em;
            font-weight: bold;
            margin-bottom: 20px;
            color: #4CAF50;
            border-bottom: 2px solid #4CAF50;
            padding-bottom: 10px;
        }

        .detected-objects {
            max-height: 400px;
            overflow-y: auto;
            padding-right: 10px;
        }

        .object-card {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 15px;
            border-left: 4px solid #4CAF50;
            transition: all 0.3s;
        }

        .object-card:hover {
            background: rgba(255, 255, 255, 0.15);
            transform: translateX(5px);
        }

        .object-name {
            font-size: 1.1em;
            font-weight: bold;
            color: #4CAF50;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .confidence-badge {
            background: #4CAF50;
            color: white;
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 0.8em;
            font-weight: bold;
        }

        .object-description {
            font-size: 0.9em;
            line-height: 1.4;
            margin: 8px 0;
            color: rgba(255, 255, 255, 0.8);
        }

        .object-properties {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
            gap: 8px;
            margin-top: 10px;
        }

        .property-tag {
            background: rgba(76, 175, 80, 0.2);
            border: 1px solid #4CAF50;
            color: #4CAF50;
            padding: 4px 8px;
            border-radius: 15px;
            font-size: 0.7em;
            text-align: center;
        }

        .no-objects {
            text-align: center;
            padding: 40px 20px;
            color: rgba(255, 255, 255, 0.5);
            font-style: italic;
        }

        .debug-info {
            margin: 20px 0;
            padding: 15px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            font-family: monospace;
            font-size: 12px;
            text-align: left;
            max-height: 150px;
            overflow-y: auto;
        }

        .library-status {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            padding: 15px;
            margin: 15px 0;
            font-size: 14px;
        }

        .library-loading {
            border-left: 4px solid #FFC107;
            color: #FFC107;
        }

        .library-loaded {
            border-left: 4px solid #4CAF50;
            color: #4CAF50;
        }

        .library-error {
            border-left: 4px solid #F44336;
            color: #F44336;
        }

        .retry-btn {
            background: #FF9800;
            margin-top: 10px;
        }

        .retry-btn:hover {
            background: #F57C00;
        }

        @media (max-width: 1024px) {
            .main-content {
                grid-template-columns: 1fr;
            }

            .analysis-panel {
                order: -1;
                min-height: 300px;
            }

            .detected-objects {
                max-height: 250px;
            }
        }

        @media (max-width: 768px) {
            .container {
                margin: 10px;
                padding: 15px;
            }

            .control-group {
                flex-direction: column;
                gap: 10px;
            }

            .control-group label {
                min-width: auto;
                text-align: center;
            }

            .stats {
                grid-template-columns: 1fr;
            }
        }

        /* Scrollbar styling */
        .detected-objects::-webkit-scrollbar {
            width: 6px;
        }

        .detected-objects::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 3px;
        }

        .detected-objects::-webkit-scrollbar-thumb {
            background: #4CAF50;
            border-radius: 3px;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>🤖 AI Object Detection & Analysis</h1>

        <div id="libraryStatus" class="library-status library-loading">
            <div class="loading-spinner"></div>
            Loading AI Detection Library... This may take a moment.
        </div>

        <div id="status" class="status-loading">
            <div class="loading-spinner"></div>
            Waiting for AI library to load...
        </div>

        <div class="main-content">
            <div class="camera-section">
                <video id="video" autoplay muted playsinline></video>
                <canvas id="canvas">Camera feed will appear here</canvas>

                <div class="controls">
                    <div class="control-group">
                        <label>Camera:</label>
                        <button id="startCamera" disabled>Start Camera</button>
                        <button id="stopCamera" disabled>Stop Camera</button>
                    </div>

                    <div class="control-group">
                        <label>AI Detection:</label>
                        <button id="toggleAI" class="toggle-btn" disabled>Start Detection</button>
                    </div>

                    <div class="control-group">
                        <label>Detection Speed:</label>
                        <input type="range" id="speedSlider" min="500" max="3000" value="1000" disabled>
                        <span id="speedValue">1000ms</span>
                    </div>
                </div>

                <div class="stats">
                    <div class="stat-item">
                        <div>Objects Detected</div>
                        <div style="font-size: 1.5em; font-weight: bold; color: #4CAF50;" id="objectCount">0</div>
                    </div>
                    <div class="stat-item">
                        <div>FPS</div>
                        <div style="font-size: 1.5em; font-weight: bold; color: #4CAF50;" id="fpsDisplay">0</div>
                    </div>
                    <div class="stat-item">
                        <div>Status</div>
                        <div style="font-size: 1.2em; font-weight: bold;" id="detectionStatus">Loading</div>
                    </div>
                </div>
            </div>

            <div class="analysis-panel">
                <div class="analysis-header">📊 Object Analysis</div>
                <div class="detected-objects" id="detectedObjects">
                    <div class="no-objects">
                        Waiting for AI library to load...
                    </div>
                </div>
            </div>
        </div>

        <div class="debug-info" id="debugInfo">
            <div><strong>Debug Log:</strong></div>
            <div>Initializing application...</div>
        </div>
    </div>

    <script>
        // Global variables
        let video, canvas, ctx;
        let objectDetector = null;
        let isDetecting = false;
        let detectionInterval = null;
        let stream = null;
        let lastFrameTime = 0;
        let frameCount = 0;
        let detectionSpeed = 1000;
        let currentObjects = [];
        let ml5Loaded = false;
        let libraryLoadAttempts = 0;
        const maxLoadAttempts = 3;

        // Enhanced logging
        function debugLog(message) {
            const debugDiv = document.getElementById('debugInfo');
            const time = new Date().toLocaleTimeString();
            debugDiv.innerHTML += `<div>[${time}] ${message}</div>`;
            debugDiv.scrollTop = debugDiv.scrollHeight;
            console.log(`[${time}] ${message}`);
        }

        // Update status displays
        function updateStatus(message, type = 'loading') {
            const statusDiv = document.getElementById('status');
            const detectionStatusDiv = document.getElementById('detectionStatus');

            statusDiv.className = `status-${type}`;
            if (type === 'loading') {
                statusDiv.innerHTML = `<div class="loading-spinner"></div>${message}`;
            } else {
                statusDiv.innerHTML = message;
            }

            detectionStatusDiv.textContent = type === 'ready' ? 'Ready' :
                type === 'error' ? 'Error' : 'Loading';
        }

        function updateLibraryStatus(message, type = 'loading') {
            const libraryDiv = document.getElementById('libraryStatus');
            libraryDiv.className = `library-status library-${type}`;

            if (type === 'loading') {
                libraryDiv.innerHTML = `<div class="loading-spinner"></div>${message}`;
            } else if (type === 'error') {
                libraryDiv.innerHTML = `
          <div>${message}</div>
          <button class="retry-btn" onclick="loadML5Library()">Retry Loading</button>
        `;
            } else {
                libraryDiv.innerHTML = message;
            }
        }

        // Load ML5 library with timeout and retry logic
        function loadML5Library() {
            libraryLoadAttempts++;
            debugLog(`Attempting to load ML5 library (attempt ${libraryLoadAttempts}/${maxLoadAttempts})`);
            updateLibraryStatus(`Loading AI library... (Attempt ${libraryLoadAttempts}/${maxLoadAttempts})`);

            // Remove existing script if present
            const existingScript = document.querySelector('script[src*="ml5"]');
            if (existingScript) {
                existingScript.remove();
            }

            const script = document.createElement('script');
            script.src = 'https://unpkg.com/ml5@0.12.2/dist/ml5.min.js';

            const timeout = setTimeout(() => {
                debugLog('ML5 library loading timed out');
                if (libraryLoadAttempts < maxLoadAttempts) {
                    setTimeout(() => loadML5Library(), 2000);
                } else {
                    updateLibraryStatus('Failed to load AI library. Please check your internet connection.', 'error');
                    updateStatus('AI library failed to load. Some features may be unavailable.', 'error');
                }
            }, 15000); // 15 second timeout

            script.onload = () => {
                clearTimeout(timeout);
                debugLog('ML5 library loaded successfully');
                updateLibraryStatus('AI library loaded successfully!', 'loaded');
                ml5Loaded = true;
                initializeApp();
            };

            script.onerror = () => {
                clearTimeout(timeout);
                debugLog('Failed to load ML5 library');
                if (libraryLoadAttempts < maxLoadAttempts) {
                    setTimeout(() => loadML5Library(), 2000);
                } else {
                    updateLibraryStatus('Failed to load AI library. Please check your internet connection.', 'error');
                    updateStatus('AI library failed to load. Some features may be unavailable.', 'error');
                }
            };

            document.head.appendChild(script);
        }

        // Object database (condensed for performance)
        const objectDatabase = {
            person: { name: "Person", category: "Human", description: "A human being detected in the scene.", properties: ["Living", "Mobile"], icon: "👤", color: "#FF6B6B" },
            car: { name: "Car", category: "Vehicle", description: "A motor vehicle for transportation.", properties: ["Transport", "Motor"], icon: "🚗", color: "#45B7D1" },
            bicycle: { name: "Bicycle", category: "Vehicle", description: "A two-wheeled eco-friendly vehicle.", properties: ["Transport", "Eco-friendly"], icon: "🚲", color: "#4ECDC4" },
            dog: { name: "Dog", category: "Pet", description: "A domesticated companion animal.", properties: ["Pet", "Domestic"], icon: "🐕", color: "#E17055" },
            cat: { name: "Cat", category: "Pet", description: "A small domesticated feline.", properties: ["Pet", "Domestic"], icon: "🐱", color: "#FDCB6E" },
            chair: { name: "Chair", category: "Furniture", description: "Seating furniture for one person.", properties: ["Furniture", "Seating"], icon: "🪑", color: "#8D6E63" },
            bottle: { name: "Bottle", category: "Container", description: "A container for liquids.", properties: ["Container", "Storage"], icon: "🍶", color: "#009688" },
            cup: { name: "Cup", category: "Drinkware", description: "A small drinking container.", properties: ["Drinkware", "Beverage"], icon: "☕", color: "#795548" }
        };

        function getObjectInfo(className) {
            return objectDatabase[className] || {
                name: className.charAt(0).toUpperCase() + className.slice(1).replace('_', ' '),
                category: "Unknown",
                description: `A ${className.replace('_', ' ')} object detected in the scene.`,
                properties: ["Detected"],
                icon: "❓",
                color: "#9E9E9E"
            };
        }

        // Initialize the application
        function initializeApp() {
            if (!ml5Loaded) {
                debugLog('Cannot initialize app - ML5 not loaded');
                return;
            }

            debugLog('Initializing application...');
            updateStatus('Initializing components...');

            video = document.getElementById('video');
            canvas = document.getElementById('canvas');
            ctx = canvas.getContext('2d');

            // Set up event listeners
            document.getElementById('startCamera').addEventListener('click', startCamera);
            document.getElementById('stopCamera').addEventListener('click', stopCamera);
            document.getElementById('toggleAI').addEventListener('click', toggleDetection);

            const speedSlider = document.getElementById('speedSlider');
            speedSlider.addEventListener('input', (e) => {
                detectionSpeed = parseInt(e.target.value);
                document.getElementById('speedValue').textContent = detectionSpeed + 'ms';

                if (isDetecting) {
                    clearInterval(detectionInterval);
                    detectionInterval = setInterval(detectObjects, detectionSpeed);
                }
            });

            document.getElementById('startCamera').disabled = false;
            updateStatus('Ready! Click "Start Camera" to begin.', 'ready');

            document.getElementById('detectedObjects').innerHTML = '<div class="no-objects">Click "Start Camera" to begin detection</div>';

            debugLog('Application initialized successfully');
        }

        async function startCamera() {
            try {
                debugLog('Starting camera...');
                updateStatus('Starting camera...');

                const constraints = {
                    video: {
                        width: { ideal: 640 },
                        height: { ideal: 480 },
                        facingMode: 'environment'
                    }
                };

                stream = await navigator.mediaDevices.getUserMedia(constraints);
                video.srcObject = stream;

                video.onloadedmetadata = () => {
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;
                    canvas.style.display = 'block';

                    document.getElementById('startCamera').disabled = true;
                    document.getElementById('stopCamera').disabled = false;
                    document.getElementById('toggleAI').disabled = false;
                    document.getElementById('speedSlider').disabled = false;

                    updateStatus('Camera started! Click "Start Detection" to begin AI analysis.', 'ready');
                    debugLog('Camera started successfully');

                    // Start drawing video feed
                    drawVideoFeed();
                };

            } catch (error) {
                debugLog(`Camera error: ${error.message}`);
                updateStatus('Camera access denied or unavailable.', 'error');
            }
        }

        function stopCamera() {
            if (stream) {
                stream.getTracks().forEach(track => track.stop());
                stream = null;
            }

            if (isDetecting) {
                stopDetection();
            }

            document.getElementById('startCamera').disabled = false;
            document.getElementById('stopCamera').disabled = true;
            document.getElementById('toggleAI').disabled = true;
            document.getElementById('speedSlider').disabled = true;

            canvas.style.display = 'none';
            updateStatus('Camera stopped.', 'ready');
            debugLog('Camera stopped');
        }

        function drawVideoFeed() {
            if (video.readyState === video.HAVE_ENOUGH_DATA) {
                ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
            }

            if (stream && stream.active) {
                requestAnimationFrame(drawVideoFeed);
            }
        }

        function toggleDetection() {
            if (isDetecting) {
                stopDetection();
            } else {
                startDetection();
            }
        }

        async function startDetection() {
            if (!ml5Loaded) {
                updateStatus('AI library not loaded yet.', 'error');
                return;
            }

            try {
                debugLog('Initializing object detector...');
                updateStatus('Loading AI model...');

                objectDetector = await ml5.objectDetector('cocossd');

                isDetecting = true;
                detectionInterval = setInterval(detectObjects, detectionSpeed);

                const toggleBtn = document.getElementById('toggleAI');
                toggleBtn.textContent = 'Stop Detection';
                toggleBtn.classList.add('active');

                updateStatus('AI detection active!', 'ready');
                debugLog('Object detection started');

            } catch (error) {
                debugLog(`Detection error: ${error.message}`);
                updateStatus('Failed to start AI detection.', 'error');
            }
        }

        function stopDetection() {
            isDetecting = false;
            if (detectionInterval) {
                clearInterval(detectionInterval);
                detectionInterval = null;
            }

            const toggleBtn = document.getElementById('toggleAI');
            toggleBtn.textContent = 'Start Detection';
            toggleBtn.classList.remove('active');

            updateStatus('Detection stopped.', 'ready');
            debugLog('Object detection stopped');
        }

        function detectObjects() {
            if (!objectDetector || !video.readyState) return;

            objectDetector.detect(video, (error, results) => {
                if (error) {
                    debugLog(`Detection error: ${error.message}`);
                    return;
                }

                currentObjects = results || [];

                // Update UI
                document.getElementById('objectCount').textContent = currentObjects.length;
                updateFPS();
                displayDetectedObjects();
                drawBoundingBoxes();
            });
        }

        function updateFPS() {
            const now = performance.now();
            if (lastFrameTime) {
                const fps = Math.round(1000 / (now - lastFrameTime));
                document.getElementById('fpsDisplay').textContent = fps;
            }
            lastFrameTime = now;
        }

        function displayDetectedObjects() {
            const container = document.getElementById('detectedObjects');

            if (currentObjects.length === 0) {
                container.innerHTML = '<div class="no-objects">No objects detected</div>';
                return;
            }

            const uniqueObjects = {};
            currentObjects.forEach(obj => {
                const key = obj.label;
                if (!uniqueObjects[key] || obj.confidence > uniqueObjects[key].confidence) {
                    uniqueObjects[key] = obj;
                }
            });

            container.innerHTML = Object.values(uniqueObjects)
                .sort((a, b) => b.confidence - a.confidence)
                .map(obj => {
                    const info = getObjectInfo(obj.label);
                    const confidence = Math.round(obj.confidence * 100);

                    return `
            <div class="object-card">
              <div class="object-name">
                <span>${info.icon}</span>
                <span>${info.name}</span>
                <span class="confidence-badge">${confidence}%</span>
              </div>
              <div class="object-description">${info.description}</div>
              <div class="object-properties">
                ${info.properties.map(prop => `<div class="property-tag">${prop}</div>`).join('')}
              </div>
            </div>
          `;
                }).join('');
        }

        function drawBoundingBoxes() {
            // Redraw video frame
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

            // Draw bounding boxes
            currentObjects.forEach(obj => {
                const info = getObjectInfo(obj.label);
                ctx.strokeStyle = info.color;
                ctx.lineWidth = 3;
                ctx.strokeRect(obj.x, obj.y, obj.width, obj.height);

                // Draw label
                ctx.fillStyle = info.color;
                ctx.font = '16px Arial';
                const text = `${info.name} ${Math.round(obj.confidence * 100)}%`;
                const textWidth = ctx.measureText(text).width;

                ctx.fillRect(obj.x, obj.y - 25, textWidth + 10, 25);
                ctx.fillStyle = 'white';
                ctx.fillText(text, obj.x + 5, obj.y - 7);
            });
        }

        // Start loading ML5 when page loads
        window.addEventListener('load', () => {
            debugLog('Page loaded, starting ML5 library loading...');
            loadML5Library();
        });

        // Handle visibility changes to pause/resume detection
        document.addEventListener('visibilitychange', () => {
            if (document.hidden && isDetecting) {
                debugLog('Page hidden, pausing detection');
                clearInterval(detectionInterval);
            } else if (!document.hidden && isDetecting) {
                debugLog('Page visible, resuming detection');
                detectionInterval = setInterval(detectObjects, detectionSpeed);
            }
        });
    </script>

</body>

</html>
