# game_it-incubator
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zombie</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            background-color: rgba(53, 2, 2, 0.981);
            padding: 30px;
            font-family: Arial, Helvetica, sans-serif;
        }

        button {
            width: 135px;
            padding: 10px 25px;
            background-color: rgba(53, 2, 2, 0.981);
            border: 2px solid white;
            border-radius: 5px;
            color: white
        }

        .game-panel {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px;
            color: white;

            /*border: 2px solid white;*/
            /* Рамка для отображения границ элементов */
        }

        .container {
            display: flex;
            flex-wrap: wrap;
            width: 800px;
            border-radius: 10px;
            margin: 50px auto;
            background-image: url("images/wall.jpg");
            background-size: 100%;
            background-position-y: -255px;
            box-shadow: 0 0 50px 30px rgba(0, 0, 0, 0.5);
            cursor: url("images/aim.svg"), pointer;
        }

        .item {
            box-sizing: border-box;
            /* Учитывать padding и border в расчетах ширины */

            width: 120px;
            height: 120px;
            margin: 10px 20px;
            border-radius: 50%;
            /*border: 2px solid white;*/
            /* Рамка для отображения границ элементов */
        }

        img {
            width: 100%;
            height: 100%;
        }
    </style>
</head>

<body>
    <div class="game-panel">
        <div>
            <button id="sound-btn">SOUND ON</button>
            <button id="start-btn">START</button>
        </div>
        
        <div>
            <div class="counter">HITS:
                <span id="hit-counter">0</span>
            </div>
            <div class="counter">MISS:
                <span id="miss-counter">0</span>
            </div>
        </div>
    </div>
    <div class="container">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
    </div>
    <!-- добавляем элемент звука-->>
    <audio id="sound-bu" src="sounds/bu.mp3" loop></audio>
    <audio id="sound-shot" src="sounds/shot.mp3"></audio>
    <script>
        
        //Появление зомби в случайном месте 
        const items = document.querySelectorAll(".item");
        const zombieImg = document.createElement("img");
        zombieImg.src = "images/zombi-3.png";
        let randomIndex;
        randomIndex = getRandomItemIndex(items);
        

        //Элементы для фонового звука и его включения/выключения
        const bu = document.querySelector("#sound-bu");
        const soundBtn = document.querySelector("#sound-btn");

        //Элементы для выстрел: картинки попадания,
        //звук выстрела, подчёт попаданий
        const hitImage = document.createElement("img");
        hitImage.src = "images/blood.png";
        const shot = document.querySelector("#sound-shot");
        const hitCounter = document.querySelector("#hit-counter");

        //"Флаг" для подсчёта промахов и счётчик промахов
        let hit = true;
        const missCounter = document.querySelector("#miss-counter");

        //Элементыи флаг для включения/выключения игры
        const startBtn = document.querySelector("#start-btn")
        let isStarted = false
        let interval;


        //Вспомогательная функция для нахождения 
        //случайного индекса в массиве
        function getRandomItemIndex(array) {
            return Math.floor(Math.random() * array.length);
        };

        function playGame() {
            //Перемещение зомби
            interval = setInterval(function () {
                //Проверяем "флаг", если было попадание - скидываем на false
                //если был промах - считаем промахи
                if (hit === true) {
                    hit = false;
                } else {
                    missCounter.innerText++;
                }
                //Помещаем нового зомби в случайную ячейку
                randomIndex = getRandomItemIndex(items);
                items[randomIndex].append(zombieImg);
                //Убираем следы крови перед появлением нового зомби
                hitImage.remove()
            }, 2000)
        }


        // Включение и выключение фонового звука, 
        // изменение текст на кнопке
        soundBtn.onclick = function () {
            if (bu.currentTime) {
                bu.pause();
                bu.currentTime = 0;
                soundBtn.innerHTML = "SOUND ON"
            } else {
                bu.play();
                soundBtn.innerHTML = "SOUND OFF"
            }
        }

        //Попадание по зомби


        startBtn.onclick = function () {
            if (!isStarted) {
                isStarted = true
                zombieImg.onclick = function () {
                    //Меняем "флаг" при попадании
                    hit = true
                    hitCounter.innerText++;
                    //Запускаем заново звук выстрела
                    shot.currentTime = 0;
                    shot.play();
                    //Убираем зомби и на его место вставляем кровавый след
                    zombieImg.remove();
                    items[randomIndex].append(hitImage);
                }
                playGame()
                items[randomIndex].append(zombieImg);
                startBtn.innerHTML = "STOP"


            } else {
                isStarted = false
                zombieImg.onclick = null
                clearInterval(interval)
                zombieImg.remove();
                hitImage.remove()
                startBtn.innerHTML = "START"
                missCounter.innerText = 0
                hitCounter.innerText = 0
                
            }
        }


    </script>
</body>

</html>![zombie](https://github.com/AnnMitrakhovich/game_it-incubator/assets/134679287/2a169131-39b6-4f31-9849-d7d9462eee9d)
![wall](https://github.com/AnnMitrakhovich/game_it-incubator/assets/134679287/c6e1e72c-24fa-43b5-8acf-eb79900113fe)
![blood](https://github.com/AnnMitrakhovich/game_it-incubator/assets/134679287/8db149ec-644a-4508-b85c-84a80689f69a)
![aim-empty](https://github.com/AnnMitrakhovich/game_it-incubator/assets/134679287/85db8c21-d08b-4237-a50d-09dda2b3d36d)
![aim](https://github.com/AnnMitrakhovich/game_it-incubator/assets/134679287/54dd6b4e-2c25-40b4-abd5-ccb90cf5fd91)

