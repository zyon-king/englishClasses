```
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Página Simples com Player de Áudio</title>
    <style>
:root {
  --primary-dark: #0f0f23;
  --secondary-dark: #1a1a2e;
  --tertiary-dark: #16213e;
  --primary-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  --accent-color: #667eea;
  --accent-hover: #7c8efc;
  --text-primary: #e0e6ed;
  --text-secondary: #c0c4c9;
  --text-muted: #a0a6b0;
  --success-color: #1abc9c;
  --success-dark: #16a085;
  --border-glass: rgba(255, 255, 255, 0.1);
  --bg-glass: rgba(255, 255, 255, 0.05);
  --bg-glass-hover: rgba(255, 255, 255, 0.08);
  --shadow-primary: 0 10px 30px rgba(0, 0, 0, 0.5);
  --shadow-hover: 0 8px 20px rgba(102, 126, 234, 0.5);
  --transition-base: all 0.3s ease;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', 'Cantarell', sans-serif;
  margin: 0;
  padding: 20px;
  background: linear-gradient(135deg, var(--primary-dark) 0%, var(--secondary-dark) 50%, var(--tertiary-dark) 100%);
  color: var(--text-primary);
  min-height: 100vh;
  
  /* BLOQUEIA A SELEÇÃO DE TEXTO */
  -webkit-user-select: none; /* Para navegadores baseados em WebKit (Chrome, Safari, iOS) */
  -moz-user-select: none;    /* Para Firefox */
  -ms-user-select: none;     /* Para Internet Explorer/Edge (legado) */
  user-select: none;         /* Padrão para todos os navegadores modernos */
}

body::before {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to right, #01051a, #24001e);
  pointer-events: none;
  z-index: -1;
}

/* Estilos Específicos do Aplicativo de Aprendizado */

#learning-app {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  background: var(--bg-glass);
  border-radius: 20px;
  box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(15px);
  border: 1px solid var(--border-glass);
}

#level-selection, #chapter-selection, #content-display {
  padding: 20px;
  text-align: center;
}

.hidden {
  display: none !important;
}

h1 {
  margin-bottom: 30px;
  font-size: 2.2rem;
  background: var(--primary-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-weight: 700;
  text-shadow: 0 0 20px rgba(102, 126, 234, 0.3);
}

/* Estilos para Níveis e Capítulos (Botões de Seleção) */

.level-list, .chapter-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-top: 20px;
  min-height: 50vh;
}

.level-button, .chapter-button {
  padding: 15px 25px;
  font-size: 1.2rem;
  font-weight: 600;
  border: 2px solid var(--accent-color);
  border-radius: 12px;
  cursor: pointer;
  transition: var(--transition-base);
  background: rgba(102, 126, 234, 0.1);
  color: var(--text-primary);
  text-align: left;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  position: relative;
  overflow: hidden;
}

.level-button::before, .chapter-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s;
}

.level-button:hover::before, .chapter-button:hover::before {
  left: 100%;
}

.level-button:hover, .chapter-button:hover {
  background: linear-gradient(90deg, var(--accent-color) 0%, #764ba2 100%);
  color: white;
  transform: translateY(-2px);
  box-shadow: var(--shadow-hover);
  border-color: transparent;
}

/* Estilos para o Botão Voltar */
.back-button {
  padding: 10px 20px;
  font-size: 1rem;
  font-weight: 500;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  background: linear-gradient(135deg, #6c757d 0%, #5a6268 100%);
  color: white;
  transition: var(--transition-base);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.back-button:hover {
  background: linear-gradient(135deg, #7a8085 0%, #65696e 100%);
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(108, 117, 125, 0.4);
}

/* Estilos para Áudio e Texto (content-display) */

.content-box {
  text-align: left;
  padding: 0;
}

.content-box h3, .footer-player h3 {
  background: linear-gradient(45deg, var(--success-color), var(--success-dark));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-top: 20px;
  margin-bottom: 10px;
  font-size: 1.3rem;
  font-weight: 700;
}



#lesson-text {
    display: block; 
    line-height: 1.8;
    font-size: 1.1rem;
    color: var(--text-secondary);
    padding: 15px 20px;
    background: rgba(0, 0, 0, 0.2);
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    margin-left: -20px;
    margin-right: -20px;

    /* 2. Aumenta a largura para compensar o espaço ganho com as margens */
    width: calc(100% + 0);

    /* 3. Remove o arredondamento para que os cantos não fiquem estranhos */
    border-radius: 0;
  	border-left: 5px solid var(--success-color);
}

.header-title {
  position: fixed;
  top: 0;
  display: flex;
  justify-content: space-between;
  left: 0;
  width: 100%;
  background:linear-gradient(to right, #01051a, #24001e);
  gap: 10px;
  padding: 5px 10px;
  font-size: 1.5rem;
  box-sizing: border-box; /* Garante que o padding não aumente a largura total */
  box-shadow: 0 -2px 5px rgba(0,0,0,0.1); /* Sombra para dar um efeito de elevação */
}

#content-title {
  background: var(--primary-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-weight: 700;
  text-shadow: 0 0 20px rgba(102, 126, 234, 0.3);
}

/* Estilo para o player fixo no rodapé */
.footer-player {
  position: fixed;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  left: 0;
  width: 100%;
  background:linear-gradient(to right, #01051a, #24001e);
  gap: 10px;
  padding: 3px 8px; /* Reduzido de 5px para 3px */
  box-sizing: border-box;
  box-shadow: 0 -2px 5px rgba(0,0,0,0.1);
}

#audio-player {
  width: 75%;
}


/* CSS para os emojis */
.emoji {
  font-family: 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji', sans-serif;
  background: none; /* Remove qualquer gradiente ou cor de fundo */
  -webkit-background-clip: initial; /* Reseta o recorte do fundo */
  background-clip: initial;
  -webkit-text-fill-color: initial; /* Reseta a cor de preenchimento do texto */
  text-shadow: none; /* Remove qualquer sombra de texto */
  display: inline-block; /* Ajuda a evitar problemas de alinhamento vertical */
}

/* Realce para a Linha de Legenda Ativa */
.subtitle-block {
    display: block; /* Mudado de inline-block para block */
    margin: 5px 0;
    padding: 5px 10px;
    cursor: pointer;
    transition: all 0.3s ease;
    border-left: 4px solid transparent;
    border-radius: 4px;
}

.subtitle-block.active-line {
    background-color: rgba(102, 126, 234, 0.25);
    border-left: 4px solid var(--accent-color);
    border-radius: 4px;
    padding: 8px 12px;
    font-weight: 600;
    color: var(--accent-hover);
    transform: translateX(5px);
    box-shadow: 0 0 15px rgba(102, 126, 234, 0.3);
}

/* Garante que o texto clicável tenha um estilo diferente de seleção */
.subtitle-block:hover {
    color: var(--text-primary);
    background-color: rgba(255, 255, 255, 0.08); 
    border-left-color: var(--text-muted);
    transform: translateX(3px);
}

/* Media Queries para Responsividade */
@media only screen and (max-width: 600px) {
  body {
    padding: 10px;
  }

  #learning-app {
    padding: 0px;
    margin: 10px;
  }

  h1 {
    font-size: 1.8rem;
  }

  .level-button, .chapter-button {
    font-size: 1rem;
    padding: 12px 20px;
  }
  #content-display h3 {
    margin-top: 65px;
  }
  .content-box {
    padding: 0;
  }

  .content-box h2 {
    font-size: 1.5rem;
  }

  #lesson-text {
    font-size: 1.2rem;
  }
  .back-button {
    padding: 5px 10px;
    font-size: 0.8rem;
    height: 30%;
  }
}
    </style>
</head>
<body>
<div id="learning-app">
    <div id="level-selection">
    	<h1>Plataforma de Aprendizado de Idiomas</h1>
        <p>_____________________</p>
        <h2>Escolha um Nível <span class="emoji">🚀</span></h2>
        <div class="level-list">
            <button class="level-button" onclick="showChapters('Stage 1 - Freshman 👶')">Stage 1 - Freshman 👶</button>
            <button class="level-button" onclick="showChapters('Stage 2 - Sophomore 🎓')">Stage 2 - Sophomore 🎓</button>
            <button class="level-button" onclick="showChapters('Stage 3 - Junior 🧠')">Stage 3 - Junior 🧠</button>
        </div>
    </div>
    
    <div id="chapter-selection" class="hidden">
        <h1 id="chapter-title">[Nome do Nível]</h1>
        <button class="back-button" onclick="showLevels()">Voltar aos Níveis</button>
        <div class="chapter-list"></div>
    </div>

    <div id="content-display" class="hidden">        
        <div class="content-box">
            <h3><span class="emoji">📝</span> Texto</h3>
            <p id="lesson-text"></p>
        </div>
    </div>
</div>

    <div class="header-title" id="header-title-container">
    	<span id="content-title">[Nome do Capítulo]</span>
        <button class="back-button" onclick="showChapters()">Voltar aos Capítulos</button>
    </div>

    <!-- Player de áudio fixo no rodapé -->
    <div class="footer-player" id="audio-player-container">
        <h3><span class="emoji">🎧</span> Áudio</h3>
        <audio id="audio-player" controls>
            <source src="" type="audio/mpeg">
            Seu navegador não suporta o elemento de áudio.
        </audio>
    </div>
<script src="https://cdn.jsdelivr.net/npm/appwrite@21.0.0"></script>
<script>
// 𒅂𒅂𒅂𒅂𒅂𒅂𒅂𒅂𒅂
// Bloco | APPWRITE
// 𒅂𒅂𒅂𒅂𒅂𒅂𒅂𒅂𒅂

// Variáveis de Configuração do Appwrite
const ENDPOINT = 'https://sfo.cloud.appwrite.io/v1';
const PROJECT_ID = '68dfeed70038e6089b24';
const DATABASE_ID = '68e02075001314d07133';
const COLLECTION_ID = '68e0209300218224e360'; 

// Acesso às classes do Appwrite através da variável global Appwrite
const Client = Appwrite.Client;
const Databases = Appwrite.Databases;
const Query = Appwrite.Query;

const client = new Client();
client
    .setEndpoint(ENDPOINT) 
    .setProject(PROJECT_ID); 

const databases = new Databases(client);

// A variável global que será preenchida pelo Appwrite
let courseData = {}; 

// --- FUNÇÃO DE CARREGAMENTO DE DADOS (USANDO CLASSES GLOBAIS) ---
async function loadCourseData() {
    try {
        // CORREÇÃO: Usar 'levelTitle' no Query.orderAsc para ordenar pela coluna correta
        const queries = [
            Appwrite.Query.orderAsc('levelTitle'), 
            Appwrite.Query.orderAsc('orderIndex'), 
        ];
        
        // Usa as constantes de ID definidas em outra parte do seu script
        const response = await databases.listDocuments(DATABASE_ID, COLLECTION_ID, queries);
        
        response.documents.forEach(doc => {
            // CORREÇÃO: Usar doc.levelTitle (nome da coluna)
            const level = doc.levelTitle; 

            if (!courseData[level]) {
                courseData[level] = [];
            }
            
            // Mapeamento: As chaves do Appwrite (doc.xxx) já estão consistentes
            // com as chaves que você forneceu, exceto pela variável 'level'.
            courseData[level].push({
                title: doc.chapterTitle, 
                audio: doc.audioUrl,     
                subtitle: doc.subtitleUrl 
            });
        });

        // Após carregar os dados, exibe os níveis
        const levelListDiv = document.querySelector('#level-selection .level-list');
        levelListDiv.innerHTML = ''; // Limpa a lista
        
        // Cria um botão para cada nível
        Object.keys(courseData).forEach(levelName => {
            const levelButton = document.createElement('button');
            levelButton.className = 'level-button';
            levelButton.textContent = levelName;
            levelButton.onclick = () => showChapters(levelName);
            levelListDiv.appendChild(levelButton);
        });
        
    } catch (error) {
        console.error("Erro ao carregar dados do Appwrite:", error);
        document.getElementById('level-selection').innerHTML = '<h1><span class="emoji">❌</span> Erro ao carregar o curso. Verifique o console e as permissões de leitura do Appwrite.</h1>';
        Appwrite.showLevels(); 
    }
}

let currentLevel = ''; // Variável para rastrear o nível atualmente selecionado

/**
 * Exibe a tela de seleção de Níveis e oculta as outras.
 */
function showLevels() {
    document.getElementById('level-selection').classList.remove('hidden');
    document.getElementById('chapter-selection').classList.add('hidden');
    document.getElementById('content-display').classList.add('hidden');
    document.getElementById('audio-player-container').classList.add('hidden');
    document.getElementById('header-title-container').classList.add('hidden');
}

/**
 * Exibe a tela de seleção de Capítulos para um Nível específico.
 * @param {string} level O nome do Nível clicado ('Iniciante', 'Intermediário', etc.).
 */
function showChapters(level) {
    currentLevel = level;
    document.getElementById('level-selection').classList.add('hidden');
    document.getElementById('chapter-selection').classList.remove('hidden');
    document.getElementById('content-display').classList.add('hidden');
    document.getElementById('audio-player-container').classList.add('hidden');
    document.getElementById('header-title-container').classList.add('hidden');
    
    const emojiRegex = /(\p{Emoji_Presentation}|\p{Extended_Pictographic})/gu;
    
    // Usa a expressão para encontrar qualquer emoji na string 'level' e envolvê-lo
    // com a span que remove a formatação.
    const formattedLevel = level.replace(emojiRegex, '<span class="emoji">$&</span>');

    // Atualiza o título
    document.getElementById('chapter-title').innerHTML = `${formattedLevel} <span class="emoji">📚</span>`;

    // Preenche a lista de capítulos
    const chapterListDiv = document.querySelector('#chapter-selection .chapter-list');
    chapterListDiv.innerHTML = ''; // Limpa os capítulos anteriores

    courseData[level].forEach((chapter, index) => {
        const chapterButton = document.createElement('button');
        chapterButton.className = 'chapter-button';
        chapterButton.textContent = chapter.title;
        chapterButton.onclick = () => showContent(level, index); // Passa o nível e o índice
        chapterListDiv.appendChild(chapterButton);
    });

    // Atualiza o botão de voltar para garantir que volte para a tela de Níveis
    document.querySelector('#chapter-selection .back-button').onclick = showLevels;
}

// 冨冨冨冨冨冨冨冨冨
// Bloco Fx | Link Srt
// 冨冨冨冨冨冨冨冨冨

/**
 * Converte o formato de tempo do SRT (HH:MM:SS,ms) para segundos.
 * @param {string} timeString O tempo no formato SRT.
 * @returns {number} O tempo total em segundos.
 */
function srtTimeToSeconds(timeString) {
    const parts = timeString.split(/[:,]/);
    const hours = parseInt(parts[0], 10) || 0;
    const minutes = parseInt(parts[1], 10) || 0;
    const seconds = parseInt(parts[2], 10) || 0;
    const milliseconds = parseInt(parts[3], 10) || 0;
    return hours * 3600 + minutes * 60 + seconds + milliseconds / 1000;
}

/**
 * Analisa o conteúdo de um arquivo SRT e o transforma em um array de objetos.
 * @param {string} srtContent O conteúdo completo do arquivo .srt.
 * @returns {Array<{startTime: number, endTime: number, text: Array<string>}>}
 */
function parseSrt(srtContent) {
    const subtitles = [];
    const blocks = srtContent.trim().split(/\n\s*\n/); // Divide em blocos de legenda

    for (const block of blocks) {
        const lines = block.split('\n');
        // Garante que o bloco tenha pelo menos a linha do tempo e uma linha de texto
        if (lines.length >= 2) { 
            const timeLine = lines[1];
            const timeMatch = timeLine.match(/(\d{2}:\d{2}:\d{2},\d{3})\s*-->\s*(\d{2}:\d{2}:\d{2},\d{3})/);
            
            if (timeMatch) {
                const startTime = srtTimeToSeconds(timeMatch[1]);
                const endTime = srtTimeToSeconds(timeMatch[2]);
                
                // CORRIGIDO: Mantém as linhas de texto como um array, em vez de juntá-las.
                const textLines = lines.slice(2);
                
                subtitles.push({ startTime, endTime, text: textLines });
            }
        }
    }
    return subtitles;
}

/**
 * Exibe a tela de Áudio e Texto (Conteúdo) para um Capítulo específico.
 * Cria legendas clicáveis, toca apenas o trecho selecionado e o destaca.
 * @param {string} level O Nível do capítulo.
 * @param {number} chapterIndex O índice do capítulo dentro do array do Nível.
 */
async function showContent(level, chapterIndex) {
    const chapter = courseData[level][chapterIndex];

    document.getElementById('level-selection').classList.add('hidden');
    document.getElementById('chapter-selection').classList.add('hidden');
    document.getElementById('content-display').classList.remove('hidden');
    document.getElementById('audio-player-container').classList.remove('hidden');
    document.getElementById('header-title-container').classList.remove('hidden');

    const emojiRegex = /(\p{Emoji_Presentation}|\p{Extended_Pictographic})/gu;
	const formatedChapter = chapter.title.replace(emojiRegex, '<span class="emoji">$&</span>');

    // Exibe o título do capítulo
document.getElementById('content-title').innerHTML = `${formatedChapter} <span class="emoji">🗣️</span>`;

    const audioPlayer = document.getElementById('audio-player');
    const lessonTextElement = document.getElementById('lesson-text');
    
    // Reset
    audioPlayer.src = chapter.audio;
    audioPlayer.ontimeupdate = null;
    lessonTextElement.innerHTML = 'Carregando texto...';

    // NOVO: Variável para controlar quando o áudio deve parar.
    // Inicia como null, indicando que não há parada programada.
    let stopAtTime = null;

    if (chapter.subtitle) {
        try {
            const response = await fetch(chapter.subtitle);
            if (!response.ok) throw new Error('Falha ao carregar o arquivo de legenda.');
            
            const srtContent = await response.text();
            const subtitles = parseSrt(srtContent);
            
            lessonTextElement.innerHTML = '';

            subtitles.forEach((sub, index) => {
                const blockContainer = document.createElement('div');
                blockContainer.className = 'subtitle-block';
                blockContainer.id = `sub-block-${index}`;
                blockContainer.dataset.startTime = sub.startTime;

                // ALTERADO: O evento de clique agora também define o 'stopAtTime'.
                blockContainer.onclick = () => {
                    audioPlayer.currentTime = sub.startTime;
                    stopAtTime = sub.endTime; // Define o tempo exato para parar
                    audioPlayer.play();
                };

                sub.text.forEach(lineText => {
                    const lineElement = document.createElement('span');
                    lineElement.textContent = lineText;
                    lineElement.className = 'subtitle-line'; 
                    blockContainer.appendChild(lineElement); 
                });
                
                lessonTextElement.appendChild(blockContainer);
            });

            // ALTERADO: 'ontimeupdate' agora tem duas responsabilidades:
            // 1. Parar o áudio no tempo certo.
            // 2. Destacar a linha ativa.
            audioPlayer.ontimeupdate = () => {
                const currentTime = audioPlayer.currentTime;

                // 1. Lógica para PARAR o áudio
                if (stopAtTime && currentTime >= stopAtTime) {
                    audioPlayer.pause();
                    stopAtTime = null;
                }

                // 2. Lógica para DESTACAR a linha
                let activeBlockIndex = -1;
                for (let i = 0; i < subtitles.length; i++) {
                    if (currentTime >= subtitles[i].startTime && currentTime <= subtitles[i].endTime) {
                        activeBlockIndex = i;
                        break;
                    }
                }

                const allBlocks = lessonTextElement.querySelectorAll('.subtitle-block');
                allBlocks.forEach((block, index) => {
                    if (index === activeBlockIndex) {
                        block.classList.add('active-line');

                        // 3. SCROLL AUTOMÁTICO - Rola a linha ativa para o topo da tela
                        block.scrollIntoView({
                            behavior: 'auto',  // Animação suave
                            block: 'center'      // Centraliza na tela (opções: 'start', 'center', 'end', 'nearest')
                        });
                    } else {
                        block.classList.remove('active-line');
                    }
                });
            };
            
        } catch (error) {
            console.error('Erro ao processar o arquivo SRT:', error);
            lessonTextElement.textContent = 'Não foi possível carregar o texto da lição. Tente novamente.';
			showChapters();
        }
    } else {
        lessonTextElement.textContent = chapter.text || 'Nenhum texto disponível.';
    }

    document.querySelector('#content-display .back-button').onclick = () => showChapters(currentLevel);
}

// --- Inicialização ---

// Garante que a tela inicial seja a de Níveis ao carregar a página
document.addEventListener('DOMContentLoaded', () => {
    // Inicializa o player de áudio com um placeholder para evitar erro de src vazio
    const audioPlayer = document.getElementById('audio-player');
    if (audioPlayer) {
        audioPlayer.src = ''; 
    }
    
    showLevels();
    loadCourseData();
});

    </script>
    </script>

</body>
</html>
```
