<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Звёздный профиль · веб-разработчик</title>
  <style>
    /* Обнуляем отступы и делаем фон тёмным */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: #0b0e1a;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Inter', 'Segoe UI', system-ui, sans-serif;
      overflow: hidden; /* чтобы падающие звёзды не вызывали скролл */
      position: relative;
    }

    /* ----- Падающие звёздочки (анимация) ----- */
    .star {
      position: absolute;
      top: -10%;
      color: #fff;
      font-size: 1.8rem;
      pointer-events: none;
      user-select: none;
      opacity: 0.9;
      filter: drop-shadow(0 0 6px #aaccff);
      animation: fall linear infinite;
      z-index: 1;
      /* Начальная прозрачность для плавности */
      will-change: transform, opacity;
    }

    @keyframes fall {
      0% {
        transform: translateY(0) rotate(0deg);
        opacity: 0.9;
      }
      70% {
        opacity: 0.9;
      }
      100% {
        transform: translateY(110vh) rotate(360deg);
        opacity: 0;
      }
    }

    /* ----- Основной профиль (стеклянная карточка) ----- */
    .profile-card {
      position: relative;
      z-index: 10;
      background: rgba(18, 25, 45, 0.55);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid rgba(255, 255, 255, 0.15);
      border-radius: 2.5rem;
      padding: 3rem 4rem;
      text-align: center;
      box-shadow: 0 25px 50px -8px rgba(0, 0, 0, 0.8), 0 0 0 1px rgba(200, 220, 255, 0.1) inset;
      max-width: 90vw;
      width: fit-content;
      color: #f0f4ff;
      transition: transform 0.2s ease;
    }

    .profile-card:hover {
      transform: scale(1.01);
    }

    /* Имя / ник */
    .name {
      font-size: 2.8rem;
      font-weight: 700;
      letter-spacing: -0.02em;
      background: linear-gradient(135deg, #ffffff 0%, #b8d0ff 80%);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      text-shadow: 0 0 30px rgba(150, 200, 255, 0.6);
      margin-bottom: 1.2rem;
      white-space: nowrap;
    }

    /* Контейнер для печатающегося текста */
    .typing-container {
      font-size: 1.8rem;
      font-weight: 400;
      color: #d0e0ff;
      min-height: 4rem;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 0.2rem;
      border-right: 3px solid #9bbaff;
      padding-right: 0.3rem;
      white-space: nowrap;
      animation: blink-caret 0.8s step-end infinite;
      margin: 0 auto;
      width: fit-content;
    }

    /* Мигающий курсор */
    @keyframes blink-caret {
      from, to { border-color: transparent; }
      50% { border-color: #9bbaff; }
    }

    /* Аватарка / иконка разработчика (опционально) */
    .avatar {
      font-size: 4.5rem;
      margin-bottom: 0.8rem;
      filter: drop-shadow(0 0 12px #7aa5ff);
    }

    /* Небольшая декоративная линия */
    .glow-line {
      width: 80px;
      height: 2px;
      background: linear-gradient(90deg, transparent, #7aa5ff, #c0d4ff, #7aa5ff, transparent);
      margin: 1.4rem auto 1.8rem;
      border-radius: 100%;
      box-shadow: 0 0 15px #7aa5ff;
    }

    /* Адаптив для маленьких экранов */
    @media (max-width: 600px) {
      .profile-card {
        padding: 2rem 1.5rem;
        border-radius: 2rem;
      }
      .name {
        font-size: 2rem;
        white-space: normal;
      }
      .typing-container {
        font-size: 1.3rem;
        min-height: 3.2rem;
      }
      .avatar {
        font-size: 3.5rem;
      }
    }

    /* Стили для звёздочек разного размера/яркости */
    .star:nth-child(odd) {
      filter: drop-shadow(0 0 10px #c2dbff);
    }
    .star:nth-child(even) {
      filter: drop-shadow(0 0 6px #d9e8ff);
    }
  </style>
</head>
<body>
  <!-- Падающие звёздочки будут сгенерированы JavaScript -->
  
  <div class="profile-card">
    <!-- Аватар (можно заменить на своё изображение) -->
    <div class="avatar">🌌</div>
    
    <div class="name">Алексей Звёздный</div>
    <div class="glow-line"></div>
    
    <!-- Сюда печатается и удаляется текст -->
    <div class="typing-container" id="typingText"></div>
  </div>

  <script>
    (function() {
      // ---------- 1. ГЕНЕРАЦИЯ ПАДАЮЩИХ ЗВЁЗД ----------
      const starsContainer = document.body; // звёзды будут на body, позади карточки
      const STAR_COUNT = 35; // количество звёзд на экране

      // Символы звёзд: ★, ✦, ✧, ⭐, 🌟, ⋆, ✫, ✬
      const starSymbols = ['★', '✦', '✧', '⋆', '✫', '✬', '⭐'];

      // Функция создания одной звезды
      function createStar() {
        const star = document.createElement('div');
        star.className = 'star';
        
        // Случайный символ
        const symbol = starSymbols[Math.floor(Math.random() * starSymbols.length)];
        star.textContent = symbol;
        
        // Случайный размер (0.8rem – 2.5rem)
        const size = 0.8 + Math.random() * 1.7;
        star.style.fontSize = size + 'rem';
        
        // Случайное начальное положение по X (от 0 до 100vw)
        const left = Math.random() * 100;
        star.style.left = left + '%';
        
        // Случайная задержка анимации (0 – 5s), чтобы звёзды падали не одновременно
        const delay = Math.random() * 5;
        star.style.animationDelay = delay + 's';
        
        // Случайная длительность анимации (4 – 9 секунд)
        const duration = 4 + Math.random() * 5;
        star.style.animationDuration = duration + 's';
        
        // Немного разная прозрачность
        star.style.opacity = 0.5 + Math.random() * 0.5;
        
        // Случайный поворот для разнообразия (но т.к. rotate в анимации, можно добавить начальный)
        // star.style.transform = `rotate(${Math.random() * 360}deg)`; // не обязательно
        
        starsContainer.appendChild(star);
      }

      // Создаём звёзды
      for (let i = 0; i < STAR_COUNT; i++) {
        createStar();
      }

      // Дополнительно: иногда добавляем новые звёзды взамен улетевших (не обязательно, 
      // т.к. анимация бесконечная, но для долгого использования можно обновлять)
      // Но у нас анимация infinite, поэтому звёзды постоянно падают заново.

      // ---------- 2. ПЕЧАТАЮЩИЙСЯ ТЕКСТ (туда-сюда) ----------
      const typingElement = document.getElementById('typingText');
      
      // Фразы о том, что я веб-разработчик (на английском)
      const phrases = [
        "I'm a Web Developer",
        "Frontend & Backend",
        "Full-Stack Engineer",
        "I build web experiences",
        "React · Node · TypeScript",
        "Let's create something amazing"
      ];
      
      let phraseIndex = 0;       // текущая фраза
      let charIndex = 0;        // текущий символ в фразе
      let isDeleting = false;   // режим удаления или печати
      let currentText = '';     // отображаемый текст
      
      // Скорости (мс)
      const TYPING_SPEED = 100;      // печать символа
      const DELETING_SPEED = 60;     // удаление символа
      const PAUSE_BETWEEN = 2000;    // пауза перед удалением/печатью
      
      function typeLoop() {
        const currentPhrase = phrases[phraseIndex];
        
        if (isDeleting) {
          // Удаляем символ
          currentText = currentPhrase.substring(0, charIndex - 1);
          charIndex--;
        } else {
          // Добавляем символ
          currentText = currentPhrase.substring(0, charIndex + 1);
          charIndex++;
        }
        
        // Обновляем текст в контейнере
        typingElement.textContent = currentText;
        
        // Определяем следующую задержку
        let delay = isDeleting ? DELETING_SPEED : TYPING_SPEED;
        
        // Логика переключения режимов
        if (!isDeleting && charIndex === currentPhrase.length) {
          // Закончили печатать фразу — пауза и начинаем удалять
          delay = PAUSE_BETWEEN;
          isDeleting = true;
        } else if (isDeleting && charIndex === 0) {
          // Закончили удалять — переходим к следующей фразе
          isDeleting = false;
          phraseIndex = (phraseIndex + 1) % phrases.length;
          delay = 400; // небольшая пауза перед новой фразой
        } else if (isDeleting && charIndex === 0) {
          // (уже обработано выше)
        }
        
        // Дополнительная проверка: если удалили всё и isDeleting = true, 
        // но charIndex = 0 — переключаем на новую фразу (сработает условие выше)
        
        // Запускаем следующий цикл
        setTimeout(typeLoop, delay);
      }
      
      // Небольшая задержка перед стартом, чтобы карточка появилась
      setTimeout(() => {
        // Начинаем с первой фразы, charIndex = 0, isDeleting = false
        charIndex = 0;
        isDeleting = false;
        phraseIndex = 0;
        currentText = '';
        typingElement.textContent = '';
        typeLoop();
      }, 500);
      
    })();
  </script>
</body>
</html>
