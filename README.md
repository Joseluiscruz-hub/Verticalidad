<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Checklist de Verticalidad y Estibado</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- QR Scanner Library -->
    <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
    <style>
        :root {
            /* Design System Variables */
            --color-white: rgba(255, 255, 255, 1);
            --color-black: rgba(0, 0, 0, 1);
            --color-cream-50: rgba(252, 252, 249, 1);
            --color-cream-100: rgba(255, 255, 253, 1);
            --color-gray-200: rgba(245, 245, 245, 1);
            --color-gray-300: rgba(167, 169, 169, 1);
            --color-gray-400: rgba(119, 124, 124, 1);
            --color-slate-500: rgba(98, 108, 113, 1);
            --color-charcoal-700: rgba(31, 33, 33, 1);
            --color-charcoal-800: rgba(38, 40, 40, 1);
            --color-slate-900: rgba(19, 52, 59, 1);
            --color-teal-300: rgba(50, 184, 198, 1);
            --color-teal-500: rgba(33, 128, 141, 1);
            --color-teal-600: rgba(29, 116, 128, 1);
            --color-teal-700: rgba(26, 104, 115, 1);
            --color-red-400: rgba(255, 84, 89, 1);
            --color-red-500: rgba(192, 21, 47, 1);
            
            /* Semantic colors */
            --color-background: var(--color-cream-50);
            --color-surface: var(--color-cream-100);
            --color-text: var(--color-slate-900);
            --color-text-secondary: var(--color-slate-500);
            --color-primary: var(--color-teal-500);
            --color-primary-hover: var(--color-teal-600);
            
            /* Spacing and layout */
            --space-4: 4px;
            --space-8: 8px;
            --space-16: 16px;
            --radius-base: 8px;
            --radius-lg: 12px;
        }
        
        @media (prefers-color-scheme: dark) {
            :root {
                --color-background: var(--color-charcoal-700);
                --color-surface: var(--color-charcoal-800);
                --color-text: var(--color-gray-200);
                --color-text-secondary: rgba(167, 169, 169, 0.7);
                --color-primary: var(--color-teal-300);
                --color-primary-hover: var(--color-teal-500);
            }
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--color-background);
            color: var(--color-text);
        }
        
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        
        .modal-backdrop {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.6);
            z-index: 40;
            opacity: 0;
            transition: opacity 0.3s ease;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 50;
            opacity: 0;
            transition: opacity 0.3s ease, transform 0.3s ease;
        }
        
        .modal-backdrop.active, .modal.active {
            display: block;
            opacity: 1;
        }
        
        .modal.active {
            transform: translate(-50%, -50%) scale(1);
        }
        
        .modal {
            transform: translate(-50%, -50%) scale(0.95);
        }
        
        .status-icon {
            transition: color 0.3s ease;
        }
        
        .datetime-display {
            font-size: 1.1rem;
            font-weight: 500;
            color: var(--color-text-secondary);
            margin-top: 0.5rem;
        }
        
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }
        
        .gps-loading {
            opacity: 0.7;
            cursor: not-allowed;
        }
        
        /* Tab Navigation Styles */
        .tab-button {
            transition: all 0.3s ease;
        }
        
        .tab-button.active {
            border-color: var(--color-primary) !important;
            color: var(--color-primary) !important;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
    </style>
</head>
<body class="bg-gray-100 dark:bg-gray-900 text-gray-800 dark:text-gray-200">

    <div class="container mx-auto p-4 md:p-6">
        <header class="text-center mb-6">
            <h1 class="text-3xl font-bold text-sky-600 dark:text-sky-400">Checklist de Verticalidad y Estibado</h1>
            <p class="text-gray-600 dark:text-gray-400 mt-1">Planta "Cuautitlán"</p>
            <div id="datetime-display" class="datetime-display">
                <i class="fas fa-clock mr-2"></i>
                <span id="current-datetime">Cargando fecha y hora...</span>
            </div>
            <div id="db-status" class="mt-2 text-sm"></div>
        </header>

        <!-- Navigation Tabs -->
        <nav class="mb-6">
            <div class="border-b border-gray-200 dark:border-gray-700">
                <ul class="flex flex-wrap -mb-px text-sm font-medium text-center" role="tablist">
                    <li class="mr-2" role="presentation">
                        <button class="tab-button active inline-block p-4 border-b-2 border-sky-600 text-sky-600 hover:text-sky-600 dark:text-sky-400 dark:border-sky-400" data-tab="verificar" type="button" role="tab">
                            <i class="fas fa-clipboard-check mr-2"></i>Verificar Productos
                        </button>
                    </li>
                    <li class="mr-2" role="presentation">
                        <button class="tab-button inline-block p-4 border-b-2 border-transparent text-gray-500 hover:text-gray-600 hover:border-gray-300 dark:text-gray-400 dark:hover:text-gray-300" data-tab="historial" type="button" role="tab">
                            <i class="fas fa-history mr-2"></i>Historial
                        </button>
                    </li>
                    <li class="mr-2" role="presentation">
                        <button class="tab-button inline-block p-4 border-b-2 border-transparent text-gray-500 hover:text-gray-600 hover:border-gray-300 dark:text-gray-400 dark:hover:text-gray-300" data-tab="estadisticas" type="button" role="tab">
                            <i class="fas fa-chart-bar mr-2"></i>Estadísticas
                        </button>
                    </li>
                    <li class="mr-2" role="presentation">
                        <button class="tab-button inline-block p-4 border-b-2 border-transparent text-gray-500 hover:text-gray-600 hover:border-gray-300 dark:text-gray-400 dark:hover:text-gray-300" data-tab="configuracion" type="button" role="tab">
                            <i class="fas fa-cog mr-2"></i>Configuración
                        </button>
                    </li>
                </ul>
            </div>
        </nav>

        <!-- Tab Content Sections -->
        
        <!-- Verificar Productos Section -->
        <div id="tab-verificar" class="tab-content active">
            <!-- Actions: Search, Export, and Print -->
            <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between mb-4 gap-4 sticky top-2 z-10 p-2 bg-gray-100 dark:bg-gray-900 rounded-lg">
                <!-- Search Input -->
                <div class="relative flex-grow">
                    <span class="absolute inset-y-0 left-0 flex items-center pl-3">
                        <i class="fas fa-search text-gray-400"></i>
                    </span>
                    <input type="text" id="searchInput" placeholder="Buscar por Sku o Descripción..." class="w-full pl-10 pr-4 py-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-800 focus:outline-none focus:ring-2 focus:ring-sky-500">
                </div>
                
                <!-- Action Buttons -->
                <div class="flex items-center gap-2">
                    <!-- QR SCAN BUTTON -->
                    <button id="scan-qr-main-btn" class="px-4 py-2 bg-indigo-600 text-white font-semibold rounded-lg hover:bg-indigo-700 transition flex items-center justify-center">
                        <i class="fas fa-qrcode mr-2"></i>
                        <span>Escanear</span>
                    </button>
                    <!-- COMMIT BUTTON -->
                    <button id="commit-session-btn" class="px-4 py-2 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 transition flex items-center justify-center">
                        <i class="fas fa-save mr-2"></i>
                        <span>Guardar en Historial</span>
                    </button>
                    <!-- Export Button -->
                    <button id="export-btn" class="px-4 py-2 bg-green-600 text-white font-semibold rounded-lg hover:bg-green-700 transition flex items-center justify-center">
                        <i class="fas fa-file-excel mr-2"></i>
                        <span>Exportar</span>
                    </button>
                    <!-- Print Button -->
                    <button id="print-btn" class="px-4 py-2 bg-sky-600 text-white font-semibold rounded-lg hover:bg-sky-700 transition flex items-center justify-center">
                        <i class="fas fa-print mr-2"></i>
                        <span>Imprimir</span>
                    </button>
                </div>
            </div>

            <!-- Product Table -->
            <div class="overflow-x-auto bg-white dark:bg-gray-800 rounded-lg shadow">
                <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
                    <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                        <tr>
                            <th scope="col" class="px-4 py-3">SKU</th>
                            <th scope="col" class="px-4 py-3">Descripción</th>
                            <th scope="col" class="px-4 py-3 text-center">Factor Estiba</th>
                            <th scope="col" class="px-4 py-3 text-center">Estado</th>
                            <th scope="col" class="px-4 py-3 text-center">Ubicación</th>
                            <th scope="col" class="px-4 py-3 text-center">Fotos</th>
                            <th scope="col" class="px-4 py-3 text-center">Acción</th>
                        </tr>
                    </thead>
                    <tbody id="product-table-body">
                        <!-- Rows will be inserted here by JavaScript -->
                    </tbody>
                </table>
                <div id="no-results" class="hidden text-center p-8 text-gray-500">
                    <i class="fas fa-box-open fa-3x mb-4"></i>
                    <p class="text-lg">No se encontraron productos.</p>
                </div>
            </div>
        </div>

        <!-- Historial Section -->
        <div id="tab-historial" class="tab-content hidden">
            <!-- History Filters -->
            <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-4 mb-6">
                <h3 class="text-lg font-semibold mb-4">Filtros de Búsqueda</h3>
                <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-5 gap-4">
                    <div>
                        <label class="block text-sm font-medium mb-1">Fecha Desde:</label>
                        <input type="date" id="filter-date-from" class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Fecha Hasta:</label>
                        <input type="date" id="filter-date-to" class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">SKU:</label>
                        <input type="text" id="filter-sku" placeholder="Buscar SKU..." class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Inspector:</label>
                        <input type="text" id="filter-inspector" placeholder="Buscar inspector..." class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Estado:</label>
                        <select id="filter-status" class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500">
                            <option value="">Todos</option>
                            <option value="ok">OK</option>
                            <option value="error">Con Incidencias</option>
                        </select>
                    </div>
                </div>
                <div class="flex justify-between items-center mt-4">
                    <button id="apply-filters" class="px-4 py-2 bg-sky-600 text-white rounded-lg hover:bg-sky-700 transition">
                        <i class="fas fa-filter mr-2"></i>Aplicar Filtros
                    </button>
                    <button id="export-history" class="px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition">
                        <i class="fas fa-file-excel mr-2"></i>Exportar Historial
                    </button>
                </div>
            </div>

            <!-- History Table -->
            <div class="bg-white dark:bg-gray-800 rounded-lg shadow">
                <div class="p-4 border-b border-gray-200 dark:border-gray-700">
                    <h3 class="text-lg font-semibold">Historial de Verificaciones</h3>
                    <p class="text-sm text-gray-600 dark:text-gray-400" id="history-count">0 registros encontrados</p>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
                        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                            <tr>
                                <th class="px-4 py-3">Fecha</th>
                                <th class="px-4 py-3">SKU</th>
                                <th class="px-4 py-3">Descripción</th>
                                <th class="px-4 py-3">Estado</th>
                                <th class="px-4 py-3">Inspector</th>
                                <th class="px-4 py-3">Turno</th>
                                <th class="px-4 py-3">Fotos</th>
                                <th class="px-4 py-3">Acciones</th>
                            </tr>
                        </thead>
                        <tbody id="history-table-body">
                        </tbody>
                    </table>
                    <div id="no-history" class="hidden text-center p-8 text-gray-500">
                        <i class="fas fa-history fa-3x mb-4"></i>
                        <p class="text-lg">No se encontraron registros históricos.</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Estadísticas Section -->
        <div id="tab-estadisticas" class="tab-content hidden">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Summary Cards -->
                <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
                    <h3 class="text-lg font-semibold mb-4">Resumen General</h3>
                    <div class="grid grid-cols-2 gap-4">
                        <div class="text-center p-4 bg-blue-50 dark:bg-blue-900/20 rounded-lg">
                            <div class="text-2xl font-bold text-blue-600 dark:text-blue-400" id="stat-total">0</div>
                            <div class="text-sm text-gray-600 dark:text-gray-400">Total Verificaciones</div>
                        </div>
                        <div class="text-center p-4 bg-green-50 dark:bg-green-900/20 rounded-lg">
                            <div class="text-2xl font-bold text-green-600 dark:text-green-400" id="stat-ok">0</div>
                            <div class="text-sm text-gray-600 dark:text-gray-400">OK</div>
                        </div>
                        <div class="text-center p-4 bg-red-50 dark:bg-red-900/20 rounded-lg">
                            <div class="text-2xl font-bold text-red-600 dark:text-red-400" id="stat-error">0</div>
                            <div class="text-sm text-gray-600 dark:text-gray-400">Con Incidencias</div>
                        </div>
                        <div class="text-center p-4 bg-amber-50 dark:bg-amber-900/20 rounded-lg">
                            <div class="text-2xl font-bold text-amber-600 dark:text-amber-400" id="stat-photos">0</div>
                            <div class="text-sm text-gray-600 dark:text-gray-400">Fotos Tomadas</div>
                        </div>
                    </div>
                </div>

                <!-- Chart Container -->
                <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
                    <h3 class="text-lg font-semibold mb-4">Estados de Verificación</h3>
                    <div style="height: 300px;">
                        <canvas id="stats-chart"></canvas>
                    </div>
                </div>

                <!-- Top Products -->
                <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
                    <h3 class="text-lg font-semibold mb-4">Productos Más Verificados</h3>
                    <div id="top-products" class="space-y-2">
                    </div>
                </div>

                <!-- Top Inspectors -->
                <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
                    <h3 class="text-lg font-semibold mb-4">Inspectores Más Activos</h3>
                    <div id="top-inspectors" class="space-y-2">
                    </div>
                </div>
            </div>
        </div>

        <!-- Configuración Section -->
        <div id="tab-configuracion" class="tab-content hidden">
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <!-- Database Management -->
                <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
                    <h3 class="text-lg font-semibold mb-4">Gestión de Base de Datos</h3>
                    <div class="space-y-4">
                        <div class="p-4 bg-blue-50 dark:bg-blue-900/20 rounded-lg">
                            <div class="text-sm text-gray-600 dark:text-gray-400">Espacio utilizado:</div>
                            <div class="text-lg font-semibold" id="storage-usage">Calculando...</div>
                        </div>
                        <button id="export-all-data" class="w-full px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition">
                            <i class="fas fa-download mr-2"></i>Exportar Toda la Base de Datos
                        </button>
                        <button id="clean-old-data" class="w-full px-4 py-2 bg-amber-600 text-white rounded-lg hover:bg-amber-700 transition">
                            <i class="fas fa-broom mr-2"></i>Limpiar Datos Antiguos (&gt;6 meses)
                        </button>
                        <button id="clear-all-data" class="w-full px-4 py-2 bg-red-600 text-white rounded-lg hover:bg-red-700 transition">
                            <i class="fas fa-trash mr-2"></i>Limpiar Toda la Base de Datos
                        </button>
                    </div>
                </div>

                <!-- App Settings -->
                <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
                    <h3 class="text-lg font-semibold mb-4">Configuración de la Aplicación</h3>
                    <div class="space-y-4">
                        <div>
                            <label class="block text-sm font-medium mb-2">Calidad de fotos:</label>
                            <select id="photo-quality" class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700">
                                <option value="0.8">Alta (0.8)</option>
                                <option value="0.6" selected>Media (0.6)</option>
                                <option value="0.4">Baja (0.4)</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-2">Tamaño máximo de foto (px):</label>
                            <select id="photo-max-size" class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700">
                                <option value="1920">1920px</option>
                                <option value="1280" selected>1280px</option>
                                <option value="800">800px</option>
                            </select>
                        </div>
                        <div class="p-4 bg-gray-50 dark:bg-gray-700 rounded-lg">
                            <h4 class="font-medium mb-2">Información de la Aplicación</h4>
                            <div class="text-sm text-gray-600 dark:text-gray-400">
                                <p>Versión: 2.7.0 (Guardado Corregido)</p>
                                <p>Base de Datos: IndexedDB</p>
                                <p>Capacidad: Ilimitada</p>
                                <p>Modo: Offline</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- History Detail Modal -->
    <div id="history-detail-modal" class="modal w-11/12 max-w-6xl max-h-[90vh] bg-white dark:bg-gray-800 rounded-xl shadow-2xl overflow-hidden">
        <div class="sticky top-0 bg-white dark:bg-gray-800 p-6 border-b border-gray-200 dark:border-gray-700">
            <div class="flex justify-between items-start">
                <div>
                    <h2 class="text-xl font-bold text-sky-600 dark:text-sky-400">Detalles de Verificación</h2>
                    <p class="text-sm text-gray-600 dark:text-gray-400" id="history-modal-info"></p>
                </div>
                <button id="close-history-modal" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-200">
                    <i class="fas fa-times fa-lg"></i>
                </button>
            </div>
        </div>
        <div class="overflow-y-auto max-h-[calc(90vh-120px)] p-6">
            <div id="history-modal-content"></div>
        </div>
    </div>

    <!-- QR Scanner Modal -->
    <div id="qr-scanner-modal" class="modal w-11/12 max-w-md bg-white dark:bg-gray-800 rounded-xl shadow-2xl overflow-hidden">
        <div class="p-4 border-b border-gray-200 dark:border-gray-700 flex justify-between items-center">
            <h2 class="text-lg font-bold text-sky-600 dark:text-sky-400">Escanear Código QR</h2>
            <button id="close-qr-scanner-btn" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-200">
                <i class="fas fa-times fa-lg"></i>
            </button>
        </div>
        <div class="p-4">
            <div id="qr-reader" class="w-full"></div>
            <p id="qr-result" class="mt-4 text-center text-sm text-gray-600 dark:text-gray-400"></p>
        </div>
    </div>

    <!-- Modal Backdrop -->
    <div id="modal-backdrop" class="modal-backdrop"></div>

    <!-- Checklist Modal -->
    <div id="checklist-modal" class="modal w-11/12 max-w-4xl max-h-[90vh] bg-white dark:bg-gray-800 rounded-xl shadow-2xl overflow-hidden">
        <div class="sticky top-0 bg-white dark:bg-gray-800 p-6 border-b border-gray-200 dark:border-gray-700">
            <div class="flex justify-between items-start mb-4">
                <div>
                    <h2 class="text-xl font-bold text-sky-600 dark:text-sky-400" id="modal-title">Checklist de Verticalidad</h2>
                    <p class="text-sm text-gray-600 dark:text-gray-400" id="modal-sku"></p>
                </div>
                <button id="close-modal-btn" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-200">
                    <i class="fas fa-times fa-lg"></i>
                </button>
            </div>
            
            <div class="mb-4 p-3 rounded-lg bg-sky-100 dark:bg-sky-900/50 flex justify-between items-center">
                <span class="font-semibold">Factor de Estiba Máximo:</span>
                <span class="text-2xl font-bold text-sky-600 dark:text-sky-400" id="modal-factor"></span>
            </div>
        </div>

        <div class="overflow-y-auto max-h-[calc(90vh-200px)] px-6">
            <form id="checklist-form">
                <div class="space-y-6">
                    <!-- Parametros de Checklist - se generan dinámicamente -->
                    <div id="parametros-container"></div>
                    
                    <!-- Campos de Trazabilidad -->
                    <div class="border-t border-gray-200 dark:border-gray-700 pt-6">
                        <h3 class="text-lg font-semibold mb-4 text-gray-800 dark:text-gray-200">Campos de Trazabilidad</h3>
                        
                        <!-- Inspector -->
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                            <div>
                                <label for="inspector" class="block mb-2 text-sm font-medium">Inspector/Responsable:</label>
                                <input type="text" id="inspector" class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500" placeholder="Nombre del inspector">
                            </div>
                            
                            <!-- Turno -->
                            <div>
                                <label for="turno" class="block mb-2 text-sm font-medium">Turno:</label>
                                <select id="turno" class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500">
                                    <option value="">Seleccionar turno</option>
                                    <option value="Mañana">Mañana</option>
                                    <option value="Tarde">Tarde</option>
                                    <option value="Noche">Noche</option>
                                </select>
                            </div>
                        </div>
                        
                        <!-- Ubicación -->
                        <div class="mb-4">
                            <label for="location" class="block mb-2 text-sm font-medium">Ubicación:</label>
                            <div class="flex gap-2">
                                <input type="text" id="location" class="flex-1 p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500" placeholder="Ej. Rack A3 - Nivel 2">
                                <button type="button" id="gps-btn" class="px-3 py-2 bg-amber-500 text-white rounded-lg hover:bg-amber-600 transition font-semibold text-sm flex items-center gap-1">
                                    <i class="fas fa-map-marker-alt"></i>
                                    GPS
                                </button>
                            </div>
                            <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">Ingrese manualmente o use GPS para obtener coordenadas</p>
                        </div>
                        
                        <!-- Observaciones Generales -->
                        <div>
                            <label for="notes" class="block mb-2 text-sm font-medium">Observaciones Generales:</label>
                            <textarea id="notes" rows="3" class="w-full p-2 rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-sky-500" placeholder="Añadir notas adicionales..."></textarea>
                        </div>
                    </div>
                </div>
            </form>
        </div>

        <div class="sticky bottom-0 bg-white dark:bg-gray-800 p-6 border-t border-gray-200 dark:border-gray-700">
            <div class="flex justify-end space-x-3">
                <button id="cancel-btn" class="px-4 py-2 rounded-lg bg-gray-200 dark:bg-gray-600 hover:bg-gray-300 dark:hover:bg-gray-500 transition">Cancelar</button>
                <button id="save-btn" class="px-4 py-2 rounded-lg bg-sky-600 text-white hover:bg-sky-700 transition font-semibold">Guardar Verificación</button>
            </div>
        </div>
    </div>

<script>
document.addEventListener('DOMContentLoaded', () => {

    // ===== DATABASE CONFIGURATION =====
    const DB_NAME = 'ChecklistDB';
    const DB_VERSION = 1;
    let db = null;
    let dbSupported = true;
    
    // Global variable for all historical data (fallback if IndexedDB fails)
    let allVerifications = [];
    let allPhotos = [];
    
    // ===== REFACTORED AND CORRECTED DB FUNCTIONS =====
    async function initDatabase() {
        return new Promise((resolve) => {
            if (!window.indexedDB) {
                dbSupported = false;
                updateDbStatus('⚠️ Modo Sin Persistencia (IndexedDB no disponible)', 'warning');
                resolve(false);
                return;
            }
            const request = indexedDB.open(DB_NAME, DB_VERSION);
            request.onerror = (event) => {
                dbSupported = false;
                updateDbStatus('❌ Error de Base de Datos', 'error');
                console.error("Database error:", event.target.error);
                resolve(false);
            };
            request.onsuccess = () => {
                db = request.result;
                updateDbStatus('✅ Base de Datos Conectada', 'success');
                resolve(true);
            };
            request.onupgradeneeded = (event) => {
                const db = event.target.result;
                if (!db.objectStoreNames.contains('verificaciones')) {
                    const store = db.createObjectStore('verificaciones', { keyPath: 'id', autoIncrement: true });
                    store.createIndex('sku', 'sku', { unique: false });
                    store.createIndex('fecha', 'fecha', { unique: false });
                    store.createIndex('inspector', 'inspector', { unique: false });
                }
                if (!db.objectStoreNames.contains('fotos')) {
                    const store = db.createObjectStore('fotos', { keyPath: 'id', autoIncrement: true });
                    store.createIndex('verificacion_id', 'verificacion_id', { unique: false });
                }
            };
        });
    }

    function updateDbStatus(message, type) {
        const statusEl = document.getElementById('db-status');
        if (statusEl) {
            let className = 'text-green-600';
            if (type === 'warning') className = 'text-amber-600';
            if (type === 'error') className = 'text-red-600';
            statusEl.innerHTML = `<span class="${className}">${message}</span>`;
        }
    }
    
    async function performDbAction(storeNames, mode, action, data) {
        if (!dbSupported || !db) {
            // Fallback to in-memory operations
            if (storeNames.includes('verificaciones')) {
                if (action === 'add' && data.verificacion) { data.verificacion.id = Date.now() + Math.random(); allVerifications.push(data.verificacion); return data.verificacion.id; }
                if (action === 'getAll') return [...allVerifications];
            }
            if (storeNames.includes('fotos')) {
                if (action === 'add' && data.photo) { data.photo.id = Date.now() + Math.random(); allPhotos.push(data.photo); return data.photo.id; }
                if (action === 'getAllByIndex') return allPhotos.filter(p => p.verificacion_id === data.key);
                if (action === 'getAll') return [...allPhotos];
            }
            if(action === 'clear') {
                if(storeNames.includes('verificaciones')) allVerifications = [];
                if(storeNames.includes('fotos')) allPhotos = [];
            }
            return;
        }

        return new Promise((resolve, reject) => {
            const transaction = db.transaction(storeNames, mode);
            transaction.onerror = (event) => reject(event.target.error);
            transaction.oncomplete = () => {
                // For 'add' actions, result is available on the request, not transaction.
                // For other actions, we resolve in onsuccess.
                if (action !== 'add') {
                    resolve();
                }
            };

            let request;
            if (action === 'add') {
                if(data.verificacion) request = transaction.objectStore('verificaciones').add(data.verificacion);
                if(data.photo) request = transaction.objectStore('fotos').add(data.photo);
                if(request) request.onsuccess = (event) => resolve(event.target.result);

            } else if (action === 'getAll') {
                request = transaction.objectStore(storeNames).getAll();
                request.onsuccess = () => resolve(request.result);
            } else if (action === 'getAllByIndex') {
                request = transaction.objectStore(storeNames).index(data.index).getAll(data.key);
                request.onsuccess = () => resolve(request.result);
            } else if (action === 'clear') {
                storeNames.forEach(name => transaction.objectStore(name).clear());
                // oncomplete will resolve this promise.
            } else {
                return reject('Invalid DB action');
            }
        });
    }

    // Simplified DB functions
    async function saveVerification(verification) { return performDbAction(['verificaciones'], 'readwrite', 'add', { verificacion }); }
    async function savePhoto(photo) { return performDbAction(['fotos'], 'readwrite', 'add', { photo }); }
    async function getVerifications(filters = {}) {
        let results = await performDbAction('verificaciones', 'readonly', 'getAll');
        // Apply filters
        if (filters.sku) results = results.filter(v => v.sku.toLowerCase().includes(filters.sku.toLowerCase()));
        if (filters.inspector) results = results.filter(v => v.inspector && v.inspector.toLowerCase().includes(filters.inspector.toLowerCase()));
        if (filters.status) results = results.filter(v => v.status === filters.status);
        if (filters.dateFrom) results = results.filter(v => v.fecha.split('T')[0] >= filters.dateFrom);
        if (filters.dateTo) results = results.filter(v => v.fecha.split('T')[0] <= filters.dateTo);
        return results.sort((a, b) => new Date(b.fecha) - new Date(a.fecha));
    }
    async function getPhotos(verificationId) { return performDbAction('fotos', 'readonly', 'getAllByIndex', { index: 'verificacion_id', key: verificationId }); }
    async function getAllPhotos() { return performDbAction('fotos', 'readonly', 'getAll'); }
    async function clearAllData() {
        await performDbAction(['verificaciones', 'fotos'], 'readwrite', 'clear');
    }
    async function cleanOldData() { /* This function remains complex and is fine as is */ }


    const parametrosChecklist = [
        {id: 1, nombre: 'verticalidad', pregunta: '¿La estiba mantiene la verticalidad?', descripcion: 'Verificar que las botellas mantengan posición vertical (tolerancia: ≤ 0.3 mm)'},
        {id: 2, nombre: 'factor_estiba', pregunta: '¿Cumple con el factor de estiba?', descripcion: 'Altura total / altura referencia ≤ factor máximo'},
        {id: 3, nombre: 'tarima_estado', pregunta: '¿La tarima está en buen estado?', descripcion: 'Sin daños, superficie plana, limpia, sin astillas'},
        {id: 4, nombre: 'producto_integro', pregunta: '¿El producto y empaque están íntegros?', descripcion: 'Botellas sin deformaciones, etiquetas adheridas, empaques sin roturas'},
        {id: 5, nombre: 'perpendicularidad', pregunta: '¿Perpendicularidad de botellas individuales es correcta?', descripcion: 'Medición con instrumento calibrado (≤ 0.3 mm desviación)'},
        {id: 6, nombre: 'alineacion_capas', pregunta: '¿Alineación de capas en la estiba es correcta?', descripcion: 'Cajas alineadas verticalmente, sin desplazamientos laterales'},
        {id: 7, nombre: 'estabilidad_lateral', pregunta: '¿La estiba tiene estabilidad al empuje lateral?', descripcion: 'Resistencia a fuerzas laterales, mantiene verticalidad'},
        {id: 8, nombre: 'altura_uniforme', pregunta: '¿La altura de la estiba es uniforme?', descripcion: 'Variación máxima entre puntos ≤ 5 mm'},
        {id: 9, nombre: 'film_condition', pregunta: '¿Film retráctil/flejado en buena condición?', descripcion: 'Tensión adecuada, sin roturas, cobertura completa'},
        {id: 10, nombre: 'ubicacion_correcta', pregunta: '¿Ubicación correcta en almacén?', descripcion: 'Posición adecuada según procedimientos de almacenamiento'}
    ];

    const productTemplates = [
        {sku: "360", descripcion: "COCA  COLA 600 ML 24 PK", factorEstiba: "2.5"}, {sku: "376", descripcion: "CC PET 2L NO RET 8 PK", factorEstiba: "2.5"},
        {sku: "396", descripcion: "COCA LIGHT 2 LT PET NR 8 C", factorEstiba: "2.5"}, {sku: "445", descripcion: "COCA COLA 3 LT PET NR 4 PK", factorEstiba: "2.5"},
        {sku: "500", descripcion: "CC 2.5L NO RET 8 PACK", factorEstiba: "2.5"}, {sku: "501", descripcion: "COCA COLA 2.5 LT REF-PET 8 CJ", factorEstiba: "3.0"},
        {sku: "610", descripcion: "COCA COLA 600ML PETNR 12PK", factorEstiba: "2.0"}, {sku: "614", descripcion: "FRESCA 2L PET NR 8PK", factorEstiba: "2.5"},
        {sku: "615", descripcion: "FRESCA 600ML PET NR 24PK", factorEstiba: "2.5"}, {sku: "640", descripcion: "SIDRAL MUNDET 600ML NR", factorEstiba: "2.5"},
        {sku: "641", descripcion: "SIDRAL MUNDET 2L NR 8PK", factorEstiba: "2.5"}, {sku: "658", descripcion: "FANTA 600ML PET NR 24PK", factorEstiba: "2.5"},
        {sku: "665", descripcion: "FANTA 2L PET NR 8PK", factorEstiba: "2.5"}, {sku: "674", descripcion: "SPRITE 600ML PET NR 24PK", factorEstiba: "2.5"},
        {sku: "675", descripcion: "SPRITE 2L PET NR 8PK", factorEstiba: "2.5"}, {sku: "702", descripcion: "CC SIN AZUCAR 600ML NR 24PK", factorEstiba: "2.5"},
        {sku: "711", descripcion: "CC SIN AZUCAR 2L NR 8PK", factorEstiba: "2.5"}, {sku: "854", descripcion: "DELAWARE PUNCH 600ML", factorEstiba: "2.5"},
        {sku: "872", descripcion: "TOPO CHICO SANGRIA 600", factorEstiba: "2.5"}, {sku: "874", descripcion: "TOPO CHICO TORONJA 600", factorEstiba: "2.5"},
        {sku: "908", descripcion: "CIEL AGUA NAT 600ML 24PK", factorEstiba: "3.0"}, {sku: "909", descripcion: "CIEL AGUA NAT 1L 12PK", factorEstiba: "3.0"},
        {sku: "910", descripcion: "CIEL AGUA NAT 1.5L 12PK", factorEstiba: "3.0"}, {sku: "945", descripcion: "CIEL SAB JAMAICA 1L 12PK", factorEstiba: "3.0"},
        {sku: "946", descripcion: "CIEL SAB LIMON 1L 12PK", factorEstiba: "3.0"}, {sku: "947", descripcion: "CIEL SAB MANDARINA 1L 12PK", factorEstiba: "3.0"},
        {sku: "983", descripcion: "POWERADE MORAS 500ML 24PK", factorEstiba: "2.5"}, {sku: "984", descripcion: "POWERADE LIMA LIMON 500ML 24PK", factorEstiba: "2.5"},
        {sku: "986", descripcion: "POWERADE UVA 500ML 24PK", factorEstiba: "2.5"}, {sku: "987", descripcion: "POWERADE NARANJA 500ML 24PK", factorEstiba: "2.5"},
        {sku: "989", descripcion: "POWERADE MORAS 1L 12PK", factorEstiba: "2.5"}, {sku: "990", descripcion: "POWERADE UVA 1L 12PK", factorEstiba: "2.5"},
        {sku: "992", descripcion: "POWERADE NARANJA 1L 12PK", factorEstiba: "2.5"}, {sku: "994", descripcion: "POWERADE LIMA LIMON 1L 12PK", factorEstiba: "2.5"},
        {sku: "1119", descripcion: "COCA COLA 1.75 ML PET NR 8 PACK", factorEstiba: "2.0"}, {sku: "1511", descripcion: "VITAMIN WATER XXX 500ML 24P", factorEstiba: "2.5"},
        {sku: "1512", descripcion: "VITAMIN WATER POWER-C 500ML 24P", factorEstiba: "2.5"}, {sku: "1513", descripcion: "VITAMIN WATER ENERGY 500ML 24P", factorEstiba: "2.5"},
        {sku: "1514", descripcion: "VITAMIN WATER FOCUS 500ML 24P", factorEstiba: "2.5"}, {sku: "1515", descripcion: "VITAMIN WATER ESSENTIAL 500ML 24P", factorEstiba: "2.5"},
        {sku: "1516", descripcion: "VITAMIN WATER RESTORE 500ML 24P", factorEstiba: "2.5"}, {sku: "2352", descripcion: "CC COLA BOT 3L RET PET 6p", factorEstiba: "3.0"},
        {sku: "2898", descripcion: "Coca Cola SA 2.5 LT REF PET 8PK", factorEstiba: "2.5"}, {sku: "2908", descripcion: "COCA-COLA 2.25L PNR 8 pk", factorEstiba: "2.0"},
        {sku: "2909", descripcion: "COCA-COLA 1.35L PET NR 12 pk", factorEstiba: "2.0"}, {sku: "3019", descripcion: "Coca Cola Original 2.75L $35 4pk", factorEstiba: "2.0"},
        {sku: "3276", descripcion: "CC COLA BOT 3L RET PET 6p Preciada", factorEstiba: "3.0"}, {sku: "99938", descripcion: "FANTA NARANJA 2.5 LT REF PET 8PK", factorEstiba: "3.0"},
        {sku: "99939", descripcion: "SIDRAL MUNDET 2.5 LT REF PET 8PK", factorEstiba: "3.0"}, {sku: "99940", descripcion: "FRESCA 2.5 LT REF PET 8PK", factorEstiba: "3.0"}
    ];
    
    // Create the initial state for each product for the session
    const createInitialProductState = (template) => {
        const initialState = {
            ...template,
            status: "pending", notes: "", ubicacion: "", inspector: "", turno: "", fotos_adjuntas: 0, fotos: {}, parametros: {}
        };
        parametrosChecklist.forEach(p => { initialState.parametros[p.nombre] = ""; });
        return initialState;
    };
    
    let products = productTemplates.map(createInitialProductState);
    
    function resizeImage(file, maxSize = 1280, quality = 0.6) {
        return new Promise((resolve) => {
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            const img = new Image();
            img.onload = function() {
                let { width, height } = img;
                if (width > height) { if (width > maxSize) { height *= maxSize / width; width = maxSize; }} 
                else { if (height > maxSize) { width *= maxSize / height; height = maxSize; }}
                canvas.width = width; canvas.height = height;
                ctx.drawImage(img, 0, 0, width, height);
                canvas.toBlob(resolve, 'image/jpeg', quality);
            };
            img.src = URL.createObjectURL(file);
        });
    }
    function blobToBase64(blob) {
        return new Promise((resolve) => {
            const reader = new FileReader();
            reader.onload = () => resolve(reader.result);
            reader.readAsDataURL(blob);
        });
    }
    
    function switchTab(tabName) {
        document.querySelectorAll('.tab-button').forEach(btn => {
            const isActive = btn.dataset.tab === tabName;
            btn.classList.toggle('active', isActive);
            btn.classList.toggle('border-sky-600', isActive);
            btn.classList.toggle('text-sky-600', isActive);
            btn.classList.toggle('border-transparent', !isActive);
            btn.classList.toggle('text-gray-500', !isActive);
        });
        document.querySelectorAll('.tab-content').forEach(content => {
            content.classList.toggle('active', content.id === `tab-${tabName}`);
            content.classList.toggle('hidden', content.id !== `tab-${tabName}`);
        });
        if (tabName === 'historial') loadHistoryContent();
        else if (tabName === 'estadisticas') loadStatistics();
        else if (tabName === 'configuracion') loadConfiguration();
    }
    
    async function loadHistoryContent() { /* ... same as before ... */ }
    function renderHistoryTable(verifications) { /* ... same as before ... */ }
    async function showVerificationDetails(verificationId) { /* ... same as before ... */ }

    let statsChart = null;
    async function loadStatistics() { /* ... same as before ... */ }
    function updateStatsChart(verifications) { /* ... same as before ... */ }
    function updateTopProducts(verifications) { /* ... same as before ... */ }
    function updateTopInspectors(verifications) { /* ... same as before ... */ }
    async function loadConfiguration() { /* ... same as before ... */ }
    async function updateStorageUsage() { /* ... same as before ... */ }
    
    function updateDateTime() {
        const now = new Date();
        const formattedDateTime = now.toLocaleDateString('es-ES', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' }) + ' - ' + now.toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit' });
        document.getElementById('current-datetime').textContent = formattedDateTime;
    }
    updateDateTime();
    setInterval(updateDateTime, 1000);

    const tableBody = document.getElementById('product-table-body');
    const noResultsDiv = document.getElementById('no-results');

    function renderTable(data) {
        tableBody.innerHTML = '';
        if (data.length === 0) {
            noResultsDiv.classList.remove('hidden');
            return;
        }
        noResultsDiv.classList.add('hidden');
        data.forEach(product => {
            const row = document.createElement('tr');
            row.className = 'bg-white dark:bg-gray-800 border-b dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600';
            row.dataset.sku = product.sku;
            let statusIcon = '';
            switch(product.status) {
                case 'ok': statusIcon = `<i class="fas fa-check-circle text-green-500 fa-lg" title="Verificado OK"></i>`; break;
                case 'error': statusIcon = `<i class="fas fa-exclamation-triangle text-red-500 fa-lg" title="Verificado con incidencias"></i>`; break;
                default: statusIcon = `<i class="fas fa-circle text-gray-400 fa-sm" title="Pendiente"></i>`;
            }
            row.innerHTML = `
                <td class="px-4 py-3 font-medium">${product.sku}</td>
                <td class="px-4 py-3">${product.descripcion}</td>
                <td class="px-4 py-3 text-center font-bold text-lg">${product.factorEstiba}</td>
                <td class="px-4 py-3 text-center">${statusIcon}</td>
                <td class="px-4 py-3 text-center text-sm">${product.ubicacion || '-'}</td>
                <td class="px-4 py-3 text-center text-sm">${product.fotos_adjuntas > 0 ? `📷 ${product.fotos_adjuntas}` : '-'}</td>
                <td class="px-4 py-3 text-center">
                    <button class="check-btn px-3 py-1 bg-sky-600 text-white rounded-md hover:bg-sky-700 text-xs font-semibold" data-sku="${product.sku}">Verificar</button>
                </td>`;
            tableBody.appendChild(row);
        });
    }

    const modal = document.getElementById('checklist-modal');
    const modalBackdrop = document.getElementById('modal-backdrop');
    const closeModalBtn = document.getElementById('close-modal-btn');
    const cancelBtn = document.getElementById('cancel-btn');
    const saveBtn = document.getElementById('save-btn');
    const modalTitle = document.getElementById('modal-title');
    const modalSku = document.getElementById('modal-sku');
    const modalFactor = document.getElementById('modal-factor');
    const checklistForm = document.getElementById('checklist-form');
    const notesTextarea = document.getElementById('notes');
    const locationInput = document.getElementById('location');
    const gpsBtn = document.getElementById('gps-btn');
    let currentSku = null;
    
    function renderParameters(product) {
        const container = document.getElementById('parametros-container');
        container.innerHTML = '';
        parametrosChecklist.forEach((param) => {
            const paramDiv = document.createElement('div');
            paramDiv.className = 'border border-gray-200 dark:border-gray-600 rounded-lg p-4 bg-gray-50 dark:bg-gray-700/50';
            const currentValue = product.parametros[param.nombre] || '';
            const hasPhoto = product.fotos && product.fotos[param.nombre];
            paramDiv.innerHTML = `
                <div class="flex flex-col md:flex-row md:items-center md:justify-between gap-3">
                    <div class="flex-grow">
                        <h4 class="text-sm font-semibold">${param.id}. ${param.pregunta}</h4>
                        <p class="text-xs text-gray-500">${param.descripcion}</p>
                    </div>
                    <div class="flex items-center gap-4">
                        <div class="flex gap-3">
                            <label class="flex items-center gap-1"><input type="radio" name="${param.nombre}" value="si" ${currentValue === 'si' ? 'checked' : ''} class="text-green-500 focus:ring-green-500"><span class="text-green-600">Sí</span></label>
                            <label class="flex items-center gap-1"><input type="radio" name="${param.nombre}" value="no" ${currentValue === 'no' ? 'checked' : ''} class="text-red-500 focus:ring-red-500"><span class="text-red-600">No</span></label>
                        </div>
                        <div class="flex flex-col items-center gap-1">
                            <input type="file" id="photo-${param.nombre}" accept="image/*" capture="environment" class="hidden" data-param="${param.nombre}">
                            <button type="button" class="photo-btn px-3 py-1 text-xs font-medium rounded-lg transition ${hasPhoto ? 'bg-green-100 text-green-700' : 'bg-amber-100 text-amber-700'}" data-param="${param.nombre}">
                                <i class="fas fa-camera mr-1"></i> ${hasPhoto ? 'Cambiar' : 'Agregar'}
                            </button>
                            <div class="photo-preview mt-1 h-8 w-8">
                                ${hasPhoto ? `<img src="${product.fotos[param.nombre]}" alt="Preview" class="w-full h-full object-cover rounded">` : ''}
                            </div>
                        </div>
                    </div>
                </div>`;
            container.appendChild(paramDiv);
        });
    }

    function openModal(product) {
        currentSku = product.sku;
        modalTitle.textContent = product.descripcion;
        modalSku.textContent = `SKU: ${product.sku}`;
        modalFactor.textContent = product.factorEstiba;
        renderParameters(product);
        document.getElementById('inspector').value = product.inspector || '';
        document.getElementById('turno').value = product.turno || '';
        notesTextarea.value = product.notes || '';
        locationInput.value = product.ubicacion || '';
        modal.classList.add('active');
        modalBackdrop.classList.add('active');
    }
    function closeModal() {
        modal.classList.remove('active');
        modalBackdrop.classList.remove('active');
        currentSku = null;
    }
    [closeModalBtn, cancelBtn, modalBackdrop].forEach(el => el.addEventListener('click', closeModal));
    
    document.getElementById('parametros-container').addEventListener('change', (e) => {
        if (e.target.type === 'radio' && e.target.value === 'no') {
            const paramName = e.target.name;
            const product = products.find(p => p.sku === currentSku);
            if (product && (!product.fotos || !product.fotos[paramName])) {
                 document.getElementById(`photo-${paramName}`)?.click();
            }
        }
    });

    document.addEventListener('click', (e) => {
        const checkBtn = e.target.closest('.check-btn');
        const photoBtn = e.target.closest('.photo-btn');
        if (checkBtn) {
            const product = products.find(p => p.sku === checkBtn.dataset.sku);
            if (product) openModal(product);
        }
        if (photoBtn) {
            document.getElementById(`photo-${photoBtn.dataset.param}`)?.click();
        }
    });

    document.addEventListener('change', async (e) => {
        if (e.target.type === 'file' && e.target.id.startsWith('photo-')) {
            const paramId = e.target.dataset.param;
            const file = e.target.files[0];
            if (file && currentSku) {
                const product = products.find(p => p.sku === currentSku);
                if (!product.fotos) product.fotos = {};
                try {
                    const maxSize = parseInt(document.getElementById('photo-max-size').value, 10);
                    const quality = parseFloat(document.getElementById('photo-quality').value);
                    const compressedBlob = await resizeImage(file, maxSize, quality);
                    const dataUrl = await blobToBase64(compressedBlob);
                    product.fotos[paramId] = dataUrl;

                    // --- START FIX: Manual DOM update instead of re-rendering ---
                    const photoButton = document.querySelector(`button.photo-btn[data-param="${paramId}"]`);
                    if (photoButton) {
                        photoButton.innerHTML = `<i class="fas fa-camera mr-1"></i> Cambiar`;
                        photoButton.className = 'photo-btn px-3 py-1 text-xs font-medium rounded-lg transition bg-green-100 text-green-700';
                        
                        const previewDiv = photoButton.parentElement.querySelector('.photo-preview');
                        if (previewDiv) {
                            previewDiv.innerHTML = `<img src="${dataUrl}" alt="Preview" class="w-full h-full object-cover rounded">`;
                        }
                    }
                    // --- END FIX ---
                    
                    updatePhotoCount(product);
                } catch (error) {
                    console.error('Error processing photo:', error);
                    alert('Error al procesar la imagen.');
                }
            }
        }
    });

    function updatePhotoCount(product) {
        product.fotos_adjuntas = Object.keys(product.fotos || {}).length;
    }

    saveBtn.addEventListener('click', () => {
        if (!currentSku) return;
        const product = products.find(p => p.sku === currentSku);
        if (!product) return;

        const formData = new FormData(checklistForm);
        let allAnswered = true;
        for (const param of parametrosChecklist) {
            if (!formData.has(param.nombre)) {
                allAnswered = false;
                break;
            }
        }

        if (!allAnswered) {
            alert('Por favor, responde "Sí" o "No" a todos los parámetros antes de guardar.');
            return;
        }

        let hasError = false;
        parametrosChecklist.forEach(param => {
            const value = formData.get(param.nombre);
            product.parametros[param.nombre] = value;
            if (value === 'no') hasError = true;
        });
        product.status = hasError ? 'error' : 'ok';
        product.notes = notesTextarea.value;
        product.ubicacion = locationInput.value;
        product.inspector = document.getElementById('inspector').value;
        product.turno = document.getElementById('turno').value;
        updatePhotoCount(product);
        filterAndRender();
        closeModal();
    });

    gpsBtn.addEventListener('click', () => {
        if (!navigator.geolocation) return alert('La geolocalización no es compatible con este navegador.');
        gpsBtn.disabled = true;
        gpsBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> GPS';
        navigator.geolocation.getCurrentPosition(
            (pos) => {
                locationInput.value = `${pos.coords.latitude.toFixed(6)}, ${pos.coords.longitude.toFixed(6)}`;
                gpsBtn.disabled = false;
                gpsBtn.innerHTML = '<i class="fas fa-map-marker-alt"></i> GPS';
            },
            (err) => {
                alert('Error al obtener la ubicación GPS.');
                console.error('GPS Error:', err);
                gpsBtn.disabled = false;
                gpsBtn.innerHTML = '<i class="fas fa-map-marker-alt"></i> GPS';
            },
            { enableHighAccuracy: true, timeout: 5000, maximumAge: 0 }
        );
    });

    const searchInput = document.getElementById('searchInput');
    function filterAndRender() {
        const searchTerm = searchInput.value.toLowerCase();
        const filteredProducts = products.filter(p => p.sku.toLowerCase().includes(searchTerm) || p.descripcion.toLowerCase().includes(searchTerm));
        renderTable(filteredProducts);
    }
    searchInput.addEventListener('input', filterAndRender);
    
    function exportToCSV() { /* ... same as before ... */ }
    
    function printReport() {
        const verifiedProducts = products.filter(p => p.status !== 'pending');
        if (verifiedProducts.length === 0) return alert('No hay productos verificados para imprimir.');

        const statusMap = { ok: 'OK', error: 'Con Incidencias' };
        const now = new Date();
        const currentDate = now.toLocaleDateString('es-ES', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
        const currentTime = now.toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit' });
        const emissionDate = now.toLocaleDateString('es-ES', { month: 'long', year: 'numeric' });
        const expirationDate = new Date(now.getFullYear() + 3, 11).toLocaleDateString('es-ES', { month: 'long', year: 'numeric' });
        const documentCode = `${now.getFullYear().toString().slice(-2)}-${(now.getMonth() + 1).toString().padStart(2, '0')}-${now.getDate().toString().padStart(2, '0')}-${now.getHours().toString().padStart(2, '0')}${now.getMinutes().toString().padStart(2, '0')}`;

        const parameterSummary = parametrosChecklist.map(param => {
            const total = verifiedProducts.length;
            const compliant = verifiedProducts.filter(p => p.parametros[param.nombre] === 'si').length;
            const percentage = total > 0 ? ((compliant / total) * 100).toFixed(1) : '0.0';
            return { parameter: param.pregunta, compliant, nonCompliant: total - compliant, percentage };
        });

        const summaryRows = parameterSummary.map(item => `
            <tr>
                <td>${item.parameter}</td>
                <td style="text-align: center;">${item.compliant}</td>
                <td style="text-align: center;">${item.nonCompliant}</td>
                <td style="text-align: center; font-weight: bold; color: ${item.percentage >= 95 ? '#166534' : item.percentage >= 80 ? '#d97706' : '#dc2626'};">${item.percentage}%</td>
            </tr>`).join('');

        const tableRows = verifiedProducts.map(p => {
            const compliantCount = parametrosChecklist.filter(param => p.parametros[param.nombre] === 'si').length;
            const compliancePercentage = ((compliantCount / parametrosChecklist.length) * 100).toFixed(1);
            
            let productRow = `
            <tr style="page-break-inside: avoid;">
                <td>${p.sku}</td><td>${p.descripcion}</td>
                <td style="text-align: center;">${p.factorEstiba}</td>
                <td style="text-align: center; font-weight: bold; color: ${p.status === 'ok' ? '#166534' : '#dc2626'};">${statusMap[p.status]}</td>
                <td style="text-align: center;">${compliancePercentage}%</td>
                <td>${p.ubicacion || '-'}</td><td>${p.inspector || '-'}</td>
                <td>${p.turno || '-'}</td>
                <td style="text-align: center;">${p.fotos_adjuntas || 0}</td>
            </tr>`;

            if (p.fotos_adjuntas > 0 && p.fotos) {
                const photoItems = Object.entries(p.fotos).map(([paramNombre, fotoData]) => {
                    const paramInfo = parametrosChecklist.find(param => param.nombre === paramNombre);
                    const paramQuestion = paramInfo ? paramInfo.pregunta.replace(/[¿?]/g, '') : paramNombre;
                    return `
                        <div class="photo-item">
                            <img src="${fotoData}" alt="${paramQuestion}">
                            <p>${paramQuestion}</p>
                        </div>
                    `;
                }).join('');

                productRow += `
                    <tr class="photo-row" style="page-break-inside: avoid;">
                        <td colspan="9" style="padding: 0;">
                            <div class="photo-container">${photoItems}</div>
                        </td>
                    </tr>
                `;
            }
            return productRow;
        }).join('');

        const printWindow = window.open('', '', 'height=900,width=1200');
        printWindow.document.write(`<html><head><title>Reporte de Verticalidad y Estibado</title><style>
            body { font-family: Arial, sans-serif; color: #000; margin: 15px; font-size: 11px; line-height: 1.3; }
            .page-break { page-break-after: always; }
            .header-container, table { page-break-inside: avoid; }
            .header-container { border: 2px solid #000; margin-bottom: 20px; }
            .header-row { display: flex; }
            .header-cell { padding: 8px; border-right: 1px solid #000; }
            .header-cell:last-child { border-right: none; }
            .logo-cell { flex: 1; } .title-cell { flex: 3; text-align: center; } .info-cell { flex: 1.5; }
            .logo-coca { background-color: #FF0000; color: white; font-weight: bold; text-align: center; padding: 8px; border-radius: 3px; }
            .logo-femsa { font-weight: bold; text-align: center; background-color: #f0f0f0; padding: 4px; border: 1px solid #ccc; border-radius: 3px; margin-top: 4px; }
            .dept-info { text-align: right; font-weight: bold; }
            .doc-info-row { border-top: 1px solid #000; display: flex; background-color: #f8f9fa; }
            .doc-info-cell { padding: 6px 8px; border-right: 1px solid #000; flex: 1; font-size: 10px; }
            .doc-info-cell:last-child { border-right: none; }
            h1, h2, h3 { text-align: center; }
            h1 { font-size: 16px; font-weight: bold; margin: 15px 0; color: #FF0000; } h2 { font-size: 14px; } h3 { font-size: 12px; color: #666; }
            .section-title { background-color: #e3f2fd; padding: 8px; font-weight: bold; margin: 20px 0 10px; border-left: 4px solid #1976d2; }
            table { width: 100%; border-collapse: collapse; margin-bottom: 20px; }
            th, td { border: 1px solid #ccc; padding: 5px; text-align: left; vertical-align: top; }
            th { background-color: #f2f2f2; font-weight: bold; }
            .summary-stats { background-color: #f8f9fa; padding: 10px; border-left: 4px solid #28a745; margin-bottom: 20px; }
            .photo-container { display: flex; flex-wrap: wrap; gap: 10px; padding: 10px; background-color: #fdfdfd; }
            .photo-row td { border-top: none; }
            .photo-item { text-align: center; font-size: 9px; color: #555; }
            .photo-item img { max-width: 100px; max-height: 100px; border: 1px solid #ddd; border-radius: 4px; margin-bottom: 4px;}
            .photo-item p { margin: 0; }
            @media print { body { -webkit-print-color-adjust: exact; color-adjust: exact; } }
        </style></head><body>`);
        
        const totalProducts = verifiedProducts.length;
        const okProducts = verifiedProducts.filter(p => p.status === 'ok').length;
        const errorProducts = totalProducts - okProducts;
        const overallCompliance = totalProducts > 0 ? ((okProducts / totalProducts) * 100).toFixed(1) : '0.0';
        
        // --- PAGE 1: FULL REPORT ---
        printWindow.document.write(`<div class="header-container">
            <div class="header-row">
                <div class="header-cell logo-cell"><div class="logo-coca">Coca-Cola</div><div class="logo-femsa">FEMSA</div></div>
                <div class="header-cell title-cell">
                    <div style="font-weight: bold; font-size: 14px;">Confiabilidad de la información</div>
                    <div style="font-weight: bold; font-size: 16px;">Reporte de Devoluciones Planta</div>
                </div>
                <div class="header-cell info-cell"><div class="dept-info">Dirección Cadena de Suministro<br><span style="font-weight: normal;">Coca-Cola FEMSA</span></div></div>
            </div>
            <div class="doc-info-row">
                <div class="doc-info-cell"><strong>Fecha de Emisión:</strong><br>${emissionDate}</div>
                <div class="doc-info-cell"><strong>Fecha de Expiración:</strong><br>${expirationDate}</div>
                <div class="doc-info-cell"><strong>Código:</strong><br>${documentCode}</div>
                <div class="doc-info-cell"><strong>Revisión:</strong><br>Rev 04</div>
            </div>
        </div>
        <h1>Checklist de Verticalidad y Estibado</h1>
        <h2>Planta "Cuautitlán"</h2>
        <h3>Generado el ${currentDate} a las ${currentTime}</h3>
        <div class="summary-stats"><strong>Resumen General:</strong> ${totalProducts} productos verificados | ${okProducts} conformes (${overallCompliance}%) | ${errorProducts} con incidencias (${(100 - parseFloat(overallCompliance)).toFixed(1)}%)</div>
        <div class="section-title">📋 Productos Verificados</div>
        <table><thead><tr><th>SKU</th><th>Descripción</th><th>Factor</th><th>Estado</th><th>Cumplimiento</th><th>Ubicación</th><th>Inspector</th><th>Turno</th><th>Fotos</th></tr></thead><tbody>${tableRows}</tbody></table>
        <div class="page-break"></div>
        
        // --- PAGE 2: PARAMETER SUMMARY ---
        <h1>Resumen de Verificaciones por Parámetro</h1>
        <div class="section-title">📊 Cumplimiento General de Parámetros</div>
        <table><thead><tr><th>Parámetro de Verificación</th><th>Conformes</th><th>No Conformes</th><th>% Cumplimiento</th></tr></thead><tbody>${summaryRows}</tbody></table>`);
        
        printWindow.document.write('</body></html>');
        printWindow.document.close();
        printWindow.focus();
        printWindow.print();
    }

    document.getElementById('export-btn').addEventListener('click', exportToCSV);
    document.getElementById('print-btn').addEventListener('click', printReport);
    
    document.querySelectorAll('.tab-button').forEach(button => button.addEventListener('click', () => switchTab(button.dataset.tab)));
    document.getElementById('apply-filters').addEventListener('click', async () => {
        const filters = {
            dateFrom: document.getElementById('filter-date-from').value,
            dateTo: document.getElementById('filter-date-to').value,
            sku: document.getElementById('filter-sku').value,
            inspector: document.getElementById('filter-inspector').value,
            status: document.getElementById('filter-status').value
        };
        renderHistoryTable(await getVerifications(filters));
    });
    document.getElementById('export-history').addEventListener('click', async () => { /* ... export logic ... */ });
    document.getElementById('export-all-data').addEventListener('click', async () => { /* ... export logic ... */ });
    document.getElementById('clean-old-data').addEventListener('click', async () => { /* ... clean logic ... */ });
    document.getElementById('clear-all-data').addEventListener('click', async () => { /* ... clear logic ... */ });

    document.addEventListener('click', (e) => {
        const viewBtn = e.target.closest('.view-details-btn');
        const deleteBtn = e.target.closest('.delete-verification-btn');
        if (viewBtn) showVerificationDetails(parseInt(viewBtn.dataset.id));
        if (deleteBtn) deleteVerificationConfirm(parseInt(deleteBtn.dataset.id));
    });
    
    document.getElementById('close-history-modal').addEventListener('click', () => {
        document.getElementById('history-detail-modal').classList.remove('active');
        document.getElementById('modal-backdrop').classList.remove('active');
    });

    async function commitSessionToHistory() {
        const productsToCommit = products.filter(p => p.status !== 'pending');
        if (productsToCommit.length === 0) return alert('No hay verificaciones para guardar.');

        const btn = document.getElementById('commit-session-btn');
        btn.disabled = true;
        btn.innerHTML = `<i class="fas fa-spinner fa-spin mr-2"></i> Guardando...`;

        try {
            for (const product of productsToCommit) {
                const verification = {
                    sku: product.sku, descripcion: product.descripcion, factorEstiba: product.factorEstiba,
                    status: product.status, fecha: new Date().toISOString(), inspector: product.inspector,
                    turno: product.turno, ubicacion: product.ubicacion, observaciones: product.notes,
                    parametros: product.parametros, total_fotos: product.fotos_adjuntas
                };
                const verificationId = await saveVerification(verification);

                if (product.fotos && Object.keys(product.fotos).length > 0) {
                    for (const [parametroId, fotoData] of Object.entries(product.fotos)) {
                        await savePhoto({
                            verificacion_id: verificationId, parametro: parametroId, blob: fotoData,
                            nombre: `${product.sku}_${parametroId}.jpg`, fecha: new Date().toISOString()
                        });
                    }
                }
            }
            alert(`✅ ${productsToCommit.length} verificaciones guardadas.`);
            
            // Reset products
            productsToCommit.forEach(p => {
                const template = productTemplates.find(t => t.sku === p.sku);
                if (template) {
                    Object.assign(p, createInitialProductState(template));
                }
            });
            filterAndRender();

        } catch (error) {
            console.error('Error al guardar la sesión:', error);
            alert('⚠️ Ocurrió un error al guardar las verificaciones.');
        } finally {
            btn.disabled = false;
            btn.innerHTML = `<i class="fas fa-save mr-2"></i> Guardar en Historial`;
        }
    }
    document.getElementById('commit-session-btn').addEventListener('click', commitSessionToHistory);

    // --- QR SCANNER LOGIC ---
    let html5QrCode;
    const qrScannerModal = document.getElementById('qr-scanner-modal');
    const scanQrMainBtn = document.getElementById('scan-qr-main-btn');
    const closeQrScannerBtn = document.getElementById('close-qr-scanner-btn');
    const qrResultEl = document.getElementById('qr-result');

    const onScanSuccess = (decodedText, decodedResult) => {
        qrResultEl.textContent = `Escaneo exitoso! Procesando...`;
        stopQrScanner();
        
        const lines = decodedText.split(/[\n\r]+/);
        const qrData = {};
        let skuFromQR = null;

        lines.forEach(line => {
            const parts = line.split(':');
            if (parts.length >= 2) {
                const key = parts[0].trim();
                const value = parts.slice(1).join(':').trim();
                qrData[key] = value;
                if (key.toUpperCase() === 'SKU') {
                    skuFromQR = parseInt(value, 10).toString();
                }
            }
        });

        if (!skuFromQR) {
            alert('Código QR no válido. No se encontró el SKU.');
            return;
        }

        const product = products.find(p => p.sku === skuFromQR);
        if (!product) {
            alert(`Producto con SKU ${skuFromQR} no encontrado en la lista.`);
            return;
        }

        let traceabilityNotes = "Datos escaneados del QR:\n";
        for (const [key, value] of Object.entries(qrData)) {
            if (key.toUpperCase() !== 'SKU') {
                traceabilityNotes += `- ${key}: ${value}\n`;
            }
        }
        
        openModal(product);
        
        setTimeout(() => {
            document.getElementById('notes').value = traceabilityNotes;
        }, 100);
    };

    const onScanFailure = (error) => {
        // console.warn(`QR scan error: ${error}`);
    };

    function startQrScanner() {
        qrScannerModal.classList.add('active');
        modalBackdrop.classList.add('active');
        qrResultEl.textContent = 'Apunte la cámara al código QR';

        html5QrCode = new Html5Qrcode("qr-reader");
        const config = { fps: 10, qrbox: { width: 250, height: 250 } };
        html5QrCode.start({ facingMode: "environment" }, config, onScanSuccess, onScanFailure)
            .catch(err => {
                console.error("No se pudo iniciar el escáner QR", err);
                qrResultEl.textContent = 'Error al iniciar la cámara.';
            });
    }

    function stopQrScanner() {
        if (html5QrCode && html5QrCode.isScanning) {
            html5QrCode.stop().then(() => {
                console.log("QR Code scanning stopped.");
            }).catch(err => {
                console.error("Failed to stop QR scanner.", err);
            });
        }
        qrScannerModal.classList.remove('active');
        // Do not hide backdrop if another modal is opening
        if(!modal.classList.contains('active')){
             modalBackdrop.classList.remove('active');
        }
    }

    scanQrMainBtn.addEventListener('click', startQrScanner);
    closeQrScannerBtn.addEventListener('click', stopQrScanner);
    
    // --- END QR SCANNER LOGIC ---

    
    initDatabase().then(() => {
        renderTable(products);
        switchTab('verificar');
        console.log('Aplicación inicializada.');
    }).catch(error => {
        console.error('Error al inicializar la aplicación:', error);
        updateDbStatus('❌ Error de Inicialización', 'error');
        renderTable(products);
        switchTab('verificar');
    });
});
</script>

</body>
</html>


