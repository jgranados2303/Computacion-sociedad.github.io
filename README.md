<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Clasificador interactivo sobre la influencia de la tecnología en la sociedad. Recurso educativo para ingeniería universitaria.">
    <title>Clasificador Interactivo: Unidad 2 (Influencia de la Tecnología en la Sociedad)</title>
    <style>
        :root {
            --color-primary: #2563eb;
            --color-secondary: #1d4ed8;
            --color-accent: #3b82f6;
            --color-bg: #f8fafc;
            --color-text: #1e293b;
            --color-success: #10b981;
            --color-error: #ef4444;
            --space-xs: 4px;
            --space-sm: 8px;
            --space-md: 16px;
            --space-lg: 24px;
            --space-xl: 32px;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: system-ui, -apple-system, 'Segoe UI', sans-serif;
            background-color: var(--color-bg);
            color: var(--color-text);
            line-height: 1.6;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: var(--space-md);
            flex: 1;
        }

        header {
            background-color: white;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            padding: var(--space-md);
            display: flex;
            align-items: center;
            gap: var(--space-sm);
        }

        .logo {
            display: inline-flex;
            align-items: center;
            gap: var(--space-xs);
        }

        .header-content {
            flex: 1;
        }

        h1 {
            font-size: 1.25rem;
            font-weight: 600;
            color: var(--color-primary);
        }

        .resource-type {
            font-size: 0.875rem;
            color: var(--color-secondary);
            font-weight: 500;
        }

        main {
            padding: var(--space-lg) 0;
        }

        .learning-objective {
            background-color: white;
            border-radius: 8px;
            padding: var(--space-md);
            margin-bottom: var(--space-lg);
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .instructions {
            background-color: #fef3c7;
            border-left: 4px solid #f59e0b;
            padding: var(--space-md);
            margin-bottom: var(--space-lg);
            border-radius: 0 8px 8px 0;
        }

        .drag-container {
            display: grid;
            grid-template-columns: 1fr;
            gap: var(--space-lg);
            margin-bottom: var(--space-lg);
        }

        @media (min-width: 768px) {
            .drag-container {
                grid-template-columns: 1fr 1fr;
            }
        }

        @media (min-width: 1024px) {
            .drag-container {
                grid-template-columns: 1fr 2fr;
            }
        }

        .source-container {
            background-color: white;
            border-radius: 8px;
            padding: var(--space-md);
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .source-title {
            font-size: 1.125rem;
            font-weight: 600;
            margin-bottom: var(--space-md);
            color: var(--color-secondary);
        }

        .items-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
            gap: var(--space-sm);
        }

        .item-card {
            background-color: #f1f5f9;
            border: 2px dashed #cbd5e1;
            border-radius: 6px;
            padding: var(--space-sm);
            text-align: center;
            cursor: grab;
            transition: all 0.2s ease;
            user-select: none;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.875rem;
        }

        .item-card:hover {
            background-color: #e2e8f0;
            transform: translateY(-2px);
        }

        .item-card.dragging {
            opacity: 0.7;
            transform: rotate(5deg);
        }

        .target-containers {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: var(--space-md);
        }

        .category-container {
            background-color: white;
            border: 2px solid transparent;
            border-radius: 8px;
            padding: var(--space-md);
            min-height: 300px;
            transition: all 0.3s ease;
        }

        .category-header {
            font-weight: 600;
            margin-bottom: var(--space-md);
            text-align: center;
            padding-bottom: var(--space-sm);
            border-bottom: 2px solid currentColor;
        }

        .category-academico .category-header { color: var(--color-primary); }
        .category-familiar .category-header { color: var(--color-success); }
        .category-laboral .category-header { color: var(--color-accent); }

        .category-container.highlight {
            border-color: var(--color-primary);
            background-color: #eff6ff;
        }

        .category-container.correct {
            border-color: var(--color-success);
            background-color: #ecfdf5;
        }

        .category-container.incorrect {
            border-color: var(--color-error);
            background-color: #fef2f2;
        }

        .category-container.shake {
            animation: shake 0.5s ease-in-out;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-5px); }
            40%, 80% { transform: translateX(5px); }
        }

        .dropped-items {
            display: grid;
            gap: var(--space-sm);
            min-height: 200px;
        }

        .dropped-item {
            background-color: #f8fafc;
            border: 1px solid #e2e8f0;
            border-radius: 4px;
            padding: var(--space-sm);
            text-align: center;
            font-size: 0.875rem;
            position: relative;
        }

        .dropped-item.correct::after {
            content: "✓";
            position: absolute;
            top: -8px;
            right: -8px;
            background-color: var(--color-success);
            color: white;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            font-size: 0.75rem;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .controls {
            display: flex;
            gap: var(--space-md);
            margin-top: var(--space-lg);
        }

        button {
            background-color: var(--color-primary);
            color: white;
            border: none;
            padding: var(--space-sm) var(--space-md);
            border-radius: 6px;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.2s ease;
        }

        button:hover {
            background-color: var(--color-secondary);
            transform: translateY(-1px);
        }

        button:active {
            transform: translateY(0);
        }

        .score-display {
            background-color: white;
            border-radius: 8px;
            padding: var(--space-md);
            margin: var(--space-lg) 0;
            text-align: center;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        .results-screen {
            background-color: white;
            border-radius: 8px;
            padding: var(--space-xl);
            margin: var(--space-lg) 0;
            text-align: center;
            box-shadow: 0 4px 16px rgba(0,0,0,0.1);
            display: none;
        }

        .results-screen.active {
            display: block;
        }

        .final-score {
            font-size: 2rem;
            font-weight: 700;
            color: var(--color-primary);
            margin: var(--space-md) 0;
        }

        .explanation {
            text-align: left;
            margin-top: var(--space-lg);
        }

        .explanation h3 {
            color: var(--color-secondary);
            margin-bottom: var(--space-md);
        }

        .explanation ul {
            list-style-position: inside;
            padding-left: var(--space-md);
        }

        footer {
            background-color: white;
            padding: var(--space-md);
            text-align: center;
            border-top: 1px solid #e2e8f0;
            margin-top: auto;
        }

        .footer-content {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: var(--space-xs);
            font-size: 0.875rem;
            color: #64748b;
        }
    </style>
</head>
<body>
    <header>
        <div class="logo">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="none" style="width:28px;height:28px;vertical-align:middle">
                <rect width="32" height="32" rx="8" fill="#2563eb"/>
                <text x="16" y="22" text-anchor="middle" font-family="system-ui,sans-serif" font-weight="700" font-size="16" fill="white">E</text>
            </svg>
        </div>
        <div class="header-content">
            <h1>Clasificador Interactivo: Unidad 2 (Influencia de la Tecnología en la Sociedad)</h1>
            <div class="resource-type">Área: Ingeniería | Nivel: Universitario</div>
        </div>
    </header>

    <div class="container">
        <main>
            <div class="learning-objective">
                <h2>Objetivo de Aprendizaje</h2>
                <p>Analizar el impacto y la influencia de las herramientas tecnológicas en los diversos ámbitos de la vida cotidiana (académico, familiar y laboral), evaluando críticamente los aportes significativos que la tecnología ha brindado al desarrollo de la humanidad.</p>
            </div>

            <div class="instructions">
                <h3>Instrucciones:</h3>
                <p>Arrastra cada tecnología hacia la categoría que mejor represente su impacto principal: Académico, Familiar o Laboral. Cada tecnología puede pertenecer a una sola categoría.</p>
            </div>

            <div class="score-display">
                <h3>Puntuación: <span id="score">0</span>/<span id="total">0</span></h3>
                <p>Elementos clasificados correctamente</p>
            </div>

            <div class="drag-container">
                <div class="source-container">
                    <h3 class="source-title">Tecnologías para Clasificar</h3>
                    <div class="items-grid" id="items-source">
                        <!-- Items will be populated by JavaScript -->
                    </div>
                </div>

                <div class="target-containers">
                    <div class="category-container category-academico" data-category="academico">
                        <h3 class="category-header">Impacto Académico</h3>
                        <div class="dropped-items"></div>
                    </div>
                    <div class="category-container category-familiar" data-category="familiar">
                        <h3 class="category-header">Impacto Familiar</h3>
                        <div class="dropped-items"></div>
                    </div>
                    <div class="category-container category-laboral" data-category="laboral">
                        <h3 class="category-header">Impacto Laboral</h3>
                        <div class="dropped-items"></div>
                    </div>
                </div>
            </div>

            <div class="controls">
                <button id="reset-btn">Reiniciar Clasificación</button>
                <button id="check-btn">Verificar Resultados</button>
            </div>

            <div class="results-screen" id="results-screen">
                <h2>¡Actividad Completada!</h2>
                <div class="final-score" id="final-score">0%</div>
                <p id="performance-text">Excelente trabajo</p>
                <div class="explanation">
                    <h3>Explicación de la Clasificación:</h3>
                    <ul id="explanation-list">
                        <!-- Explanations will be populated by JavaScript -->
                    </ul>
                </div>
            </div>
        </main>
    </div>

    <footer>
        <div class="footer-content">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="none" style="width:24px;height:24px;vertical-align:middle">
                <rect width="32" height="32" rx="8" fill="#2563eb"/>
                <text x="16" y="22" text-anchor="middle" font-family="system-ui,sans-serif" font-weight="700" font-size="14" fill="white">E</text>
            </svg>
            <span>Creado con John Fernando Granados Romero</span>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Data for the classification activity
            const items = [
                { id: 1, text: "Sistemas de Gestión Académica", category: "academico" },
                { id: 2, text: "Videoconferencias", category: "familiar" },
                { id: 3, text: "Plataformas de E-learning", category: "academico" },
                { id: 4, text: "Redes Sociales", category: "familiar" },
                { id: 5, text: "Software de Contabilidad", category: "laboral" },
                { id: 6, text: "Smart TVs", category: "familiar" },
                { id: 7, text: "ERP (Planificación de Recursos)", category: "laboral" },
                { id: 8, text: "Aplicaciones de Mensajería", category: "familiar" },
                { id: 9, text: "Sistemas de Evaluación Virtual", category: "academico" },
                { id: 10, text: "Automatización Industrial", category: "laboral" },
                { id: 11, text: "Bibliotecas Digitales", category: "academico" },
                { id: 12, text: "Dispositivos Wearables", category: "familiar" },
                { id: 13, text: "CRM (Gestión de Clientes)", category: "laboral" },
                { id: 14, text: "Simuladores de Laboratorio", category: "academico" },
                { id: 15, text: "Asistentes Virtuales", category: "familiar" },
                { id: 16, text: "Sistemas de Nómina", category: "laboral" }
            ];

            // Explanations for each item
            const explanations = {
                1: "Los sistemas de gestión académica transforman la administración de instituciones educativas, facilitando procesos como matrículas, calificaciones y control de asistencia.",
                2: "Las videoconferencias han revolucionado la comunicación familiar, permitiendo mantener contacto visual y emocional con seres queridos a distancia.",
                3: "Las plataformas de e-learning democratizan el acceso a la educación, permitiendo aprendizaje flexible y personalizado a millones de estudiantes.",
                4: "Las redes sociales han transformado la forma en que las familias comparten experiencias, mantienen relaciones y construyen comunidades virtuales.",
                5: "El software de contabilidad automatiza procesos financieros empresariales, aumentando eficiencia y reduciendo errores humanos.",
                6: "Las Smart TVs integran entretenimiento y conectividad en el hogar, permitiendo acceso a contenido multimedia y servicios digitales.",
                7: "Los sistemas ERP integran todas las áreas de una empresa, optimizando procesos y mejorando la toma de decisiones estratégicas.",
                8: "Las aplicaciones de mensajería facilitan la comunicación instantánea entre familiares, manteniendo vínculos emocionales fuertes.",
                9: "Los sistemas de evaluación virtual permiten medir el desempeño académico de manera objetiva y eficiente, adaptándose a nuevas formas de enseñanza.",
                10: "La automatización industrial incrementa productividad y calidad en procesos manufactureros, transformando el panorama laboral.",
                11: "Las bibliotecas digitales preservan y distribuyen conocimiento globalmente, eliminando barreras geográficas y temporales.",
                12: "Los dispositivos wearables monitorean salud y actividad física, integrando tecnología en la vida diaria familiar.",
                13: "Los sistemas CRM gestionan relaciones con clientes, aumentando la eficacia comercial y la satisfacción del cliente.",
                14: "Los simuladores de laboratorio permiten prácticas seguras y repetibles en entornos controlados, mejorando la calidad educativa.",
                15: "Los asistentes virtuales automatizan tareas domésticas y proporcionan información, facilitando la gestión del hogar.",
                16: "Los sistemas de nómina automatizan cálculos salariales y cumplimiento legal, mejorando la eficiencia en recursos humanos."
            };

            // State management
            let state = {
                items: [],
                score: 0,
                total: 0,
                currentDragItem: null,
                currentDragElement: null
            };

            // Initialize the game
            function initGame() {
                // Shuffle items using Fisher-Yates algorithm
                const shuffledItems = [...items];
                for (let i = shuffledItems.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [shuffledItems[i], shuffledItems[j]] = [shuffledItems[j], shuffledItems[i]];
                }
                
                state.items = shuffledItems.map(item => ({
                    ...item,
                    isCorrectlyPlaced: false,
                    currentContainer: null
                }));
                
                state.score = 0;
                state.total = items.length;
                
                renderItems();
                updateScore();
            }

            // Render items in source container
            function renderItems() {
                const sourceContainer = document.getElementById('items-source');
                sourceContainer.innerHTML = '';
                
                state.items.forEach(item => {
                    if (!item.isCorrectlyPlaced && !item.currentContainer) {
                        const itemElement = createItemElement(item);
                        sourceContainer.appendChild(itemElement);
                    }
                });
            }

            // Create item element with drag events
            function createItemElement(item) {
                const element = document.createElement('div');
                element.className = 'item-card';
                element.textContent = item.text;
                element.draggable = true;
                element.dataset.itemId = item.id;

                // Mouse events
                element.addEventListener('mousedown', startDrag);
                element.addEventListener('mouseup', endDrag);

                // Touch events
                element.addEventListener('touchstart', handleTouchStart);
                element.addEventListener('touchend', handleTouchEnd);

                return element;
            }

            // Start drag with mouse
            function startDrag(e) {
                const itemElement = e.target.closest('.item-card');
                if (!itemElement) return;

                const itemId = parseInt(itemElement.dataset.itemId);
                state.currentDragItem = state.items.find(item => item.id === itemId);
                state.currentDragElement = itemElement;

                itemElement.classList.add('dragging');
                
                document.addEventListener('mousemove', handleMouseMove);
                document.addEventListener('mouseup', endDrag);
            }

            // Handle mouse move during drag
            function handleMouseMove(e) {
                if (!state.currentDragElement) return;
                
                const rect = state.currentDragElement.getBoundingClientRect();
                const x = e.clientX - rect.width / 2;
                const y = e.clientY - rect.height / 2;
                
                state.currentDragElement.style.position = 'fixed';
                state.currentDragElement.style.left = x + 'px';
                state.currentDragElement.style.top = y + 'px';
                state.currentDragElement.style.zIndex = '1000';

                // Highlight target containers
                highlightTargetContainers(e.clientX, e.clientY);
            }

            // End drag
            function endDrag(e) {
                if (!state.currentDragElement) return;

                document.removeEventListener('mousemove', handleMouseMove);
                document.removeEventListener('mouseup', endDrag);

                // Reset dragging styles
                state.currentDragElement.style.position = '';
                state.currentDragElement.style.left = '';
                state.currentDragElement.style.top = '';
                state.currentDragElement.style.zIndex = '';
                state.currentDragElement.classList.remove('dragging');

                // Find target container
                const targetContainer = document.elementFromPoint(
                    e.clientX, 
                    e.clientY
                )?.closest('.category-container');

                if (targetContainer && state.currentDragItem) {
                    handleDrop(targetContainer, state.currentDragItem, state.currentDragElement);
                } else if (state.currentDragItem) {
                    // Return to source if dropped outside valid target
                    resetItemToSource(state.currentDragElement);
                }

                state.currentDragItem = null;
                state.currentDragElement = null;
            }

            // Handle touch start
            function handleTouchStart(e) {
                const itemElement = e.target.closest('.item-card');
                if (!itemElement) return;

                e.preventDefault();
                const itemId = parseInt(itemElement.dataset.itemId);
                state.currentDragItem = state.items.find(item => item.id === itemId);
                state.currentDragElement = itemElement;

                itemElement.classList.add('dragging');
                
                document.addEventListener('touchmove', handleTouchMove, { passive: false });
                document.addEventListener('touchend', handleTouchEnd);
            }

            // Handle touch move
            function handleTouchMove(e) {
                if (!state.currentDragElement || !e.touches[0]) return;
                
                e.preventDefault();
                const touch = e.touches[0];
                const rect = state.currentDragElement.getBoundingClientRect();
                const x = touch.clientX - rect.width / 2;
                const y = touch.clientY - rect.height / 2;
                
                state.currentDragElement.style.position = 'fixed';
                state.currentDragElement.style.left = x + 'px';
                state.currentDragElement.style.top = y + 'px';
                state.currentDragElement.style.zIndex = '1000';

                // Highlight target containers
                highlightTargetContainers(touch.clientX, touch.clientY);
            }

            // Handle touch end
            function handleTouchEnd(e) {
                if (!state.currentDragElement) return;

                document.removeEventListener('touchmove', handleTouchMove);
                document.removeEventListener('touchend', handleTouchEnd);

                // Reset dragging styles
                state.currentDragElement.style.position = '';
                state.currentDragElement.style.left = '';
                state.currentDragElement.style.top = '';
                state.currentDragElement.style.zIndex = '';
                state.currentDragElement.classList.remove('dragging');

                // Find target container using the last touch point
                if (e.changedTouches && e.changedTouches[0]) {
                    const touch = e.changedTouches[0];
                    const targetContainer = document.elementFromPoint(
                        touch.clientX, 
                        touch.clientY
                    )?.closest('.category-container');

                    if (targetContainer && state.currentDragItem) {
                        handleDrop(targetContainer, state.currentDragItem, state.currentDragElement);
                    } else if (state.currentDragItem) {
                        resetItemToSource(state.currentDragElement);
                    }
                }

                state.currentDragItem = null;
                state.currentDragElement = null;
            }

            // Highlight target containers based on mouse/touch position
            function highlightTargetContainers(x, y) {
                const elements = document.elementsFromPoint(x, y);
                const targetContainers = elements.filter(el => 
                    el.classList.contains('category-container')
                );

                document.querySelectorAll('.category-container').forEach(container => {
                    container.classList.remove('highlight');
                });

                if (targetContainers.length > 0) {
                    targetContainers[0].classList.add('highlight');
                }
            }

            // Handle drop into target container
            function handleDrop(targetContainer, item, element) {
                const targetCategory = targetContainer.dataset.category;
                
                if (targetCategory === item.category) {
                    // Correct placement
                    item.isCorrectlyPlaced = true;
                    item.currentContainer = targetCategory;
                    state.score++;
                    
                    // Remove from source and add to target
                    element.remove();
                    const droppedItemElement = document.createElement('div');
                    droppedItemElement.className = 'dropped-item correct';
                    droppedItemElement.textContent = item.text;
                    targetContainer.querySelector('.dropped-items').appendChild(droppedItemElement);
                    
                    // Add correct class to container
                    targetContainer.classList.add('correct');
                } else {
                    // Incorrect placement
                    targetContainer.classList.add('incorrect');
                    targetContainer.classList.add('shake');
                    
                    setTimeout(() => {
                        targetContainer.classList.remove('incorrect', 'shake');
                        resetItemToSource(element);
                    }, 500);
                }
                
                updateScore();
                checkCompletion();
            }

            // Reset item to source container
            function resetItemToSource(element) {
                const itemId = parseInt(element.dataset.itemId);
                const item = state.items.find(i => i.id === itemId);
                
                if (item) {
                    item.currentContainer = null;
                    element.style.position = '';
                    element.style.left = '';
                    element.style.top = '';
                    element.style.zIndex = '';
                    
                    document.getElementById('items-source').appendChild(element);
                }
            }

            // Update score display
            function updateScore() {
                document.getElementById('score').textContent = state.score;
                document.getElementById('total').textContent = state.total;
            }

            // Check if all items are placed
            function checkCompletion() {
                const completed = state.items.every(item => item.isCorrectlyPlaced);
                if (completed) {
                    showResults();
                }
            }

            // Show results screen
            function showResults() {
                const percentage = Math.round((state.score / state.total) * 100);
                document.getElementById('final-score').textContent = percentage + '%';
                
                let performanceText = '';
                if (percentage >= 90) {
                    performanceText = '¡Excelente! Has comprendido perfectamente la clasificación de tecnologías.';
                } else if (percentage >= 70) {
                    performanceText = '¡Muy bien! Tienes un buen entendimiento del impacto tecnológico.';
                } else if (percentage >= 50) {
                    performanceText = 'Bien hecho. Puedes mejorar tu comprensión con más práctica.';
                } else {
                    performanceText = 'Sigue practicando para mejorar tu comprensión del impacto tecnológico.';
                }
                
                document.getElementById('performance-text').textContent = performanceText;
                
                // Populate explanations
                const explanationList = document.getElementById('explanation-list');
                explanationList.innerHTML = '';
                
                items.forEach(item => {
                    const li = document.createElement('li');
                    li.innerHTML = `<strong>${item.text}:</strong> ${explanations[item.id]}`;
                    explanationList.appendChild(li);
                });
                
                document.getElementById('results-screen').classList.add('active');
            }

            // Reset button functionality
            document.getElementById('reset-btn').addEventListener('click', function() {
                // Clear all dropped items
                document.querySelectorAll('.dropped-items').forEach(container => {
                    container.innerHTML = '';
                });
                
                // Remove highlight classes
                document.querySelectorAll('.category-container').forEach(container => {
                    container.classList.remove('highlight', 'correct', 'incorrect');
                });
                
                // Reinitialize game
                initGame();
                
                // Hide results screen
                document.getElementById('results-screen').classList.remove('active');
            });

            // Check button functionality
            document.getElementById('check-btn').addEventListener('click', function() {
                checkCompletion();
            });

            // Initialize the game
            initGame();
        });
    </script>
</body>
</html>
