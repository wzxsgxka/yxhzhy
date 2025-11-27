<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>红色征程：中国革命历史交互地图</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body { 
            font-family: 'Microsoft YaHei', 'PingFang SC', sans-serif;
            background: linear-gradient(135deg, #8B0000 0%, #B22222 30%, #CD5C5C 70%, #8B0000 100%);
            color: white;
            min-height: 100vh;
            position: relative;
            overflow-x: hidden;
        }
        
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                radial-gradient(circle at 20% 80%, rgba(255,215,0,0.1) 0%, transparent 50%),
                radial-gradient(circle at 80% 20%, rgba(255,255,255,0.1) 0%, transparent 50%),
                radial-gradient(circle at 40% 40%, rgba(139,0,0,0.2) 0%, transparent 50%);
            z-index: -1;
        }
        
        .container { 
            max-width: 1400px; 
            margin: 0 auto;
            padding: 20px;
            position: relative;
            z-index: 1;
        }
        
        header { 
            text-align: center; 
            margin-bottom: 30px;
            padding: 40px 30px;
            background: linear-gradient(135deg, rgba(0,0,0,0.4) 0%, rgba(139,0,0,0.3) 100%);
            border-radius: 20px;
            backdrop-filter: blur(15px);
            box-shadow: 0 15px 35px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.1);
            position: relative;
            overflow: hidden;
        }
        
        header::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
            animation: shimmer 8s infinite;
        }
        
        @keyframes shimmer {
            0% { left: -100%; }
            100% { left: 100%; }
        }
        
        h1 { 
            font-size: 3.5rem; 
            margin-bottom: 15px; 
            text-shadow: 3px 3px 6px rgba(0,0,0,0.5);
            background: linear-gradient(45deg, #ffd700, #ffec8b, #ffd700);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-size: 200% 200%;
            animation: gradientShift 3s ease infinite;
            font-weight: 700;
            letter-spacing: 2px;
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .subtitle { 
            font-size: 1.4rem; 
            opacity: 0.9; 
            margin-bottom: 20px;
            font-weight: 300;
            letter-spacing: 1px;
        }
        
        .logo {
            font-size: 1.1rem;
            background: linear-gradient(135deg, rgba(255,215,0,0.2), rgba(255,255,255,0.1));
            display: inline-block;
            padding: 12px 30px;
            border-radius: 25px;
            margin-top: 15px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,215,0,0.3);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }
        
        .map-area { 
            display: flex; 
            gap: 25px;
            margin-bottom: 30px;
        }
        
        .controls { 
            width: 280px;
            display: flex;
            flex-direction: column;
            gap: 25px;
        }
        
        .layer-controls, .zoom-controls {
            background: linear-gradient(135deg, rgba(0,0,0,0.4) 0%, rgba(139,0,0,0.3) 100%);
            padding: 25px;
            border-radius: 15px;
            backdrop-filter: blur(15px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .layer-btn, .zoom-btn { 
            width: 100%; 
            padding: 15px; 
            margin-bottom: 12px;
            background: linear-gradient(135deg, rgba(255,255,255,0.15) 0%, rgba(139,0,0,0.1) 100%);
            border: none; 
            border-radius: 12px;
            color: white;
            cursor: pointer;
            font-size: 15px;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            font-weight: 500;
            letter-spacing: 0.5px;
        }
        
        .layer-btn:hover, .zoom-btn:hover {
            background: linear-gradient(135deg, rgba(255,255,255,0.25) 0%, rgba(139,0,0,0.2) 100%);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }
        
        .layer-btn.active { 
            background: linear-gradient(135deg, rgba(255,215,0,0.3) 0%, rgba(255,215,0,0.1) 100%);
            box-shadow: 0 8px 25px rgba(255,215,0,0.3);
            border: 1px solid rgba(255,215,0,0.5);
        }
        
        .layer-btn::after, .zoom-btn::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: left 0.6s;
        }
        
        .layer-btn:hover::after, .zoom-btn:hover::after {
            left: 100%;
        }
        
        .map-container { 
            flex: 1; 
            height: 750px; 
            background: linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 100%);
            border-radius: 20px;
            overflow: hidden;
            position: relative;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        #map { 
            width: 100%; 
            height: 100%; 
            cursor: grab; 
            position: relative;
            transition: transform 0.1s ease;
        }
        
        #map:active { cursor: grabbing; }
        
        .layer { 
            position: absolute; 
            width: 100%; 
            height: 100%;
            opacity: 0;
            transition: opacity 0.5s ease;
        }
        
        .layer.active { opacity: 1; }
        
        .layer-image { 
            width: 100%; 
            height: 100%;
            object-fit: contain;
        }
        
        .zoom-level {
            position: absolute;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, rgba(0,0,0,0.7) 0%, rgba(139,0,0,0.5) 100%);
            padding: 10px 20px;
            border-radius: 25px;
            font-size: 0.9rem;
            z-index: 10;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.1);
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .layer-info {
            font-size: 13px;
            color: rgba(255,255,255,0.8);
            margin-top: 8px;
            text-align: center;
            font-weight: 300;
        }
        
        .timeline {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
            background: linear-gradient(135deg, rgba(0,0,0,0.4) 0%, rgba(139,0,0,0.3) 100%);
            border-radius: 15px;
            padding: 20px;
            backdrop-filter: blur(15px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .timeline-item {
            text-align: center;
            flex: 1;
            padding: 15px;
            border-radius: 12px;
            transition: all 0.4s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }
        
        .timeline-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, transparent, rgba(255,255,255,0.05), transparent);
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .timeline-item:hover::before {
            opacity: 1;
        }
        
        .timeline-item.active {
            background: linear-gradient(135deg, rgba(255,215,0,0.2) 0%, rgba(255,215,0,0.1) 100%);
            box-shadow: 0 8px 25px rgba(255,215,0,0.2);
            transform: translateY(-5px);
        }
        
        .timeline-year {
            font-weight: bold;
            font-size: 1.2rem;
            color: #ffd700;
            margin-bottom: 5px;
        }
        
        .timeline-title {
            font-size: 1rem;
            margin-top: 5px;
            font-weight: 500;
        }
        
        .controls h3 {
            color: #ffd700;
            margin-bottom: 20px;
            text-align: center;
            font-size: 1.3rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .historical-context {
            background: linear-gradient(135deg, rgba(0,0,0,0.3) 0%, rgba(139,0,0,0.2) 100%);
            padding: 20px;
            border-radius: 12px;
            margin-top: 15px;
            font-size: 0.95rem;
            line-height: 1.6;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .historical-context p {
            margin-bottom: 10px;
        }
        
        @media (max-width: 768px) {
            .map-area { flex-direction: column; }
            .controls { width: 100%; }
            .map-container { height: 500px; }
            h1 { font-size: 2.5rem; }
            .timeline { flex-direction: column; gap: 15px; }
            .container { padding: 15px; }
        }
        
        .floating-elements {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }
        
        .floating-element {
            position: absolute;
            background: rgba(255,215,0,0.1);
            border-radius: 50%;
            animation: float 15s infinite linear;
        }
        
        @keyframes float {
            0% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
            100% { transform: translateY(0) rotate(360deg); }
        }
    </style>
</head>
<body>
    <div class="floating-elements">
        <div class="floating-element" style="width: 100px; height: 100px; top: 10%; left: 5%;"></div>
        <div class="floating-element" style="width: 150px; height: 150px; top: 60%; left: 80%;"></div>
        <div class="floating-element" style="width: 80px; height: 80px; top: 80%; left: 10%;"></div>
        <div class="floating-element" style="width: 120px; height: 120px; top: 20%; left: 85%;"></div>
    </div>
    
    <div class="container">
        <header>
            <h1>红色征程</h1>
            <p class="subtitle">中国革命历史交互地图</p>
            <div class="logo">
                <i class="fas fa-history"></i>
                探索革命历程 · 传承红色基因
            </div>
        </header>
        
        <div class="timeline">
            <div class="timeline-item active">
                <div class="timeline-year">1921-1927</div>
                <div class="timeline-title">初期斗争</div>
                <div class="timeline-desc">革命萌芽时期</div>
            </div>
            <div class="timeline-item">
                <div class="timeline-year">1927-1934</div>
                <div class="timeline-title">长征之路</div>
                <div class="timeline-desc">战略转移与长征</div>
            </div>
            <div class="timeline-item">
                <div class="timeline-year">1921-1934</div>
                <div class="timeline-title">星星之火</div>
                <div class="timeline-desc">革命力量发展</div>
            </div>
        </div>
        
        <div class="map-area">
            <div class="controls">
                <div class="layer-controls">
                    <h3><i class="fas fa-layer-group"></i> 历史时期</h3>
                    <button class="layer-btn active" id="layer1Btn">
                        <i class="fas fa-seedling"></i> 初期斗争图
                    </button>
                    <div class="layer-info">1921-1927 · 革命初期斗争与探索</div>
                    
                    <button class="layer-btn" id="layer2Btn">
                        <i class="fas fa-route"></i> 长征之路
                    </button>
                    <div class="layer-info">1927-1934 · 战略转移与长征</div>
                    
                    <button class="layer-btn" id="layer3Btn">
                        <i class="fas fa-fire"></i> 星星之火
                    </button>
                    <div class="layer-info">1921-1934 · 革命力量发展壮大</div>
                    
                    <div class="historical-context">
                        <p><strong><i class="fas fa-landmark"></i> 历史背景：</strong></p>
                        <p>这三个时期展现了中国革命从萌芽到发展壮大的完整历程，体现了中国共产党领导人民艰苦奋斗的伟大征程。</p>
                    </div>
                </div>
                
                <div class="zoom-controls">
                    <h3><i class="fas fa-cogs"></i> 地图控制</h3>
                    <button class="zoom-btn" id="zoomIn">
                        <i class="fas fa-search-plus"></i> 放大地图
                    </button>
                    <button class="zoom-btn" id="zoomOut">
                        <i class="fas fa-search-minus"></i> 缩小地图
                    </button>
                    <button class="zoom-btn" id="reset">
                        <i class="fas fa-sync-alt"></i> 重置视图
                    </button>
                </div>
            </div>
            
            <div class="map-container">
                <div class="zoom-level">
                    <i class="fas fa-expand-arrows-alt"></i>
                    缩放级别: <span id="zoomValue">1.0</span>
                </div>
                <div id="map">
                    <!-- 直接内置图片 -->
                    <div class="layer active" id="layer1">
                        <img src="初期斗争图.jpg" class="layer-image" alt="初期斗争图">
                    </div>
                    <div class="layer" id="layer2">
                        <img src="二成长之路.jpg" class="layer-image" alt="长征之路">
                    </div>
                    <div class="layer" id="layer3">
                        <img src="星星之火.jpg" class="layer-image" alt="星星之火">
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const map = document.getElementById('map');
            const zoomInBtn = document.getElementById('zoomIn');
            const zoomOutBtn = document.getElementById('zoomOut');
            const resetBtn = document.getElementById('reset');
            const layer1Btn = document.getElementById('layer1Btn');
            const layer2Btn = document.getElementById('layer2Btn');
            const layer3Btn = document.getElementById('layer3Btn');
            const zoomValue = document.getElementById('zoomValue');
            const timelineItems = document.querySelectorAll('.timeline-item');
            
            let scale = 1;
            let translateX = 0;
            let translateY = 0;
            let isDragging = false;
            let startX, startY;
            
            // 创建浮动元素
            createFloatingElements();
            
            function createFloatingElements() {
                const container = document.querySelector('.floating-elements');
                for (let i = 0; i < 8; i++) {
                    const element = document.createElement('div');
                    element.className = 'floating-element';
                    const size = Math.random() * 80 + 40;
                    element.style.width = `${size}px`;
                    element.style.height = `${size}px`;
                    element.style.top = `${Math.random() * 100}%`;
                    element.style.left = `${Math.random() * 100}%`;
                    element.style.animationDuration = `${Math.random() * 10 + 10}s`;
                    element.style.animationDelay = `${Math.random() * 5}s`;
                    container.appendChild(element);
                }
            }
            
            function updateView() {
                map.style.transform = `scale(${scale}) translate(${translateX}px, ${translateY}px)`;
                zoomValue.textContent = scale.toFixed(1);
            }
            
            function switchLayer(layerIndex) {
                // 更新按钮状态
                layer1Btn.classList.remove('active');
                layer2Btn.classList.remove('active');
                layer3Btn.classList.remove('active');
                
                // 更新图层显示
                const layer1 = document.getElementById('layer1');
                const layer2 = document.getElementById('layer2');
                const layer3 = document.getElementById('layer3');
                
                layer1.classList.remove('active');
                layer2.classList.remove('active');
                layer3.classList.remove('active');
                
                // 更新时间轴状态
                timelineItems.forEach((item, index) => {
                    item.classList.remove('active');
                    if (index === layerIndex) {
                        item.classList.add('active');
                    }
                });
                
                if (layerIndex === 0) {
                    layer1Btn.classList.add('active');
                    layer1.classList.add('active');
                } else if (layerIndex === 1) {
                    layer2Btn.classList.add('active');
                    layer2.classList.add('active');
                } else {
                    layer3Btn.classList.add('active');
                    layer3.classList.add('active');
                }
            }
            
            function zoom(factor) {
                scale *= factor;
                scale = Math.max(0.5, Math.min(scale, 3));
                updateView();
            }
            
            function resetView() {
                scale = 1;
                translateX = 0;
                translateY = 0;
                updateView();
            }
            
            // 事件监听
            zoomInBtn.addEventListener('click', () => zoom(1.2));
            zoomOutBtn.addEventListener('click', () => zoom(0.8));
            resetBtn.addEventListener('click', resetView);
            layer1Btn.addEventListener('click', () => switchLayer(0));
            layer2Btn.addEventListener('click', () => switchLayer(1));
            layer3Btn.addEventListener('click', () => switchLayer(2));
            
            // 时间轴点击事件
            timelineItems.forEach((item, index) => {
                item.addEventListener('click', () => switchLayer(index));
            });
            
            map.addEventListener('wheel', (e) => {
                e.preventDefault();
                zoom(e.deltaY > 0 ? 0.9 : 1.1);
            });
            
            map.addEventListener('mousedown', (e) => {
                isDragging = true;
                startX = e.clientX - translateX;
                startY = e.clientY - translateY;
                map.style.cursor = 'grabbing';
            });
            
            document.addEventListener('mousemove', (e) => {
                if (!isDragging) return;
                translateX = e.clientX - startX;
                translateY = e.clientY - startY;
                updateView();
            });
            
            document.addEventListener('mouseup', () => {
                isDragging = false;
                map.style.cursor = 'grab';
            });
            
            updateView();
        });
    </script>
</body>
</html>
