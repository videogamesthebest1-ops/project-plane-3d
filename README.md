<!DOCTYPE html>
<html lang="ro">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Boeing 787 Dreamliner 3D Complet</title>
    <style>
        body { margin: 0; overflow: hidden; background-color: #0b1d28; }
        #info { position: absolute; top: 15px; width: 100%; text-align: center; color: #ffffff; font-family: -apple-system, sans-serif; font-size: 14px; z-index: 10; font-weight: bold; text-shadow: 1px 1px 3px black; }
    </style>
</head>
<body>
    <div id="info">Boeing 787 Dreamliner - Model Complet Detaliat (Rotește cu degetul)</div>
    
    <script src="https://unpkg.com"></script>
    <script src="https://unpkg.com"></script>
    
    <script>
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x0f1b29);

        const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        document.body.appendChild(renderer.domElement);

        const controls = new THREE.OrbitControls(camera, renderer.domElement);
        controls.enableDamping = true;
        camera.position.set(0, 5, 14);
        controls.update();

        const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
        scene.add(ambientLight);
        
        const sunLight = new THREE.DirectionalLight(0xffffff, 1.0);
        sunLight.position.set(20, 30, 10);
        scene.add(sunLight);

        const blueSkyLight = new THREE.DirectionalLight(0x4a90e2, 0.4);
        blueSkyLight.position.set(-20, -10, -10);
        scene.add(blueSkyLight);

        const airplane = new THREE.Group();

        const whiteGloss = new THREE.MeshStandardMaterial({ color: 0xffffff, roughness: 0.15, metalness: 0.1 });
        const boeingBlue = new THREE.MeshStandardMaterial({ color: 0x0033aa, roughness: 0.2 });
        const aluminum = new THREE.MeshStandardMaterial({ color: 0xaaaaaa, roughness: 0.3, metalness: 0.6 });
        const darkGlass = new THREE.MeshStandardMaterial({ color: 0x111111, roughness: 0.05 });

        const bodyGeo = new THREE.CylinderGeometry(1, 0.8, 12, 32);
        bodyGeo.rotateX(Math.PI / 2);
        const fuselage = new THREE.Mesh(bodyGeo, whiteGloss);
        airplane.add(fuselage);

        const noseGeo = new THREE.ConeGeometry(1, 2.5, 32);
        noseGeo.rotateX(Math.PI / 2);
        const nose = new THREE.Mesh(noseGeo, whiteGloss);
        nose.position.z = -7.25;
        airplane.add(nose);

        const cockpitWindowGeo = new THREE.CylinderGeometry(1.02, 1.02, 0.5, 32, 1, false, 0, Math.PI);
        cockpitWindowGeo.rotateX(Math.PI / 2);
        const cockpitWindow = new THREE.Mesh(cockpitWindowGeo, darkGlass);
        cockpitWindow.position.set(0, 0.3, -6.5);
        airplane.add(cockpitWindow);

        const windowsLeftGeo = new THREE.BoxGeometry(0.02, 0.15, 7);
        const windowsLeft = new THREE.Mesh(windowsLeftGeo, darkGlass);
        windowsLeft.position.set(-1.01, 0.1, -1);
        airplane.add(windowsLeft);

        const windowsRight = windowsLeft.clone();
        windowsRight.position.x = 1.01;
        airplane.add(windowsRight);

        const wingGroupLeft = new THREE.Group();
        const innerWingGeo = new THREE.BoxGeometry(4, 0.15, 2);
        const innerWing = new THREE.Mesh(innerWingGeo, whiteGloss);
        innerWing.position.x = -2;
        wingGroupLeft.add(innerWing);

        const outerWingGeo = new THREE.BoxGeometry(4, 0.1, 1.2);
        const outerWing = new THREE.Mesh(outerWingGeo, whiteGloss);
        outerWing.position.set(-5.8, 0.2, -0.2);
        outerWing.rotation.z = 0.15;
        wingGroupLeft.add(outerWing);
        
        wingGroupLeft.position.set(0, -0.1, -0.5);
        wingGroupLeft.rotation.y = 0.22;
        airplane.add(wingGroupLeft);

        const wingGroupRight = wingGroupLeft.clone();
        wingGroupRight.scale.x = -1;
        wingGroupRight.rotation.y = -0.22;
        airplane.add(wingGroupRight);

        const engineGeo = new THREE.CylinderGeometry(0.55, 0.5, 2, 24);
        engineGeo.rotateX(Math.PI / 2);
        const engineCoreGeo = new THREE.CylinderGeometry(0.4, 0.4, 2.1, 16);
        engineCoreGeo.rotateX(Math.PI / 2);

        const leftEngine = new THREE.Mesh(engineGeo, whiteGloss);
        const leftEngineCore = new THREE.Mesh(engineCoreGeo, aluminum);
        leftEngine.add(leftEngineCore);
        leftEngine.position.set(-2.8, -0.5, -0.8);
        airplane.add(leftEngine);

        const rightEngine = leftEngine.clone();
        rightEngine.position.x = 2.8;
        airplane.add(rightEngine);

        const tailVertGeo = new THREE.BoxGeometry(0.15, 3.2, 2);
        const tailVert = new THREE.Mesh(tailVertGeo, boeingBlue);
        tailVert.position.set(0, 1.9, 5);
        tailVert.rotation.x = 0.35;
        airplane.add(tailVert);

        const tailHorGeo = new THREE.BoxGeometry(4.5, 0.08, 1.2);
        const tailHor = new THREE.Mesh(tailHorGeo, whiteGloss);
        tailHor.position.set(0, 0, 5.2);
        tailHor.rotation.y = 0.1;
        airplane.add(tailHor);

        scene.add(airplane);
        airplane.position.set(0, 0, 0);

        function animate() {
            requestAnimationFrame(animate);
            airplane.rotation.y += 0.002;
            controls.update();
            renderer.render(scene, camera);
        }

        window.addEventListener('resize', () => {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        });

        animate();
    </script>
</body>
</html>