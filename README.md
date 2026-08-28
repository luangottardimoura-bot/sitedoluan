<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Minecraft Redondo 3D</title>

  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: #87ceeb;
      font-family: Arial, sans-serif;
    }

    #info {
      position: fixed;
      top: 12px;
      left: 12px;
      padding: 12px 16px;
      color: white;
      background: rgba(0, 0, 0, 0.7);
      border-radius: 8px;
      z-index: 10;
      font-size: 14px;
      line-height: 1.5;
    }
  </style>

  <!-- Biblioteca Three.js via script direto (evita erros de módulo/CORS no navegador) -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <!-- OrbitControls legado compatível -->
  <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
</head>

<body>

<div id="info">
  🌍 <b>Minecraft Redondo 3D</b><br>
  🖱️ Clique e arraste para girar<br>
  🔍 Scroll para aproximar/afastar
</div>

<script>
  // CENA E FOG
  const cena = new THREE.Scene();
  cena.background = new THREE.Color(0x87ceeb);

  // CÂMERA
  const camera = new THREE.PerspectiveCamera(
    60,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.set(0, 4, 14);

  // RENDERIZADOR
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.shadowMap.enabled = true;
  document.body.appendChild(renderer.domElement);

  // ILUMINAÇÃO
  const luzSol = new THREE.DirectionalLight(0xffffff, 1.5);
  luzSol.position.set(10, 20, 10);
  luzSol.castShadow = true;
  cena.add(luzSol);

  const luzAmbiente = new THREE.AmbientLight(0xffffff, 0.5);
  cena.add(luzAmbiente);

  // PLANETA
  const planeta = new THREE.Group();
  cena.add(planeta);

  const raioPlaneta = 4.5;
  const tamanhoBloco = 0.28;

  // MATERIAIS
  const matGrama = new THREE.MeshStandardMaterial({ color: 0x4caf50, roughness: 0.8 });
  const matTerra = new THREE.MeshStandardMaterial({ color: 0x795548, roughness: 0.9 });
  const matAgua  = new THREE.MeshStandardMaterial({ color: 0x2196f3, transparent: true, opacity: 0.75 });

  const geoBloco = new THREE.BoxGeometry(tamanhoBloco, tamanhoBloco, tamanhoBloco);

  // CRIAR SUPERFÍCIE REDONDA DE BLOCOS
  const pontosCriados = new Set();
  const subdivisoes = 28;

  for (let i = 0; i <= subdivisoes; i++) {
    const lat = (Math.PI * i) / subdivisoes;
    for (let j = 0; j <= subdivisoes * 2; j++) {
      const lon = (2 * Math.PI * j) / (subdivisoes * 2);

      // Converte coordenadas esféricas para grade 3D de blocos
      let x = raioPlaneta * Math.sin(lat) * Math.cos(lon);
      let y = raioPlaneta * Math.cos(lat);
      let z = raioPlaneta * Math.sin(lat) * Math.sin(lon);

      // Ajusta posição para grade discreta (voxel)
      const gx = Math.round(x / tamanhoBloco) * tamanhoBloco;
      const gy = Math.round(y / tamanhoBloco) * tamanhoBloco;
      const gz = Math.round(z / tamanhoBloco) * tamanhoBloco;

      const chave = `${gx.toFixed(2)},${gy.toFixed(2)},${gz.toFixed(2)}`;

      if (!pontosCriados.has(chave)) {
        pontosCriados.add(chave);

        const bloco = new THREE.Mesh(geoBloco, matGrama);
        bloco.position.set(gx, gy, gz);
        bloco.castShadow = true;
        bloco.receiveShadow = true;
        planeta.add(bloco);
      }
    }
  }

  // NÚCLEO INTERNO DE TERRA
  const geoNucleo = new THREE.SphereGeometry(raioPlaneta - 0.4, 32, 32);
  const nucleo = new THREE.Mesh(geoNucleo, matTerra);
  planeta.add(nucleo);

  // OCEANOS NAS PARTES BAIXAS
  const geoAgua = new THREE.SphereGeometry(raioPlaneta - 0.1, 32, 32);
  const oceano = new THREE.Mesh(geoAgua, matAgua);
  planeta.add(oceano);

  // GERADOR DE ÁRVORES PERPENDICULARES
  function criarArvore(direcao) {
    const dir = direcao.clone().normalize();
    const posBase = dir.clone().multiplyScalar(raioPlaneta + 0.1);

    const arvore = new THREE.Group();

    // Tronco
    const troncoGeo = new THREE.BoxGeometry(0.2, 0.9, 0.2);
    const troncoMat = new THREE.MeshStandardMaterial({ color: 0x4e342e });
    const tronco = new THREE.Mesh(troncoGeo, troncoMat);
    tronco.position.y = 0.45;
    tronco.castShadow = true;
    arvore.add(tronco);

    // Folhas
    const folhasGeo = new THREE.BoxGeometry(0.7, 0.7, 0.7);
    const folhasMat = new THREE.MeshStandardMaterial({ color: 0x1b5e20 });
    const folhas = new THREE.Mesh(folhasGeo, folhasMat);
    folhas.position.y = 1.0;
    folhas.castShadow = true;
    arvore.add(folhas);

    // Orientação
    arvore.position.copy(posBase);
    const eixoCima = new THREE.Vector3(0, 1, 0);
    arvore.quaternion.setFromUnitVectors(eixoCima, dir);

    planeta.add(arvore);
  }

  // ÁRVORES ESPALHADAS
  const posicoes = [
    new THREE.Vector3(0, 1, 0),
    new THREE.Vector3(1, 0.5, 0.2),
    new THREE.Vector3(-0.8, 0.6, -0.4),
    new THREE.Vector3(-0.4, -0.9, 0.3),
    new THREE.Vector3(0.7, -0.6, -0.5),
    new THREE.Vector3(-0.9, -0.2, -0.6)
  ];

  posicoes.forEach(pos => criarArvore(pos));

  // CONTROLES DE CÂMERA
  const controls = new THREE.OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.dampingFactor = 0.05;
  controls.minDistance = 6;
  controls.maxDistance = 25;

  // LOOP DE ANIMAÇÃO
  function animar() {
    requestAnimationFrame(animar);
    planeta.rotation.y += 0.002;
    controls.update();
    renderer.render(cena, camera);
  }
  animar();

  // REDIMENSIONAMENTO DE TELA
  window.addEventListener("resize", () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });
</script>

</body>
</html>