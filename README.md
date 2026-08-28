<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Minecraft Redondo - Ultra Detalhado</title>

  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: #1a1a2e;
      font-family: Arial, sans-serif;
    }

    #info {
      position: fixed;
      top: 12px;
      left: 12px;
      padding: 12px 16px;
      color: white;
      background: rgba(0,0,0,.75);
      border-radius: 10px;
      z-index: 10;
      line-height: 1.6;
      font-size: 14px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.3);
    }
  </style>
</head>

<body>

<div id="info">
  🌍 <b>Planeta Voxel Redondo</b><br>
  🖱️ Clique e arraste para girar<br>
  🔍 Scroll para zoom
</div>

<script type="module">

import * as THREE from "https://cdn.jsdelivr.net/npm/three@0.161/build/three.module.js";
import { OrbitControls } from "https://cdn.jsdelivr.net/npm/three@0.161/examples/jsm/controls/OrbitControls.js";

// CENA E FOG (NÉVOA)
const cena = new THREE.Scene();
cena.background = new THREE.Color(0x87ceeb);

// CÂMERA
const camera = new THREE.PerspectiveCamera(
  55,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
camera.position.set(0, 5, 15);

// RENDERIZADOR
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
document.body.appendChild(renderer.domElement);

// ILUMINAÇÃO
const luzDirecional = new THREE.DirectionalLight(0xffffff, 1.8);
luzDirecional.position.set(12, 24, 12);
luzDirecional.castShadow = true;
luzDirecional.shadow.mapSize.width = 1024;
luzDirecional.shadow.mapSize.height = 1024;
cena.add(luzDirecional);

const luzAmbiente = new THREE.AmbientLight(0xffffff, 0.5);
cena.add(luzAmbiente);

// PLANETA
const planeta = new THREE.Group();
cena.add(planeta);

// CONFIGURAÇÕES DO PLANETA
const tamanhoBloco = 0.25; // Blocos pequenos garantem visual esférico suave
const raioBase = 5.2;

// MATERIAIS DOS BLOCOS (CORES ESTILO MINECRAFT)
const matGrama = new THREE.MeshStandardMaterial({ color: 0x47a036, roughness: 0.8 });
const matTerra = new THREE.MeshStandardMaterial({ color: 0x795548, roughness: 0.9 });
const matAreia = new THREE.MeshStandardMaterial({ color: 0xe0c38c, roughness: 0.9 });
const matAgua  = new THREE.MeshStandardMaterial({ color: 0x2980b9, transparent: true, opacity: 0.8 });

const geoBloco = new THREE.BoxGeometry(tamanhoBloco, tamanhoBloco, tamanhoBloco);

// GERAÇÃO DOS BLOCOS NA SUPERFÍCIE ESFÉRICA
const limite = Math.ceil((raioBase + 1) / tamanhoBloco);

for (let x = -limite; x <= limite; x++) {
  for (let y = -limite; y <= limite; y++) {
    for (let z = -limite; z <= limite; z++) {
      const px = x * tamanhoBloco;
      const py = y * tamanhoBloco;
      const pz = z * tamanhoBloco;

      const dist = Math.sqrt(px * px + py * py + pz * pz);

      // Simula ondulação do terreno (relevo) usando funções trigonométricas
      const variacaoTerreno = Math.sin(px * 1.2) * Math.cos(py * 1.2) * Math.sin(pz * 1.2) * 0.4;
      const raioEfetivo = raioBase + variacaoTerreno;

      // Camada externa (Grama/Areia/Água)
      if (dist <= raioEfetivo && dist >= raioEfetivo - tamanhoBloco * 1.5) {
        let materialUsado = matGrama;

        if (variacaoTerreno < -0.1) {
          materialUsado = matAreia; // Áreas baixas viram praia
        }

        const bloco = new THREE.Mesh(geoBloco, materialUsado);
        bloco.position.set(px, py, pz);
        bloco.castShadow = true;
        bloco.receiveShadow = true;
        planeta.add(bloco);
      }
      // Camada interna (Terra)
      else if (dist < raioEfetivo - tamanhoBloco * 1.5 && dist >= raioBase - 0.8) {
        const blocoTerra = new THREE.Mesh(geoBloco, matTerra);
        blocoTerra.position.set(px, py, pz);
        planeta.add(blocoTerra);
      }
    }
  }
}

// NÚCLEO INTERNO COMPACTO
const geoNucleo = new THREE.SphereGeometry(raioBase - 0.8, 32, 32);
const nucleo = new THREE.Mesh(geoNucleo, matTerra);
planeta.add(nucleo);

// OCEANOS (ESFERA DE ÁGUA LÍQUIDA INTERNA)
const geoAgua = new THREE.SphereGeometry(raioBase - 0.05, 48, 48);
const oceano = new THREE.Mesh(geoAgua, matAgua);
planeta.add(oceano);

// ÁRVORES ESTILO MINECRAFT (ALINHADAS COM A CURVATURA)
function criarArvore(direcao) {
  const dir = direcao.clone().normalize();
  const posBase = dir.clone().multiplyScalar(raioBase + 0.1);

  const arvoreGroup = new THREE.Group();

  // Tronco (Cubo vertical)
  const geoTronco = new THREE.BoxGeometry(0.25, 1.0, 0.25);
  const matTronco = new THREE.MeshStandardMaterial({ color: 0x5d4037 });
  const tronco = new THREE.Mesh(geoTronco, matTronco);
  tronco.position.y = 0.5;
  tronco.castShadow = true;
  arvoreGroup.add(tronco);

  // Folhas (Bloco verde escuro)
  const geoFolhas = new THREE.BoxGeometry(0.8, 0.8, 0.8);
  const matFolhas = new THREE.MeshStandardMaterial({ color: 0x2e7d32 });
  const folhas = new THREE.Mesh(geoFolhas, matFolhas);
  folhas.position.y = 1.1;
  folhas.castShadow = true;
  arvoreGroup.add(folhas);

  // Ajusta rotação para apontar para fora do planeta
  arvoreGroup.position.copy(posBase);
  const eixoCima = new THREE.Vector3(0, 1, 0);
  arvoreGroup.quaternion.setFromUnitVectors(eixoCima, dir);

  planeta.add(arvoreGroup);
}

// DISTRIBUIÇÃO DAS ÁRVORES PELO PLANETA
const posicoesArvores = [
  new THREE.Vector3(0, 1, 0),
  new THREE.Vector3(1, 0.6, 0.4),
  new THREE.Vector3(-0.8, 0.5, -0.6),
  new THREE.Vector3(-0.5, -0.8, 0.4),
  new THREE.Vector3(0.6, -0.7, -0.5),
  new THREE.Vector3(0.3, 0.9, -0.4),
  new THREE.Vector3(-0.9, 0.2, 0.6)
];

posicoesArvores.forEach(p => criarArvore(p));

// CONTROLES DE INTERAÇÃO
const controls = new OrbitControls(camera, renderer.domElement);
controls.enableDamping = true;
controls.dampingFactor = 0.05;
controls.minDistance = 7;
controls.maxDistance = 25;

// ANIMAÇÃO DE ROTAÇÃO
function animar() {
  requestAnimationFrame(animar);
  planeta.rotation.y += 0.0015;
  controls.update();
  renderer.render(cena, camera);
}
animar();

// AJUSTE AUTOMÁTICO DE JANELA
window.addEventListener("resize", () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});

</script>

</body>
</html>