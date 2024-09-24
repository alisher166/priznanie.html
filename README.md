<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Милана, я люблю тебя!</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #ff9a9e, #fad0c4);
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            height: 100vh;
            font-family: 'Comic Sans MS', cursive;
            overflow: hidden;
            transition: background 0.5s ease; /* Плавный переход фона */
        }

        h1 {
            font-size: 50px;
            text-align: center;
            color: white;
            text-shadow: 2px 2px 4px #000000;
            animation: fadeIn 2s ease-in-out infinite alternate;
            margin-bottom: 20px;
        }

        @keyframes fadeIn {
            from {
                opacity: 0.5;
            }
            to {
                opacity: 1;
            }
        }

        .hearts {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            pointer-events: none; /* Сердечки не мешают кликам по кнопкам */
        }

        .heart {
            position: absolute;
            width: 30px;
            height: 30px;
            background: pink;
            transform: rotate(45deg);
            opacity: 0.8;
            animation: fall 5s infinite ease-in-out;
        }

        .heart:before,
        .heart:after {
            content: '';
            position: absolute;
            width: 30px;
            height: 30px;
            background: pink;
            border-radius: 50%;
        }

        .heart:before {
            top: -15px;
            left: 0;
        }

        .heart:after {
            left: 15px;
            top: 0;
        }

        @keyframes fall {
            0% {
                transform: translateY(0) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(100vh) rotate(720deg); /* Падает вниз и крутится */
                opacity: 0;
            }
        }

        button {
            padding: 10px 20px;
            background-color: #f76c6c;
            color: white;
            font-size: 20px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            position: relative;
            animation: pop 1s infinite alternate;
            margin-top: 10px;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.8);
            margin-bottom: 20px;
        }

        @keyframes pop {
            from {
                transform: scale(1);
            }
            to {
                transform: scale(1.1);
            }
        }

        button:hover {
            background-color: #ff5151;
        }

        button:active {
            background-color: #ff3030;
        }

        #loveText {
            font-size: 30px;
            color: white;
            text-align: center;
            margin-top: 20px;
            animation: fadeInOut 6s ease-in-out infinite;
            text-shadow: 0 0 20px rgba(255, 255, 255, 0.6);
        }

        @keyframes fadeInOut {
            0% { opacity: 0; }
            50% { opacity: 1; }
            100% { opacity: 0; }
        }

        /* Модальное окно */
        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.8);
            padding-top: 60px;
        }

        .modal-content {
            background-color: #fff;
            margin: 5% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 90%;
            max-width: 400px;
            border-radius: 10px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            text-align: center;
        }

        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }

        .close:hover,
        .close:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }

        .settings {
            margin-top: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .color-picker {
            margin-bottom: 10px;
            width: 60%;
            height: 30px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        /* Кнопка настроек */
        #settingsButton {
            position: absolute;
            top: 20px;
            right: 20px;
            background: none;
            border: none;
            font-size: 30px;
            cursor: pointer;
            color: white;
            transition: color 0.3s;
        }

        #settingsButton:hover {
            color: #ffcccc;
        }
    </style>
</head>
<body>

<!-- Музыка на фоне -->
<audio id="backgroundMusic" autoplay loop>
    <source src="https://www.bensound.com/bensound-music/bensound-sunny.mp3" type="audio/mpeg">
    Ваш браузер не поддерживает элемент audio.
</audio>

<!-- Музыка Mot "Порабощённый" -->
<audio id="motSong" controls loop style="display: none;">
    <source src="https://example.com/path-to-mot-song.mp3" type="audio/mpeg"> <!-- Укажите корректный путь к песне -->
    Ваш браузер не поддерживает элемент audio.
</audio>

<div class="hearts" id="heartsContainer"></div>

<!-- Признание -->
<div style="text-align: center;">
    <h1>Милана, я люблю тебя!</h1>
    <button onclick="showHearts()">Нажми на меня!</button>
    <div id="loveText"></div>
    <button id="settingsButton" onclick="openModal()">⚙️</button>
</div>

<!-- Модальное окно с настройками -->
<div id="settingsModal" class="modal">
    <div class="modal-content">
        <span class="close" onclick="closeModal()">&times;</span>
        <h2>Настройки</h2>
        <label for="bgColor">Выберите цвет фона:</label>
        <input type="color" id="bgColor" class="color-picker" value="#ff9a9e" onchange="changeBackgroundColor(this.value)">
        <label for="songSelect">Выберите песню:</label>
        <select id="songSelect" onchange="changeSong(this.value)">
            <option value="https://example.com/path-to-mot-song.mp3">Мот - Порабощённый</option>
            <option value="https://www.bensound.com/bensound-music/bensound-ukulele.mp3">Песня 2</option>
            <option value="https://www.bensound.com/bensound-music/bensound-sunny.mp3">Песня 3</option>
        </select>
        <button onclick="toggleMusic()">Включить/Выключить музыку</button>
    </div>
</div>

<script>
    // Тексты признания
    const loveMessages = [
        "Ты — мое сердце!",
        "Без тебя мир был бы серым!",
        "Ты — мое вдохновение!",
        "Ты приносишь счастье в мою жизнь!",
        "Милана, я всегда буду рядом!",
        "Ты — моя радость!",
        "С тобой каждый день — праздник!",
        "Ты — мой солнечный свет!",
        "С тобой навсегда!",
        "Ты — моя мечта!"
    ];

    // Функция для смены текста признаний
    let currentIndex = 0;
    setInterval(() => {
        document.getElementById('loveText').textContent = loveMessages[currentIndex];
        currentIndex = (currentIndex + 1) % loveMessages.length;
    }, 6000);

    // Функция для показа сердечек при нажатии на кнопку
    function showHearts() {
        for (let i = 0; i < 10; i++) { // Создаем 10 сердечек
            const heart = document.createElement('div');
            heart.classList.add('heart');
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.animationDuration = Math.random() * 3 + 2 + 's'; // Случайная скорость падения
            document.getElementById('heartsContainer').appendChild(heart);
            setTimeout(() => {
                heart.remove();
            }, 5000); // Удаляем сердечко через 5 секунд
        }
        document.getElementById('motSong').play(); // Включаем песню при нажатии
    }

    // Функция для открытия модального окна
    function openModal() {
        document.getElementById('settingsModal').style.display = 'block';
    }

    // Функция для закрытия модального окна
    function closeModal() {
        document.getElementById('settingsModal').style.display = 'none';
    }

    // Функция для смены цвета фона
    function changeBackgroundColor(color) {
        document.body.style.background = color;
    }

    // Функция для смены песни
    function changeSong(songUrl) {
        const motSong = document.getElementById('motSong');
        motSong.src = songUrl;
        motSong.load();
        motSong.play(); // Включаем новую песню
    }

    // Функция для переключения музыки
    function toggleMusic() {
        const motSong = document.getElementById('motSong');
        if (motSong.paused) {
            motSong.play();
        } else {
            motSong.pause();
        }
    }
</script>

</body>
</html>
