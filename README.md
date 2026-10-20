#Reconhecimento facial
Funcionalidades
Detecção de até 3 rostos ao mesmo tempo
A máscara acompanha o tamanho, a posição e a inclinação da cabeça
Os olhos da pessoa aparecem pelos buracos da máscara
Vídeo espelhado (efeito selfie)
Controle deslizante para ajustar o tamanho da máscara
Tecnologias
HTML, CSS e JavaScript (ES Modules)
MediaPipe Tasks Vision (Face Landmarker)
Canvas 2D para desenhar o vídeo e a máscara
getUserMedia para acessar a câmera
Estrutura
mascara-site/
├── index.html   # página, estilos e lógica
├── mask.png     # imagem da máscara (PNG com transparência)
└── README.md
Como executar

A câmera só funciona em https ou localhost, então o arquivo não pode ser aberto direto (file://).

bash
cd mascara-site
python3 -m http.server 8080

Abra http://localhost:8080, clique em Ligar câmera e permita o acesso. Também funciona com a extensão Live Server do VS Code.

É necessária conexão com a internet: o modelo e os arquivos WASM do MediaPipe são carregados de CDN (jsDelivr e Google Storage).

Como funciona
O Face Landmarker analisa cada frame do vídeo e retorna 478 pontos do rosto.
O código usa apenas o centro das duas íris (landmarks 468 e 473).
A distância entre elas define a escala da máscara, e o ângulo entre elas define a rotação.
O ponto médio entre os olhos é alinhado com o ponto médio dos buracos dos olhos na imagem (mask.png).
A máscara é desenhada no canvas por cima do vídeo a cada frame, usando requestAnimationFrame.
Personalização
O que mudar	Onde
Tamanho padrão da máscara	atributo value do <input id="size">
Número máximo de rostos	numFaces em FaceLandmarker.createFromOptions
Outra máscara	trocar mask.png e atualizar MASK_EYE_L e MASK_EYE_R com a posição dos olhos na nova imagem
Limitações
A rotação é apenas no plano da imagem (inclinar a cabeça para o lado). Virar o rosto para os lados não gira a máscara em 3D.
Para rotação 3D seria necessário usar a matriz de transformação facial do MediaPipe com WebGL ou Three.js.
