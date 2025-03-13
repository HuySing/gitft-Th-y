<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gửi bạn của tôi</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding-top: 50px;
            background-color: #fffaf0;
            overflow: hidden;
            touch-action: manipulation;
        }
        h1 {
            color: #333;
        }
        .flower-container {
            margin: 30px auto;
            position: relative;
            width: 200px;
            height: 200px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .flower {
            font-size: 100px;
            cursor: pointer;
            transition: all 0.5s ease;
            user-select: none;
            -webkit-user-select: none;
            -webkit-tap-highlight-color: transparent;
        }
        .flower.blooming {
            transform: scale(1.5);
            animation: bloom 1s ease-out;
        }
        @keyframes bloom {
            0% { transform: scale(1); }
            50% { transform: scale(1.8); }
            100% { transform: scale(1.5); }
        }
        .petals {
            position: absolute;
            top: 50%;
            left: 50%;
            font-size: 40px;
            opacity: 0;
            pointer-events: none;
        }
        .petal-animation {
            animation: petal-fly 1.5s ease-out forwards;
        }
        @keyframes petal-fly {
            0% {
                opacity: 0;
                transform: translate(-50%, -50%) scale(0);
            }
            20% {
                opacity: 1;
            }
            100% {
                opacity: 0;
                transform: translate(calc(-50% + var(--x)), calc(-50% + var(--y))) rotate(var(--r)) scale(1);
            }
        }
        .message {
            font-size: 24px;
            color: #ff6b6b;
            margin: 20px auto;
            padding: 15px;
            display: none;
            background-color: #ffe6f2;
            border-radius: 10px;
            width: 80%;
            max-width: 500px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        .heart {
            position: fixed;
            font-size: 20px;
            color: red;
            top: -10%;
            animation: fall linear forwards;
            pointer-events: none;
        }
        @keyframes fall {
            from {
                transform: translateY(0) rotate(0deg);
                opacity: 1;
            }
            to {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }
    </style>
</head>
<body>
    <h1>Gửi bạn của tôi</h1>
    
    <div class="flower-container">
        <div id="flower" class="flower">🌷</div>
        <!-- Cánh hoa sẽ được thêm vào đây -->
    </div>
    
    <div id="greetingMessage" class="message">
        14-03 vui vẻ nha ❤️ ❤️
    </div>
    
    <script>
        // Hỗ trợ cả sự kiện click và touch
        const flower = document.getElementById("flower");
        const message = document.getElementById("greetingMessage");
        const flowerContainer = document.querySelector(".flower-container");
        let isOpen = false;
        let heartInterval;
        
        // Đăng ký cả sự kiện click và touch
        flower.addEventListener("click", toggleFlower);
        flower.addEventListener("touchstart", function(e) {
            e.preventDefault(); // Ngăn chặn hành vi mặc định
            toggleFlower();
        });
        
        function toggleFlower() {
            if (!isOpen) {
                // Hiệu ứng nở hoa
                flower.textContent = "🌺";
                flower.classList.add("blooming");
                
                // Hiển thị lời chúc
                message.style.display = "block";
                
                // Tạo hiệu ứng cánh hoa bay ra
                createPetals();
                
                // Bắt đầu hiệu ứng trái tim
                createHearts();
                
                isOpen = true;
            } else {
                // Đóng hoa
                flower.textContent = "🌷";
                flower.classList.remove("blooming");
                
                // Ẩn lời chúc
                message.style.display = "none";
                
                // Dừng hiệu ứng trái tim
                stopHearts();
                
                isOpen = false;
            }
        }
        
        function createPetals() {
            // Xóa cánh hoa cũ
            const oldPetals = document.querySelectorAll(".petals");
            oldPetals.forEach(petal => petal.remove());
            
            // Tạo 8 cánh hoa bay ra các hướng khác nhau
            const petalEmojis = ["🌸", "💮", "🌹", "🌷", "🌼", "🌻"];
            
            for (let i = 0; i < 8; i++) {
                const petal = document.createElement("div");
                petal.className = "petals";
                petal.textContent = petalEmojis[Math.floor(Math.random() * petalEmojis.length)];
                
                // Tính toán hướng bay (x, y) và góc xoay (r)
                const angle = (i * 45) * (Math.PI / 180);
                const distance = 100 + Math.random() * 50;
                const x = Math.cos(angle) * distance;
                const y = Math.sin(angle) * distance;
                const r = Math.random() * 360;
                
                petal.style.setProperty('--x', x + 'px');
                petal.style.setProperty('--y', y + 'px');
                petal.style.setProperty('--r', r + 'deg');
                
                flowerContainer.appendChild(petal);
                
                // Kích hoạt animation sau một khoảng thời gian ngắn
                setTimeout(() => {
                    petal.classList.add("petal-animation");
                }, 10);
                
                // Xóa cánh hoa sau khi animation kết thúc
                setTimeout(() => {
                    petal.remove();
                }, 1600);
            }
        }
        
        function createHearts() {
            // Dừng interval cũ nếu có
            stopHearts();
            
            // Tạo trái tim mỗi 300ms
            heartInterval = setInterval(function() {
                var heart = document.createElement("div");
                heart.innerHTML = "❤️";
                heart.classList.add("heart");
                
                // Vị trí ngẫu nhiên từ trái sang phải
                heart.style.left = Math.random() * 100 + "%";
                
                // Thời gian rơi ngẫu nhiên từ 3-6 giây
                var duration = Math.random() * 3 + 3;
                heart.style.animationDuration = duration + "s";
                
                document.body.appendChild(heart);
                
                // Xóa trái tim sau khi animation kết thúc
                setTimeout(function() {
                    heart.remove();
                }, duration * 1000);
            }, 300);
        }
        
        function stopHearts() {
            if (heartInterval) {
                clearInterval(heartInterval);
            }
        }
    </script>
</body>
</html>
