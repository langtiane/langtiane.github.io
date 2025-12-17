# langtiane.github.io
分享一些物理学习资源以及工具
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>重力模拟环境 - 多物体增强版</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #2a3c7f);
            color: #fff;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
            gap: 25px;
        }
        
        header {
            text-align: center;
            padding: 20px 0;
            border-bottom: 2px solid rgba(255, 255, 255, 0.1);
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 10px;
            background: linear-gradient(90deg, #ff7e5f, #feb47b);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        
        .subtitle {
            font-size: 1.2rem;
            color: #a0b3ff;
            max-width: 800px;
            margin: 0 auto;
            line-height: 1.6;
        }
        
        .main-content {
            display: flex;
            flex-wrap: wrap;
            gap: 25px;
        }
        
        .control-panel {
            flex: 1;
            min-width: 320px;
            background: rgba(0, 10, 40, 0.7);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .simulation-area {
            flex: 2;
            min-width: 500px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
            position: relative;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .panel-title {
            font-size: 1.5rem;
            margin-bottom: 25px;
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            color: #6ee7ff;
        }
        
        .control-group {
            margin-bottom: 25px;
        }
        
        .control-title {
            font-size: 1.2rem;
            margin-bottom: 15px;
            color: #a0b3ff;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .control-title i {
            font-size: 1.4rem;
        }
        
        .gravity-options {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-bottom: 20px;
        }
        
        .gravity-btn {
            flex: 1;
            min-width: 120px;
            padding: 14px 10px;
            background: rgba(30, 40, 80, 0.7);
            border: none;
            border-radius: 10px;
            color: white;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .gravity-btn:hover {
            background: rgba(50, 70, 150, 0.8);
            transform: translateY(-3px);
        }
        
        .gravity-btn.active {
            background: linear-gradient(135deg, #3a7bd5, #00d2ff);
            box-shadow: 0 5px 15px rgba(58, 123, 213, 0.4);
        }
        
        .gravity-btn .value {
            font-size: 1.2rem;
            font-weight: bold;
            margin-top: 5px;
        }
        
        .slider-container {
            margin-top: 20px;
        }
        
        .slider-label {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        
        .slider-value {
            font-weight: bold;
            color: #6ee7ff;
        }
        
        input[type="range"] {
            width: 100%;
            height: 10px;
            -webkit-appearance: none;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 5px;
            outline: none;
        }
        
        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 22px;
            height: 22px;
            border-radius: 50%;
            background: #00d2ff;
            cursor: pointer;
            box-shadow: 0 0 10px rgba(0, 210, 255, 0.7);
        }
        
        .object-controls {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        
        .control-btn {
            flex: 1;
            padding: 12px;
            background: linear-gradient(135deg, #3a7bd5, #00d2ff);
            border: none;
            border-radius: 10px;
            color: white;
            font-size: 0.95rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            min-width: 120px;
        }
        
        .control-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 7px 15px rgba(58, 123, 213, 0.4);
        }
        
        .control-btn.remove {
            background: linear-gradient(135deg, #ff416c, #ff4b2b);
        }
        
        .control-btn:disabled {
            background: #555;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .control-btn.square {
            background: linear-gradient(135deg, #4CAF50, #8BC34A);
        }
        
        .control-btn.triangle {
            background: linear-gradient(135deg, #FF9800, #FF5722);
        }
        
        .object-counter {
            text-align: center;
            font-size: 1.1rem;
            margin-bottom: 20px;
            padding: 10px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 8px;
        }
        
        .object-counter span {
            font-weight: bold;
            color: #6ee7ff;
            font-size: 1.3rem;
        }
        
        .object-list {
            max-height: 250px;
            overflow-y: auto;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 8px;
            padding: 15px;
        }
        
        .object-item {
            display: flex;
            justify-content: space-between;
            padding: 12px;
            margin-bottom: 10px;
            background: rgba(30, 40, 80, 0.5);
            border-radius: 8px;
            border-left: 4px solid #00d2ff;
        }
        
        .object-item.square {
            border-left: 4px solid #4CAF50;
        }
        
        .object-item.triangle {
            border-left: 4px solid #FF9800;
        }
        
        .object-item:last-child {
            margin-bottom: 0;
        }
        
        .object-info {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }
        
        .object-icon {
            display: inline-block;
            margin-right: 8px;
            vertical-align: middle;
        }
        
        .object-id {
            font-weight: bold;
            font-size: 1.1rem;
        }
        
        .object-details {
            font-size: 0.9rem;
            color: #ccc;
        }
        
        .instructions {
            margin-top: 30px;
            padding: 20px;
            background: rgba(0, 20, 60, 0.5);
            border-radius: 10px;
            font-size: 0.95rem;
            line-height: 1.6;
        }
        
        .instructions h3 {
            margin-bottom: 10px;
            color: #6ee7ff;
        }
        
        .instructions ul {
            padding-left: 20px;
        }
        
        .instructions li {
            margin-bottom: 8px;
        }
        
        canvas {
            display: block;
            background: #0a0f2e;
        }
        
        .stats {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(0, 10, 40, 0.8);
            padding: 15px;
            border-radius: 10px;
            font-size: 1rem;
            border: 1px solid rgba(255, 255, 255, 0.1);
            min-width: 200px;
        }
        
        .stats-title {
            font-size: 1.2rem;
            margin-bottom: 10px;
            color: #6ee7ff;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            padding-bottom: 5px;
        }
        
        .stat-item {
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
        }
        
        .stat-value {
            font-weight: bold;
            color: #a0ffb3;
        }
        
        .checkbox-container {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .checkbox-container input[type="checkbox"] {
            margin-right: 10px;
            width: 18px;
            height: 18px;
        }
        
        .checkbox-container label {
            font-size: 1rem;
            cursor: pointer;
        }
        
        .velocity-control {
            margin-top: 20px;
            padding: 15px;
            background: rgba(0, 20, 60, 0.5);
            border-radius: 10px;
            display: none;
        }
        
        .velocity-title {
            font-size: 1.2rem;
            margin-bottom: 10px;
            color: #ffcc00;
        }
        
        .velocity-inputs {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .velocity-input-group {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .velocity-input-group label {
            font-size: 0.95rem;
        }
        
        .velocity-input-group input {
            width: 100px;
            padding: 5px 10px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 5px;
            color: white;
            text-align: center;
        }
        
        .velocity-apply-btn {
            margin-top: 10px;
            padding: 10px;
            background: linear-gradient(135deg, #ff9800, #ff5722);
            border: none;
            border-radius: 5px;
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .velocity-apply-btn:hover {
            background: linear-gradient(135deg, #ff5722, #ff9800);
        }
        
        footer {
            text-align: center;
            margin-top: 20px;
            padding-top: 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            color: #a0b3ff;
            font-size: 0.9rem;
        }
        
        @media (max-width: 1100px) {
            .main-content {
                flex-direction: column;
            }
            
            .control-panel, .simulation-area {
                min-width: 100%;
            }
        }
        
        @media (max-width: 600px) {
            h1 {
                font-size: 2rem;
            }
            
            .control-panel {
                padding: 20px;
            }
            
            .gravity-btn {
                min-width: 100px;
                padding: 12px 8px;
            }
            
            .control-btn {
                min-width: 100px;
                font-size: 0.85rem;
            }
        }
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <div class="container">
        <header>
            <h1><i class="fas fa-globe-americas"></i> 重力模拟环境 - 多物体增强版</h1>
            <p class="subtitle">模拟不同重力环境下小球的运动。新增正方形物块和三角形斜面，可调整大小、倾角和摩擦系数。</p>
        </header>
        
        <div class="main-content">
            <div class="control-panel">
                <h2 class="panel-title"><i class="fas fa-sliders-h"></i> 控制面板</h2>
                
                <div class="control-group">
                    <h3 class="control-title"><i class="fas fa-weight-hanging"></i> 重力设置</h3>
                    <div class="gravity-options">
                        <button class="gravity-btn active" data-gravity="9.8">
                            <i class="fas fa-earth-americas"></i> 地球
                            <span class="value">9.8 m/s²</span>
                        </button>
                        <button class="gravity-btn" data-gravity="1.6">
                            <i class="fas fa-moon"></i> 月球
                            <span class="value">1.6 m/s²</span>
                        </button>
                        <button class="gravity-btn" data-gravity="0">
                            <i class="fas fa-infinity"></i> 完全失重
                            <span class="value">0 m/s²</span>
                        </button>
                    </div>
                    
                    <div class="slider-container">
                        <div class="slider-label">
                            <span>自定义重力加速度</span>
                            <span class="slider-value" id="gravity-value">9.8 m/s²</span>
                        </div>
                        <input type="range" id="gravity-slider" min="0" max="20" step="0.1" value="9.8">
                    </div>
                </div>
                
                <div class="control-group">
                    <h3 class="control-title"><i class="fas fa-basketball-ball"></i> 小球设置</h3>
                    
                    <div class="slider-container">
                        <div class="slider-label">
                            <span>小球半径</span>
                            <span class="slider-value" id="ball-radius-value">30 像素</span>
                        </div>
                        <input type="range" id="ball-radius-slider" min="10" max="60" step="2" value="30">
                    </div>
                    
                    <div class="slider-container">
                        <div class="slider-label">
                            <span>碰撞弹性</span>
                            <span class="slider-value" id="elasticity-value">0.8</span>
                        </div>
                        <input type="range" id="elasticity-slider" min="0.1" max="1" step="0.1" value="0.8">
                    </div>
                    
                    <div class="object-controls">
                        <button class="control-btn" id="add-ball">
                            <i class="fas fa-plus-circle"></i> 添加小球
                        </button>
                        <button class="control-btn square" id="add-square">
                            <i class="fas fa-square"></i> 添加正方形
                        </button>
                        <button class="control-btn triangle" id="add-triangle">
                            <i class="fas fa-shapes"></i> 添加斜面
                        </button>
                        <button class="control-btn remove" id="remove-object" disabled>
                            <i class="fas fa-minus-circle"></i> 移除物体
                        </button>
                    </div>
                    
                    <div class="object-counter">
                        当前物体数量: <span id="object-count">0</span>
                    </div>
                    
                    <div class="object-list" id="object-list">
                        <!-- 物体列表将在这里动态生成 -->
                        <div class="empty-list">暂无物体，点击上方按钮添加</div>
                    </div>
                </div>
                
                <div class="control-group">
                    <h3 class="control-title"><i class="fas fa-wind"></i> 阻力与摩擦设置</h3>
                    
                    <div class="slider-container">
                        <div class="slider-label">
                            <span>空气阻力系数</span>
                            <span class="slider-value" id="drag-value">0.01</span>
                        </div>
                        <input type="range" id="drag-slider" min="0" max="0.5" step="0.01" value="0.01">
                    </div>
                    
                    <div class="slider-container">
                        <div class="slider-label">
                            <span>斜面摩擦系数</span>
                            <span class="slider-value" id="friction-value">0.1</span>
                        </div>
                        <input type="range" id="friction-slider" min="0" max="1" step="0.05" value="0.1">
                    </div>
                    
                    <div class="checkbox-container">
                        <input type="checkbox" id="show-forces">
                        <label for="show-forces">显示受力箭头 (红色: 重力, 蓝色: 阻力)</label>
                    </div>
                    
                    <div class="slider-container" id="arrow-scale-container" style="display: none;">
                        <div class="slider-label">
                            <span>箭头缩放因子</span>
                            <span class="slider-value" id="arrow-scale-value">0.1</span>
                        </div>
                        <input type="range" id="arrow-scale-slider" min="0.01" max="1.0" step="0.01" value="0.1">
                    </div>
                </div>
                
                <div class="control-group">
                    <h3 class="control-title"><i class="fas fa-tachometer-alt"></i> 模拟设置</h3>
                    
                    <div class="slider-container">
                        <div class="slider-label">
                            <span>模拟倍速 (0-1倍)</span>
                            <span class="slider-value" id="speed-value">1.0</span>
                        </div>
                        <input type="range" id="speed-slider" min="0" max="1" step="0.1" value="1.0">
                    </div>
                    
                    <p style="margin-top: 10px; font-size: 0.9rem; color: #ccc; line-height: 1.4;">
                        提示: 使用鼠标右键点击物体可以设置初始速度
                    </p>
                </div>
                
                <div class="velocity-control" id="velocity-control">
                    <div class="velocity-title">
                        <i class="fas fa-bullseye"></i> 设置物体初速度
                    </div>
                    <div class="velocity-inputs">
                        <div class="velocity-input-group">
                            <label for="velocity-magnitude">速度大小 (像素/秒):</label>
                            <input type="number" id="velocity-magnitude" min="0" max="500" value="100">
                        </div>
                        <div class="velocity-input-group">
                            <label for="velocity-angle">角度 (度, 0=右, 90=下):</label>
                            <input type="number" id="velocity-angle" min="0" max="360" value="45">
                        </div>
                        <div class="velocity-input-group">
                            <label>目标物体:</label>
                            <span id="velocity-object-id" style="font-weight: bold; color: #ffcc00;">#0</span>
                        </div>
                    </div>
                    <button class="velocity-apply-btn" id="apply-velocity">应用速度</button>
                </div>
                
                <div class="instructions">
                    <h3><i class="fas fa-info-circle"></i> 使用说明</h3>
                    <ul>
                        <li><strong>选择重力环境</strong>：点击地球、月球或完全失重按钮切换预设重力</li>
                        <li><strong>自定义重力</strong>：使用滑块调整自定义重力加速度</li>
                        <li><strong>添加物体</strong>：可以添加小球、正方形和三角形斜面</li>
                        <li><strong>调整物体大小</strong>：小球半径、正方形边长和斜面大小可调</li>
                        <li><strong>斜面控制</strong>：可以调整斜面倾角(0-180度)和摩擦系数</li>
                        <li><strong>碰撞弹性</strong>：调整物体碰撞后的弹性系数（0.1-1.0）</li>
                        <li><strong>空气阻力</strong>：调整空气阻力系数，影响物体运动</li>
                        <li><strong>受力箭头</strong>：显示物体受到的力（红色:重力, 蓝色:阻力）</li>
                        <li><strong>箭头缩放</strong>：调整箭头显示的大小，便于观察</li>
                        <li><strong>模拟倍速</strong>：调整模拟速度（0-1倍），方便观察</li>
                        <li><strong>设定初速度</strong>：右键点击物体，设置初始速度大小和方向</li>
                        <li><strong>碰撞体积</strong>：物体之间会发生碰撞，根据弹性系数反弹</li>
                    </ul>
                </div>
            </div>
            
            <div class="simulation-area">
                <canvas id="simulation-canvas"></canvas>
                <div class="stats">
                    <div class="stats-title"><i class="fas fa-chart-line"></i> 模拟状态</div>
                    <div class="stat-item">
                        <span>当前重力加速度:</span>
                        <span class="stat-value" id="current-gravity">9.8 m/s²</span>
                    </div>
                    <div class="stat-item">
                        <span>空气阻力系数:</span>
                        <span class="stat-value" id="current-drag">0.01</span>
                    </div>
                    <div class="stat-item">
                        <span>斜面摩擦系数:</span>
                        <span class="stat-value" id="current-friction">0.1</span>
                    </div>
                    <div class="stat-item">
                        <span>模拟倍速:</span>
                        <span class="stat-value" id="current-speed">1.0</span>
                    </div>
                    <div class="stat-item">
                        <span>物体总数:</span>
                        <span class="stat-value" id="sim-object-count">0</span>
                    </div>
                    <div class="stat-item">
                        <span>被抓住的物体:</span>
                        <span class="stat-value" id="grabbed-object">无</span>
                    </div>
                    <div class="stat-item">
                        <span>箭头缩放因子:</span>
                        <span class="stat-value" id="current-arrow-scale">0.1</span>
                    </div>
                </div>
            </div>
        </div>
        
        <footer>
            <p>重力模拟环境 - 多物体增强版 &copy; 2023 | 使用 HTML5 Canvas 和 JavaScript 实现 | 物理引擎模拟</p>
        </footer>
    </div>

    <script>
        // 获取DOM元素
        const canvas = document.getElementById('simulation-canvas');
        const ctx = canvas.getContext('2d');
        const gravityValueElement = document.getElementById('gravity-value');
        const gravitySlider = document.getElementById('gravity-slider');
        const ballRadiusValueElement = document.getElementById('ball-radius-value');
        const ballRadiusSlider = document.getElementById('ball-radius-slider');
        const elasticityValueElement = document.getElementById('elasticity-value');
        const elasticitySlider = document.getElementById('elasticity-slider');
        const dragValueElement = document.getElementById('drag-value');
        const dragSlider = document.getElementById('drag-slider');
        const frictionValueElement = document.getElementById('friction-value');
        const frictionSlider = document.getElementById('friction-slider');
        const speedValueElement = document.getElementById('speed-value');
        const speedSlider = document.getElementById('speed-slider');
        const arrowScaleValueElement = document.getElementById('arrow-scale-value');
        const arrowScaleSlider = document.getElementById('arrow-scale-slider');
        const arrowScaleContainer = document.getElementById('arrow-scale-container');
        const currentGravityElement = document.getElementById('current-gravity');
        const currentDragElement = document.getElementById('current-drag');
        const currentFrictionElement = document.getElementById('current-friction');
        const currentSpeedElement = document.getElementById('current-speed');
        const currentArrowScaleElement = document.getElementById('current-arrow-scale');
        const objectCountElement = document.getElementById('object-count');
        const simObjectCountElement = document.getElementById('sim-object-count');
        const grabbedObjectElement = document.getElementById('grabbed-object');
        const addBallButton = document.getElementById('add-ball');
        const addSquareButton = document.getElementById('add-square');
        const addTriangleButton = document.getElementById('add-triangle');
        const removeObjectButton = document.getElementById('remove-object');
        const objectListElement = document.getElementById('object-list');
        const gravityButtons = document.querySelectorAll('.gravity-btn');
        const showForcesCheckbox = document.getElementById('show-forces');
        const velocityControl = document.getElementById('velocity-control');
        const velocityMagnitudeInput = document.getElementById('velocity-magnitude');
        const velocityAngleInput = document.getElementById('velocity-angle');
        const velocityObjectIdElement = document.getElementById('velocity-object-id');
        const applyVelocityButton = document.getElementById('apply-velocity');
        
        // 设置画布尺寸
        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }
        
        // 初始化画布尺寸
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);
        
        // 模拟参数
        let gravity = 9.8; // 重力加速度 (m/s²)
        let ballRadius = 30; // 小球半径 (像素)
        let elasticity = 0.8; // 碰撞弹性系数
        let dragCoefficient = 0.01; // 空气阻力系数
        let frictionCoefficient = 0.1; // 斜面摩擦系数
        let timeScale = 1.0; // 时间倍速 (0-1)
        let showForces = false; // 是否显示受力箭头（默认不显示）
        let arrowScale = 0.1; // 箭头缩放因子
        let objects = []; // 存储所有物体
        let objectIdCounter = 0; // 物体ID计数器
        let grabbedObject = null; // 当前被抓住的物体
        let isDragging = false; // 是否正在拖拽
        let lastTime = 0; // 用于计算时间间隔
        let animationId = null; // 动画ID
        let velocityTargetObject = null; // 设置速度的目标物体
        
        // 物体类型常量
        const OBJECT_TYPES = {
            BALL: 'ball',
            SQUARE: 'square',
            TRIANGLE: 'triangle'
        };
        
        // 物理物体基类
        class PhysicsObject {
            constructor(id, type, x, y) {
                this.id = id;
                this.type = type;
                this.x = x;
                this.y = y;
                this.vx = 0;
                this.vy = 0;
                this.mass = 1;
                this.color = '#FFFFFF';
                this.isGrabbed = false;
                this.gravityForce = 0;
                this.dragForce = { x: 0, y: 0 };
                this.trail = [];
                this.trailLength = 15;
                this.rotation = 0; // 旋转角度（弧度）
            }
            
            // 更新物体状态
            update(deltaTime, gravity, dragCoefficient, allObjects) {
                // 应用时间倍速
                deltaTime *= timeScale;
                
                // 如果不是被抓住的状态，则应用物理
                if (!this.isGrabbed) {
                    // 计算重力加速度（转换为像素/秒²）
                    const pixelGravity = gravity * 50;
                    
                    // 计算空气阻力 (与速度成正比)
                    const speed = Math.sqrt(this.vx * this.vx + this.vy * this.vy);
                    if (speed > 0) {
                        this.dragForce.x = -dragCoefficient * this.vx * this.mass;
                        this.dragForce.y = -dragCoefficient * this.vy * this.mass;
                    } else {
                        this.dragForce.x = 0;
                        this.dragForce.y = 0;
                    }
                    
                    // 计算净加速度 (重力 + 阻力)
                    const ax = this.dragForce.x / this.mass;
                    const ay = pixelGravity + (this.dragForce.y / this.mass);
                    
                    // 应用加速度到速度
                    this.vx += ax * deltaTime;
                    this.vy += ay * deltaTime;
                    
                    // 更新位置
                    this.x += this.vx * deltaTime * 60;
                    this.y += this.vy * deltaTime * 60;
                    
                    // 边界碰撞检测
                    this.handleBoundaryCollision();
                    
                    // 与其他物体的碰撞检测
                    this.handleObjectCollisions(allObjects);
                    
                    // 添加轨迹点
                    this.trail.push({x: this.x, y: this.y});
                    if (this.trail.length > this.trailLength) {
                        this.trail.shift();
                    }
                } else {
                    // 如果物体被抓住，重置阻力
                    this.dragForce.x = 0;
                    this.dragForce.y = 0;
                }
                
                // 更新重力大小
                this.gravityForce = this.mass * gravity;
            }
            
            // 处理边界碰撞（需要在子类中实现）
            handleBoundaryCollision() {
                // 子类需要实现具体的边界碰撞逻辑
            }
            
            // 处理与其他物体的碰撞
            handleObjectCollisions(allObjects) {
                for (const otherObject of allObjects) {
                    // 跳过自己、已抓住的物体、斜面
                    if (otherObject === this || otherObject.isGrabbed || otherObject.type === OBJECT_TYPES.TRIANGLE) {
                        continue;
                    }
                    
                    // 检测碰撞并处理
                    if (this.isCollidingWith(otherObject)) {
                        this.resolveCollision(otherObject);
                    }
                }
            }
            
            // 检测是否与另一个物体碰撞（需要在子类中实现）
            isCollidingWith(otherObject) {
                return false;
            }
            
            // 解决碰撞（需要在子类中实现）
            resolveCollision(otherObject) {
                // 子类需要实现具体的碰撞解决逻辑
            }
            
            // 绘制物体（需要在子类中实现）
            draw() {
                // 子类需要实现具体的绘制逻辑
            }
            
            // 检查点是否在物体内部（需要在子类中实现）
            containsPoint(x, y) {
                return false;
            }
            
            // 设置初始速度
            setVelocity(magnitude, angleDegrees) {
                const angleRadians = angleDegrees * Math.PI / 180;
                this.vx = magnitude * Math.cos(angleRadians);
                this.vy = magnitude * Math.sin(angleRadians);
            }
            
            // 绘制力向量
            drawForceVectors() {
                // 绘制重力向量 (红色)
                const gravityLength = this.gravityForce * arrowScale;
                
                // 只有当重力长度大于最小阈值时才绘制
                if (gravityLength > 0.5) {
                    ctx.beginPath();
                    ctx.moveTo(this.x, this.y);
                    ctx.lineTo(this.x, this.y + gravityLength);
                    ctx.strokeStyle = '#FF5252'; // 红色表示重力
                    ctx.lineWidth = 3;
                    ctx.stroke();
                    
                    // 绘制重力箭头头部
                    const arrowSize = Math.max(4, Math.min(12, gravityLength * 0.2));
                    ctx.beginPath();
                    ctx.moveTo(this.x, this.y + gravityLength);
                    ctx.lineTo(this.x - arrowSize, this.y + gravityLength - arrowSize);
                    ctx.lineTo(this.x + arrowSize, this.y + gravityLength - arrowSize);
                    ctx.closePath();
                    ctx.fillStyle = '#FF5252';
                    ctx.fill();
                }
                
                // 绘制阻力向量 (蓝色)
                const dragMagnitude = Math.sqrt(this.dragForce.x * this.dragForce.x + this.dragForce.y * this.dragForce.y);
                const dragLength = dragMagnitude * arrowScale;
                
                if (dragLength > 0.5) {
                    const dragAngle = Math.atan2(this.dragForce.y, this.dragForce.x);
                    
                    ctx.beginPath();
                    ctx.moveTo(this.x, this.y);
                    ctx.lineTo(
                        this.x + dragLength * Math.cos(dragAngle),
                        this.y + dragLength * Math.sin(dragAngle)
                    );
                    ctx.strokeStyle = '#448AFF'; // 蓝色表示阻力
                    ctx.lineWidth = 3;
                    ctx.stroke();
                    
                    // 绘制阻力箭头头部
                    const arrowSize = Math.max(4, Math.min(12, dragLength * 0.2));
                    ctx.beginPath();
                    ctx.moveTo(
                        this.x + dragLength * Math.cos(dragAngle),
                        this.y + dragLength * Math.sin(dragAngle)
                    );
                    ctx.lineTo(
                        this.x + dragLength * Math.cos(dragAngle) - arrowSize * Math.cos(dragAngle - Math.PI/6),
                        this.y + dragLength * Math.sin(dragAngle) - arrowSize * Math.sin(dragAngle - Math.PI/6)
                    );
                    ctx.lineTo(
                        this.x + dragLength * Math.cos(dragAngle) - arrowSize * Math.cos(dragAngle + Math.PI/6),
                        this.y + dragLength * Math.sin(dragAngle) - arrowSize * Math.sin(dragAngle + Math.PI/6)
                    );
                    ctx.closePath();
                    ctx.fillStyle = '#448AFF';
                    ctx.fill();
                }
            }
        }
        
        // 小球类
        class Ball extends PhysicsObject {
            constructor(id, x, y, radius) {
                super(id, OBJECT_TYPES.BALL, x, y);
                this.radius = radius || ballRadius;
                this.mass = this.radius * 0.5;
                this.color = this.getRandomColor();
                this.vx = (Math.random() - 0.5) * 2;
            }
            
            // 生成随机颜色
            getRandomColor() {
                const colors = [
                    '#FF5252', '#FF4081', '#E040FB', '#7C4DFF',
                    '#536DFE', '#448AFF', '#40C4FF', '#18FFFF',
                    '#64FFDA', '#69F0AE', '#B2FF59', '#EEFF41'
                ];
                return colors[Math.floor(Math.random() * colors.length)];
            }
            
            // 处理边界碰撞
            handleBoundaryCollision() {
                // 右边界
                if (this.x + this.radius > canvas.width) {
                    this.x = canvas.width - this.radius;
                    this.vx = -this.vx * elasticity;
                }
                // 左边界
                if (this.x - this.radius < 0) {
                    this.x = this.radius;
                    this.vx = -this.vx * elasticity;
                }
                // 下边界
                if (this.y + this.radius > canvas.height) {
                    this.y = canvas.height - this.radius;
                    this.vy = -this.vy * elasticity;
                }
                // 上边界
                if (this.y - this.radius < 0) {
                    this.y = this.radius;
                    this.vy = -this.vy * elasticity;
                }
            }
            
            // 检测是否与另一个物体碰撞
            isCollidingWith(otherObject) {
                if (otherObject.type === OBJECT_TYPES.BALL) {
                    // 球与球的碰撞检测
                    const dx = otherObject.x - this.x;
                    const dy = otherObject.y - this.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    return distance < (this.radius + otherObject.radius);
                } else if (otherObject.type === OBJECT_TYPES.SQUARE) {
                    // 球与正方形的碰撞检测
                    return otherObject.isCollidingWith(this); // 调用正方形的检测方法
                }
                return false;
            }
            
            // 解决碰撞
            resolveCollision(otherObject) {
                if (otherObject.type === OBJECT_TYPES.BALL) {
                    // 球与球的碰撞解决
                    const dx = otherObject.x - this.x;
                    const dy = otherObject.y - this.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance === 0) return;
                    
                    // 计算碰撞法线
                    const nx = dx / distance;
                    const ny = dy / distance;
                    
                    // 计算相对速度
                    const dvx = otherObject.vx - this.vx;
                    const dvy = otherObject.vy - this.vy;
                    
                    // 计算相对速度在法线方向上的投影
                    const speed = dvx * nx + dvy * ny;
                    
                    // 如果小球正在远离，则不处理碰撞
                    if (speed > 0) return;
                    
                    // 计算冲量
                    const impulse = 2 * speed / (this.mass + otherObject.mass);
                    
                    // 应用冲量（考虑弹性）
                    this.vx += impulse * otherObject.mass * nx * elasticity;
                    this.vy += impulse * otherObject.mass * ny * elasticity;
                    otherObject.vx -= impulse * this.mass * nx * elasticity;
                    otherObject.vy -= impulse * this.mass * ny * elasticity;
                    
                    // 分离小球，防止重叠
                    const overlap = (this.radius + otherObject.radius) - distance;
                    const separateX = overlap * nx * 0.5;
                    const separateY = overlap * ny * 0.5;
                    
                    this.x -= separateX;
                    this.y -= separateY;
                    otherObject.x += separateX;
                    otherObject.y += separateY;
                } else if (otherObject.type === OBJECT_TYPES.SQUARE) {
                    // 球与正方形的碰撞解决（由正方形处理）
                    otherObject.resolveCollision(this);
                }
            }
            
            // 绘制小球
            draw() {
                // 绘制轨迹
                if (this.trail.length > 1) {
                    ctx.beginPath();
                    ctx.moveTo(this.trail[0].x, this.trail[0].y);
                    
                    for (let i = 1; i < this.trail.length; i++) {
                        const point = this.trail[i];
                        ctx.lineTo(point.x, point.y);
                    }
                    
                    ctx.strokeStyle = this.color + '80'; // 半透明
                    ctx.lineWidth = 2;
                    ctx.stroke();
                }
                
                // 绘制小球
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                
                // 创建径向渐变
                const gradient = ctx.createRadialGradient(
                    this.x - this.radius/3, this.y - this.radius/3, 1,
                    this.x, this.y, this.radius
                );
                
                gradient.addColorStop(0, '#FFFFFF');
                gradient.addColorStop(0.3, this.color);
                gradient.addColorStop(1, this.color + 'CC');
                
                ctx.fillStyle = gradient;
                ctx.fill();
                
                // 绘制小球边框
                ctx.lineWidth = 2;
                ctx.strokeStyle = '#FFFFFF';
                ctx.stroke();
                
                // 如果小球被抓住，绘制抓取效果
                if (this.isGrabbed) {
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, this.radius + 5, 0, Math.PI * 2);
                    ctx.strokeStyle = '#FFFF00';
                    ctx.lineWidth = 3;
                    ctx.setLineDash([5, 5]);
                    ctx.stroke();
                    ctx.setLineDash([]);
                }
                
                // 绘制小球ID和质量
                ctx.fillStyle = '#FFFFFF';
                ctx.font = 'bold 14px Arial';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(`#${this.id}`, this.x, this.y);
                
                // 在小球下方绘制重力信息
                ctx.font = '12px Arial';
                ctx.fillText(`${this.gravityForce.toFixed(1)} N`, this.x, this.y + this.radius + 15);
                
                // 如果显示受力箭头，绘制力向量
                if (showForces) {
                    this.drawForceVectors();
                }
            }
            
            // 检查点是否在小球内部
            containsPoint(x, y) {
                const dx = x - this.x;
                const dy = y - this.y;
                return dx * dx + dy * dy <= this.radius * this.radius;
            }
        }
        
        // 正方形类
        class Square extends PhysicsObject {
            constructor(id, x, y, size) {
                super(id, OBJECT_TYPES.SQUARE, x, y);
                this.size = size || 40;
                this.mass = this.size * 0.5;
                this.color = '#4CAF50';
            }
            
            // 处理边界碰撞
            handleBoundaryCollision() {
                const halfSize = this.size / 2;
                
                // 右边界
                if (this.x + halfSize > canvas.width) {
                    this.x = canvas.width - halfSize;
                    this.vx = -this.vx * elasticity;
                }
                // 左边界
                if (this.x - halfSize < 0) {
                    this.x = halfSize;
                    this.vx = -this.vx * elasticity;
                }
                // 下边界
                if (this.y + halfSize > canvas.height) {
                    this.y = canvas.height - halfSize;
                    this.vy = -this.vy * elasticity;
                }
                // 上边界
                if (this.y - halfSize < 0) {
                    this.y = halfSize;
                    this.vy = -this.vy * elasticity;
                }
            }
            
            // 检测是否与另一个物体碰撞
            isCollidingWith(otherObject) {
                if (otherObject.type === OBJECT_TYPES.BALL) {
                    // 正方形与球的碰撞检测
                    const halfSize = this.size / 2;
                    
                    // 计算球心到正方形中心的最近点
                    const closestX = Math.max(this.x - halfSize, Math.min(otherObject.x, this.x + halfSize));
                    const closestY = Math.max(this.y - halfSize, Math.min(otherObject.y, this.y + halfSize));
                    
                    // 计算最近点到球心的距离
                    const dx = otherObject.x - closestX;
                    const dy = otherObject.y - closestY;
                    
                    return (dx * dx + dy * dy) < (otherObject.radius * otherObject.radius);
                } else if (otherObject.type === OBJECT_TYPES.SQUARE) {
                    // 正方形与正方形的碰撞检测
                    const halfSize1 = this.size / 2;
                    const halfSize2 = otherObject.size / 2;
                    
                    return (Math.abs(this.x - otherObject.x) < (halfSize1 + halfSize2) &&
                            Math.abs(this.y - otherObject.y) < (halfSize1 + halfSize2));
                }
                return false;
            }
            
            // 解决碰撞
            resolveCollision(otherObject) {
                if (otherObject.type === OBJECT_TYPES.BALL) {
                    // 正方形与球的碰撞解决
                    const halfSize = this.size / 2;
                    
                    // 计算球心到正方形中心的最近点
                    const closestX = Math.max(this.x - halfSize, Math.min(otherObject.x, this.x + halfSize));
                    const closestY = Math.max(this.y - halfSize, Math.min(otherObject.y, this.y + halfSize));
                    
                    // 计算最近点到球心的距离
                    const dx = otherObject.x - closestX;
                    const dy = otherObject.y - closestY;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance === 0) return;
                    
                    // 计算碰撞法线
                    const nx = dx / distance;
                    const ny = dy / distance;
                    
                    // 计算相对速度
                    const dvx = otherObject.vx - this.vx;
                    const dvy = otherObject.vy - this.vy;
                    
                    // 计算相对速度在法线方向上的投影
                    const speed = dvx * nx + dvy * ny;
                    
                    // 如果正在远离，则不处理碰撞
                    if (speed > 0) return;
                    
                    // 计算冲量
                    const impulse = 2 * speed / (this.mass + otherObject.mass);
                    
                    // 应用冲量（考虑弹性）
                    this.vx += impulse * otherObject.mass * nx * elasticity;
                    this.vy += impulse * otherObject.mass * ny * elasticity;
                    otherObject.vx -= impulse * this.mass * nx * elasticity;
                    otherObject.vy -= impulse * this.mass * ny * elasticity;
                    
                    // 分离物体，防止重叠
                    const overlap = otherObject.radius - distance;
                    const separateX = overlap * nx * 0.5;
                    const separateY = overlap * ny * 0.5;
                    
                    this.x -= separateX;
                    this.y -= separateY;
                    otherObject.x += separateX;
                    otherObject.y += separateY;
                } else if (otherObject.type === OBJECT_TYPES.SQUARE) {
                    // 正方形与正方形的碰撞解决
                    const halfSize1 = this.size / 2;
                    const halfSize2 = otherObject.size / 2;
                    
                    // 计算碰撞法线（从this指向otherObject）
                    const nx = otherObject.x - this.x;
                    const ny = otherObject.y - this.y;
                    const distance = Math.sqrt(nx * nx + ny * ny);
                    
                    if (distance === 0) return;
                    
                    const normalX = nx / distance;
                    const normalY = ny / distance;
                    
                    // 计算相对速度
                    const dvx = otherObject.vx - this.vx;
                    const dvy = otherObject.vy - this.vy;
                    
                    // 计算相对速度在法线方向上的投影
                    const speed = dvx * normalX + dvy * normalY;
                    
                    // 如果正在远离，则不处理碰撞
                    if (speed > 0) return;
                    
                    // 计算冲量
                    const impulse = 2 * speed / (this.mass + otherObject.mass);
                    
                    // 应用冲量（考虑弹性）
                    this.vx += impulse * otherObject.mass * normalX * elasticity;
                    this.vy += impulse * otherObject.mass * normalY * elasticity;
                    otherObject.vx -= impulse * this.mass * normalX * elasticity;
                    otherObject.vy -= impulse * this.mass * normalY * elasticity;
                    
                    // 分离物体，防止重叠
                    const overlap = (halfSize1 + halfSize2) - distance;
                    const separateX = overlap * normalX * 0.5;
                    const separateY = overlap * normalY * 0.5;
                    
                    this.x -= separateX;
                    this.y -= separateY;
                    otherObject.x += separateX;
                    otherObject.y += separateY;
                }
            }
            
            // 绘制正方形
            draw() {
                const halfSize = this.size / 2;
                
                // 绘制轨迹
                if (this.trail.length > 1) {
                    ctx.beginPath();
                    ctx.moveTo(this.trail[0].x, this.trail[0].y);
                    
                    for (let i = 1; i < this.trail.length; i++) {
                        const point = this.trail[i];
                        ctx.lineTo(point.x, point.y);
                    }
                    
                    ctx.strokeStyle = this.color + '80'; // 半透明
                    ctx.lineWidth = 2;
                    ctx.stroke();
                }
                
                // 保存当前画布状态
                ctx.save();
                
                // 移动到正方形中心并旋转
                ctx.translate(this.x, this.y);
                ctx.rotate(this.rotation);
                
                // 绘制正方形
                ctx.fillStyle = this.color;
                ctx.fillRect(-halfSize, -halfSize, this.size, this.size);
                
                // 绘制正方形边框
                ctx.lineWidth = 2;
                ctx.strokeStyle = '#FFFFFF';
                ctx.strokeRect(-halfSize, -halfSize, this.size, this.size);
                
                // 如果正方形被抓住，绘制抓取效果
                if (this.isGrabbed) {
                    ctx.beginPath();
                    ctx.rect(-halfSize - 5, -halfSize - 5, this.size + 10, this.size + 10);
                    ctx.strokeStyle = '#FFFF00';
                    ctx.lineWidth = 3;
                    ctx.setLineDash([5, 5]);
                    ctx.stroke();
                    ctx.setLineDash([]);
                }
                
                // 恢复画布状态
                ctx.restore();
                
                // 绘制正方形ID和质量
                ctx.fillStyle = '#FFFFFF';
                ctx.font = 'bold 14px Arial';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(`#${this.id}`, this.x, this.y);
                
                // 在正方形下方绘制重力信息
                ctx.font = '12px Arial';
                ctx.fillText(`${this.gravityForce.toFixed(1)} N`, this.x, this.y + halfSize + 15);
                
                // 如果显示受力箭头，绘制力向量
                if (showForces) {
                    this.drawForceVectors();
                }
            }
            
            // 检查点是否在正方形内部
            containsPoint(x, y) {
                const halfSize = this.size / 2;
                return (x >= this.x - halfSize && x <= this.x + halfSize &&
                        y >= this.y - halfSize && y <= this.y + halfSize);
            }
        }
        
        // 三角形斜面类
        class TriangleRamp extends PhysicsObject {
            constructor(id, x, y, width, height, angle) {
                super(id, OBJECT_TYPES.TRIANGLE, x, y);
                this.width = width || 150;
                this.height = height || 80;
                this.angle = (angle || 30) * Math.PI / 180; // 转换为弧度
                this.color = '#FF9800';
                this.mass = 1000; // 斜面质量很大，视为固定
                this.fixed = true; // 斜面固定不动
                this.vertices = this.calculateVertices();
            }
            
            // 计算三角形顶点
            calculateVertices() {
                // 顶点A: 左下角（旋转中心）
                const Ax = this.x;
                const Ay = this.y;
                
                // 顶点B: 右下角
                const Bx = this.x + this.width * Math.cos(this.angle);
                const By = this.y - this.width * Math.sin(this.angle);
                
                // 顶点C: 上顶点
                const Cx = this.x + this.height * Math.sin(this.angle);
                const Cy = this.y - this.height * Math.cos(this.angle);
                
                return [
                    {x: Ax, y: Ay}, // 左下角
                    {x: Bx, y: By}, // 右下角
                    {x: Cx, y: Cy}  // 上顶点
                ];
            }
            
            // 更新斜面（斜面固定不动）
            update() {
                // 斜面固定，不更新位置
                this.vertices = this.calculateVertices();
            }
            
            // 绘制斜面
            draw() {
                // 绘制斜面三角形
                ctx.beginPath();
                ctx.moveTo(this.vertices[0].x, this.vertices[0].y);
                ctx.lineTo(this.vertices[1].x, this.vertices[1].y);
                ctx.lineTo(this.vertices[2].x, this.vertices[2].y);
                ctx.closePath();
                
                // 填充三角形
                ctx.fillStyle = this.color + 'AA';
                ctx.fill();
                
                // 绘制三角形边框
                ctx.lineWidth = 2;
                ctx.strokeStyle = '#FFFFFF';
                ctx.stroke();
                
                // 绘制斜面ID和倾角
                ctx.fillStyle = '#FFFFFF';
                ctx.font = 'bold 14px Arial';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                
                // 计算三角形中心
                const centerX = (this.vertices[0].x + this.vertices[1].x + this.vertices[2].x) / 3;
                const centerY = (this.vertices[0].y + this.vertices[1].y + this.vertices[2].y) / 3;
                
                ctx.fillText(`#${this.id}`, centerX, centerY);
                
                // 在斜面下方绘制倾角信息
                const angleDeg = Math.round(this.angle * 180 / Math.PI);
                ctx.font = '12px Arial';
                ctx.fillText(`${angleDeg}°`, centerX, centerY + 20);
                
                // 绘制斜面摩擦系数
                ctx.fillText(`μ=${frictionCoefficient.toFixed(2)}`, centerX, centerY + 35);
            }
            
            // 检查点是否在斜面内部
            containsPoint(x, y) {
                // 使用重心坐标法判断点是否在三角形内
                const v0 = this.vertices[0];
                const v1 = this.vertices[1];
                const v2 = this.vertices[2];
                
                const denom = (v1.y - v2.y) * (v0.x - v2.x) + (v2.x - v1.x) * (v0.y - v2.y);
                
                const a = ((v1.y - v2.y) * (x - v2.x) + (v2.x - v1.x) * (y - v2.y)) / denom;
                const b = ((v2.y - v0.y) * (x - v2.x) + (v0.x - v2.x) * (y - v2.y)) / denom;
                const c = 1 - a - b;
                
                return a >= 0 && a <= 1 && b >= 0 && b <= 1 && c >= 0 && c <= 1;
            }
            
            // 设置斜面倾角
            setAngle(degrees) {
                this.angle = degrees * Math.PI / 180;
                this.vertices = this.calculateVertices();
            }
        }
        
        // 更新物体列表显示
        function updateObjectList() {
            objectListElement.innerHTML = '';
            
            if (objects.length === 0) {
                objectListElement.innerHTML = '<div class="empty-list">暂无物体，点击上方按钮添加</div>';
                return;
            }
            
            objects.forEach(obj => {
                const objectItem = document.createElement('div');
                objectItem.className = `object-item ${obj.type}`;
                objectItem.innerHTML = `
                    <div class="object-info">
                        <div>
                            <span class="object-icon">
                                ${obj.type === OBJECT_TYPES.BALL ? '<i class="fas fa-circle" style="color:' + obj.color + '"></i>' :
                                  obj.type === OBJECT_TYPES.SQUARE ? '<i class="fas fa-square" style="color:#4CAF50"></i>' :
                                  '<i class="fas fa-shapes" style="color:#FF9800"></i>'}
                            </span>
                            <span class="object-id">${obj.type === OBJECT_TYPES.BALL ? '小球' : 
                                                    obj.type === OBJECT_TYPES.SQUARE ? '正方形' : '斜面'} #${obj.id}</span>
                        </div>
                        <div class="object-details">
                            ${obj.type === OBJECT_TYPES.BALL ? `半径: ${obj.radius}px | 质量: ${obj.mass.toFixed(1)} kg` :
                              obj.type === OBJECT_TYPES.SQUARE ? `边长: ${obj.size}px | 质量: ${obj.mass.toFixed(1)} kg` :
                              `宽: ${obj.width}px | 高: ${obj.height}px | 倾角: ${Math.round(obj.angle * 180 / Math.PI)}°`}
                        </div>
                    </div>
                    <div class="object-status">
                        ${obj.isGrabbed ? '<i class="fas fa-hand-paper" style="color:#FFD700;"></i> 被抓取' : 
                          obj.fixed ? '<i class="fas fa-anchor" style="color:#aaa;"></i> 固定' : 
                          '<i class="fas fa-sync-alt" style="color:#6ee7ff;"></i> 运动中'}
                    </div>
                `;
                objectListElement.appendChild(objectItem);
            });
        }
        
        // 更新统计信息
        function updateStats() {
            currentGravityElement.textContent = `${gravity.toFixed(1)} m/s²`;
            currentDragElement.textContent = dragCoefficient.toFixed(2);
            currentFrictionElement.textContent = frictionCoefficient.toFixed(2);
            currentSpeedElement.textContent = timeScale.toFixed(1);
            currentArrowScaleElement.textContent = arrowScale.toFixed(2);
            objectCountElement.textContent = objects.length;
            simObjectCountElement.textContent = objects.length;
            
            if (grabbedObject) {
                grabbedObjectElement.textContent = `#${grabbedObject.id} (${grabbedObject.type === OBJECT_TYPES.BALL ? '小球' : 
                                                    grabbedObject.type === OBJECT_TYPES.SQUARE ? '正方形' : '斜面'})`;
            } else {
                grabbedObjectElement.textContent = "无";
            }
        }
        
        // 添加小球
        function addBall() {
            objectIdCounter++;
            const x = 50 + Math.random() * (canvas.width - 100);
            const y = 50 + Math.random() * (canvas.height - 100);
            const ball = new Ball(objectIdCounter, x, y, ballRadius);
            objects.push(ball);
            
            updateObjectList();
            updateStats();
            
            // 启用移除按钮
            removeObjectButton.disabled = false;
        }
        
        // 添加正方形
        function addSquare() {
            objectIdCounter++;
            const x = 50 + Math.random() * (canvas.width - 100);
            const y = 50 + Math.random() * (canvas.height - 100);
            const square = new Square(objectIdCounter, x, y, 40);
            objects.push(square);
            
            updateObjectList();
            updateStats();
            
            // 启用移除按钮
            removeObjectButton.disabled = false;
        }
        
        // 添加斜面
        function addTriangle() {
            objectIdCounter++;
            const x = 50 + Math.random() * (canvas.width - 200);
            const y = canvas.height - 100;
            const triangle = new TriangleRamp(objectIdCounter, x, y, 150, 80, 30);
            objects.push(triangle);
            
            updateObjectList();
            updateStats();
            
            // 启用移除按钮
            removeObjectButton.disabled = false;
        }
        
        // 移除最后一个物体
        function removeObject() {
            if (objects.length > 0) {
                // 如果正在拖拽要移除的物体，先释放
                if (grabbedObject && grabbedObject.id === objects[objects.length - 1].id) {
                    grabbedObject.isGrabbed = false;
                    grabbedObject = null;
                    isDragging = false;
                }
                
                // 如果正在设置速度的物体被移除，隐藏速度控制面板
                if (velocityTargetObject && velocityTargetObject.id === objects[objects.length - 1].id) {
                    velocityTargetObject = null;
                    velocityControl.style.display = 'none';
                }
                
                objects.pop();
                
                // 如果没有物体了，禁用移除按钮
                if (objects.length === 0) {
                    removeObjectButton.disabled = true;
                }
                
                updateObjectList();
                updateStats();
            }
        }
        
        // 设置重力
        function setGravity(value) {
            gravity = value;
            gravityValueElement.textContent = `${gravity.toFixed(1)} m/s²`;
            gravitySlider.value = gravity;
            updateStats();
        }
        
        // 设置小球半径
        function setBallRadius(value) {
            ballRadius = value;
            ballRadiusValueElement.textContent = `${ballRadius} 像素`;
            ballRadiusSlider.value = ballRadius;
            updateStats();
        }
        
        // 设置碰撞弹性
        function setElasticity(value) {
            elasticity = value;
            elasticityValueElement.textContent = elasticity;
            elasticitySlider.value = elasticity;
            updateStats();
        }
        
        // 设置空气阻力系数
        function setDragCoefficient(value) {
            dragCoefficient = value;
            dragValueElement.textContent = dragCoefficient.toFixed(2);
            dragSlider.value = dragCoefficient;
            updateStats();
        }
        
        // 设置斜面摩擦系数
        function setFrictionCoefficient(value) {
            frictionCoefficient = value;
            frictionValueElement.textContent = frictionCoefficient.toFixed(2);
            frictionSlider.value = frictionCoefficient;
            updateStats();
        }
        
        // 设置时间倍速
        function setTimeScale(value) {
            timeScale = value;
            speedValueElement.textContent = timeScale.toFixed(1);
            speedSlider.value = timeScale;
            updateStats();
        }
        
        // 设置箭头缩放因子
        function setArrowScale(value) {
            arrowScale = value;
            arrowScaleValueElement.textContent = arrowScale.toFixed(2);
            arrowScaleSlider.value = arrowScale;
            updateStats();
        }
        
        // 显示速度控制面板
        function showVelocityControl(obj) {
            velocityTargetObject = obj;
            velocityObjectIdElement.textContent = `#${obj.id} (${obj.type === OBJECT_TYPES.BALL ? '小球' : 
                                                   obj.type === OBJECT_TYPES.SQUARE ? '正方形' : '斜面'})`;
            velocityMagnitudeInput.value = Math.sqrt(obj.vx*obj.vx + obj.vy*obj.vy).toFixed(0);
            
            // 计算当前速度的角度
            let angle = Math.atan2(obj.vy, obj.vx) * 180 / Math.PI;
            if (angle < 0) angle += 360;
            velocityAngleInput.value = angle.toFixed(0);
            
            velocityControl.style.display = 'block';
        }
        
        // 应用速度设置
        function applyVelocity() {
            if (!velocityTargetObject) return;
            
            const magnitude = parseFloat(velocityMagnitudeInput.value);
            const angle = parseFloat(velocityAngleInput.value);
            
            if (isNaN(magnitude) || isNaN(angle)) {
                alert('请输入有效的数值');
                return;
            }
            
            velocityTargetObject.setVelocity(magnitude, angle);
            updateObjectList();
            
            // 隐藏速度控制面板
            velocityControl.style.display = 'none';
            velocityTargetObject = null;
        }
        
        // 重力按钮点击事件
        gravityButtons.forEach(button => {
            button.addEventListener('click', () => {
                const gravityValue = parseFloat(button.getAttribute('data-gravity'));
                setGravity(gravityValue);
                
                // 更新按钮活动状态
                gravityButtons.forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
            });
        });
        
        // 重力滑块事件
        gravitySlider.addEventListener('input', () => {
            const value = parseFloat(gravitySlider.value);
            setGravity(value);
            
            // 更新按钮活动状态
            gravityButtons.forEach(btn => btn.classList.remove('active'));
        });
        
        // 小球半径滑块事件
        ballRadiusSlider.addEventListener('input', () => {
            const value = parseInt(ballRadiusSlider.value);
            setBallRadius(value);
        });
        
        // 弹性滑块事件
        elasticitySlider.addEventListener('input', () => {
            const value = parseFloat(elasticitySlider.value);
            setElasticity(value);
        });
        
        // 阻力滑块事件
        dragSlider.addEventListener('input', () => {
            const value = parseFloat(dragSlider.value);
            setDragCoefficient(value);
        });
        
        // 摩擦系数滑块事件
        frictionSlider.addEventListener('input', () => {
            const value = parseFloat(frictionSlider.value);
            setFrictionCoefficient(value);
        });
        
        // 倍速滑块事件
        speedSlider.addEventListener('input', () => {
            const value = parseFloat(speedSlider.value);
            setTimeScale(value);
        });
        
        // 箭头缩放滑块事件
        arrowScaleSlider.addEventListener('input', () => {
            const value = parseFloat(arrowScaleSlider.value);
            setArrowScale(value);
        });
        
        // 显示受力箭头复选框事件
        showForcesCheckbox.addEventListener('change', () => {
            showForces = showForcesCheckbox.checked;
            
            // 显示或隐藏箭头缩放滑块
            if (showForces) {
                arrowScaleContainer.style.display = 'block';
            } else {
                arrowScaleContainer.style.display = 'none';
            }
        });
        
        // 添加小球按钮事件
        addBallButton.addEventListener('click', addBall);
        
        // 添加正方形按钮事件
        addSquareButton.addEventListener('click', addSquare);
        
        // 添加斜面按钮事件
        addTriangleButton.addEventListener('click', addTriangle);
        
        // 移除物体按钮事件
        removeObjectButton.addEventListener('click', removeObject);
        
        // 应用速度按钮事件
        applyVelocityButton.addEventListener('click', applyVelocity);
        
        // 鼠标事件处理
        canvas.addEventListener('mousedown', (e) => {
            const rect = canvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;
            
            // 检查是否点击到了物体
            for (let i = objects.length - 1; i >= 0; i--) {
                const obj = objects[i];
                if (obj.containsPoint(x, y) && !obj.fixed) {
                    // 左键：抓取物体
                    if (e.button === 0) {
                        grabbedObject = obj;
                        obj.isGrabbed = true;
                        obj.vx = 0;
                        obj.vy = 0;
                        isDragging = true;
                        
                        // 将选中物体移到数组末尾，使其最后绘制（在最上层）
                        objects.splice(i, 1);
                        objects.push(grabbedObject);
                        
                        updateObjectList();
                        updateStats();
                    }
                    // 右键：显示速度控制面板
                    else if (e.button === 2) {
                        e.preventDefault(); // 防止默认右键菜单
                        showVelocityControl(obj);
                    }
                    break;
                }
            }
        });
        
        canvas.addEventListener('mousemove', (e) => {
            if (isDragging && grabbedObject) {
                const rect = canvas.getBoundingClientRect();
                grabbedObject.x = e.clientX - rect.left;
                grabbedObject.y = e.clientY - rect.top;
                
                // 清除轨迹
                grabbedObject.trail = [];
            }
        });
        
        canvas.addEventListener('mouseup', (e) => {
            if (isDragging && grabbedObject && e.button === 0) {
                grabbedObject.isGrabbed = false;
                grabbedObject = null;
                isDragging = false;
                
                updateObjectList();
                updateStats();
            }
        });
        
        canvas.addEventListener('mouseleave', () => {
            if (isDragging && grabbedObject) {
                grabbedObject.isGrabbed = false;
                grabbedObject = null;
                isDragging = false;
                
                updateObjectList();
                updateStats();
            }
        });
        
        // 防止右键菜单
        canvas.addEventListener('contextmenu', (e) => {
            e.preventDefault();
        });
        
        // 动画循环
        function animate(timestamp) {
            if (!lastTime) lastTime = timestamp;
            const deltaTime = (timestamp - lastTime) / 1000; // 转换为秒
            lastTime = timestamp;
            
            // 清空画布
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // 绘制背景
            ctx.fillStyle = '#0a0f2e';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // 绘制网格
            ctx.strokeStyle = 'rgba(100, 150, 255, 0.1)';
            ctx.lineWidth = 1;
            
            const gridSize = 50;
            for (let x = 0; x < canvas.width; x += gridSize) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, canvas.height);
                ctx.stroke();
            }
            
            for (let y = 0; y < canvas.height; y += gridSize) {
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(canvas.width, y);
                ctx.stroke();
            }
            
            // 首先绘制斜面（作为背景）
            objects.forEach(obj => {
                if (obj.type === OBJECT_TYPES.TRIANGLE) {
                    obj.update();
                    obj.draw();
                }
            });
            
            // 更新和绘制其他物体
            objects.forEach(obj => {
                if (obj.type !== OBJECT_TYPES.TRIANGLE && !obj.fixed) {
                    obj.update(deltaTime, gravity, dragCoefficient, objects);
                    obj.draw();
                }
            });
            
            // 绘制当前模拟信息
            ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
            ctx.font = 'bold 16px Arial';
            ctx.textAlign = 'right';
            ctx.fillText(`重力: ${gravity.toFixed(1)} m/s²`, canvas.width - 20, 30);
            ctx.fillText(`阻力: ${dragCoefficient.toFixed(2)}`, canvas.width - 20, 55);
            ctx.fillText(`摩擦: ${frictionCoefficient.toFixed(2)}`, canvas.width - 20, 80);
            ctx.fillText(`倍速: ${timeScale.toFixed(1)}x`, canvas.width - 20, 105);
            ctx.fillText(`物体: ${objects.length}个`, canvas.width - 20, 130);
            
            // 绘制图例
            if (showForces) {
                ctx.textAlign = 'left';
                ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
                ctx.fillText('受力箭头:', 20, canvas.height - 55);
                ctx.fillStyle = '#FF5252';
                ctx.fillText('● 重力', 20, canvas.height - 30);
                ctx.fillStyle = '#448AFF';
                ctx.fillText('● 空气阻力', 80, canvas.height - 30);
            }
            
            // 绘制物体图例
            ctx.textAlign = 'left';
            ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
            ctx.fillText('物体类型:', 20, canvas.height - 100);
            ctx.fillStyle = '#FF5252';
            ctx.fillText('● 小球', 20, canvas.height - 75);
            ctx.fillStyle = '#4CAF50';
            ctx.fillText('● 正方形', 80, canvas.height - 75);
            ctx.fillStyle = '#FF9800';
            ctx.fillText('● 斜面', 150, canvas.height - 75);
            
            // 绘制重力方向箭头
            if (gravity > 0) {
                const centerX = canvas.width - 50;
                const centerY = 155;
                
                // 箭头线
                ctx.beginPath();
                ctx.moveTo(centerX, centerY);
                ctx.lineTo(centerX, centerY + 60);
                ctx.strokeStyle = '#FF5252';
                ctx.lineWidth = 3;
                ctx.stroke();
                
                // 箭头头部
                ctx.beginPath();
                ctx.moveTo(centerX - 8, centerY + 50);
                ctx.lineTo(centerX, centerY + 65);
                ctx.lineTo(centerX + 8, centerY + 50);
                ctx.fillStyle = '#FF5252';
                ctx.fill();
                
                // 重力标签
                ctx.fillStyle = '#FF5252';
                ctx.font = '14px Arial';
                ctx.textAlign = 'center';
                ctx.fillText('重力方向', centerX, centerY + 85);
            }
            
            animationId = requestAnimationFrame(animate);
        }
        
        // 启动动画
        animationId = requestAnimationFrame(animate);
        
        // 初始化
        updateObjectList();
        updateStats();
        setBallRadius(ballRadius);
        setElasticity(elasticity);
        setDragCoefficient(dragCoefficient);
        setFrictionCoefficient(frictionCoefficient);
        setTimeScale(timeScale);
        setArrowScale(arrowScale);
        
        // 默认不显示受力箭头，所以隐藏箭头缩放滑块
        arrowScaleContainer.style.display = 'none';
    </script>
</body>
</html>
