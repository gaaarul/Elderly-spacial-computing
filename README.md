<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Aarul Protocol v1</title>
    <script type="module" src="https://ajax.googleapis.com/ajax/libs/model-viewer/3.4.0/model-viewer.min.js"></script>
    
    <style>
        /* 1. RESET & LAYOUT */
        body { margin: 0; overflow: hidden; background: black; font-family: 'Courier New', Courier, monospace; }
        
        /* 2. THE CAMERA FEED (Background) */
        #camera-feed {
            position: absolute;
            top: 0; left: 0;
            width: 100vw; height: 100vh;
            object-fit: cover;
            z-index: 0;
            opacity: 0.8; /* Slight dim to make UI pop */
        }

        /* 3. THE 3D MODEL (Middle Layer) */
        model-viewer {
            position: absolute;
            top: 0; left: 0;
            width: 100vw; height: 100vh;
            z-index: 1;
            --poster-color: transparent;
        }

        /* 4. THE SCI-FI UI (Top Layer) */
        #ui-layer {
            position: absolute;
            top: 0; left: 0;
            width: 100%; height: 100%;
            z-index: 2;
            pointer-events: none; /* Lets you touch the model through the UI */
            box-sizing: border-box;
            padding: 20px;
        }

        /* --- UI ELEMENTS STYLING --- */
        .hud-corner {
            position: absolute;
            width: 40px; height: 40px;
            border: 2px solid #00ffcc; /* Cyan Sci-Fi Color */
        }
        .top-left { top: 20px; left: 20px; border-right: none; border-bottom: none; }
        .top-right { top: 20px; right: 20px; border-left: none; border-bottom: none; }
        .bottom-left { bottom: 20px; left: 20px; border-right: none; border-top: none; }
        .bottom-right { bottom: 20px; right: 20px; border-left: none; border-top: none; }

        .scan-line {
            position: absolute;
            top: 50%; left: 0;
            width: 100%; height: 2px;
            background: rgba(0, 255, 204, 0.5);
            box-shadow: 0 0 10px #00ffcc;
            animation: scan 3s infinite linear;
        }

        .data-block {
            position: absolute;
            top: 100px; right: 20px;
            color: #00ffcc;
            text-align: right;
            font-size: 12px;
            line-height: 1.5;
            text-shadow: 0 0 5px rgba(0, 255, 204, 0.8);
        }

        .reticle {
            position: absolute;
            top: 50%; left: 50%;
            width: 200px; height: 200px;
            transform: translate(-50%, -50%);
            border: 1px dashed rgba(0, 255, 204, 0.3);
            border-radius: 50%;
        }

        /* Animations */
        @keyframes scan {
            0% { top: 0%; opacity: 0; }
            50% { opacity: 1; }
            100% { top: 100%; opacity: 0; }
        }
    </style>
</head>
<body>

    <video id="camera-feed" autoplay playsinline muted></video>

    <model-viewer 
        src="YourLogo.glb" 
        camera-controls 
        auto-rotate 
        shadow-intensity="1"
        disable-zoom
        interaction-prompt="none">
    </model-viewer>

    <div id="ui-layer">
        <div class="hud-corner top-left"></div>
        <div class="hud-corner top-right"></div>
        <div class="hud-corner bottom-left"></div>
        <div class="hud-corner bottom-right"></div>

        <div class="reticle"></div>
        <div class="scan-line"></div>

        <div class="data-block">
            <span>ANALYZING GEOMETRY...</span><br>
            <span>VERTICES: 1,024</span><br>
            <span>SYMMETRY: MATCH</span><br>
            <span>STATUS: <b style="color:white;">LOCKED</b></span>
        </div>
    </div>

    <script>
        async function startCamera() {
            try {
                const stream = await navigator.mediaDevices.getUserMedia({ 
                    video: { facingMode: { exact: "environment" } } // Tries to use Back Camera
                });
                const video = document.getElementById('camera-feed');
                video.srcObject = stream;
            } catch (err) {
                console.error("Camera denied or not found:", err);
                // Fallback if camera fails (e.g. on laptop)
                document.getElementById('camera-feed').style.background = "#333"; 
            }
        }
        startCamera();
    </script>

</body>
</html>
