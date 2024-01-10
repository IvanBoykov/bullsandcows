# Игра "Быки и коровы"
Источник: https://ru.wikipedia.org/wiki/Быки_и_коровы
## Задание 1:
Написать игру Быки и Коровы с использованием паттернов проектирования. В игре должна быть возможность читать/выводить данные как с консоли так и из файла. При этом должна быть возможность изменить длину исходного числа и включить/выключить повторяющиеся цифры. К игре нужно дополнительно написать несколько юнит тестов.
Классические вариант игры: В классическом варианте игра рассчитана на двух игроков. Каждый из игроков задумывает и записывает тайное 4-значное число с неповторяющимися цифрами. Игрок, который начинает игру по жребию, делает первую попытку отгадать число. Попытка — это 4-значное число с неповторяющимися цифрами, сообщаемое противнику. Противник сообщает в ответ, сколько цифр угадано без совпадения с их позициями в тайном числе (то есть количество коров) и сколько угадано вплоть до позиции в тайном числе (то есть количество быков). Например:
Задумано тайное число «3219».
Попытка: «2310».
Результат: две «коровы» (две цифры: «2» и «3» — угаданы на неверных позициях) и один «бык» (одна цифра «1» угадана вплоть до позиции).
Игроки начинают по очереди угадывать число соперника. Побеждает тот, кто угадает число первым, при условии, что он не начинал игру. Если же отгадавший начинал игру — его противнику предоставляется последний шанс угадать последовательность.
При игре против компьютера игрок вводит комбинации одну за другой, пока не отгадает всю последовательность.
Выполняется в /game
## Задание 2:
Быки и Коровы - REST API

Подготовка: создаем новый java проект используя spring boot initializer. В качестве сборщика используем maven. В зависимости добавляем Spring Web, Spring Data JPA. После скачивания в зависимости дополнительно добавляем скомпилированный пакет из прошлого задания.
Задание: нужно написать REST API для взаимодействия с нашей игрой. Описания методов ниже:
1. POST /register
Создает нового пользователя с логином и паролем. Проверки полей приветствуются, пароль можно не шифровать, но если у кого-то будет желание, то можно добавить шифрование и дополнительно посолить его.
Запрос:
{
    "login": "login1"
    "password": "password1"
}
Ответ:
{
    "status": "OK",
    "errorMessage": null
}
Ошибка:
{
    "status": "FAIL",
    "errorMessage": "пароль не может быть меньше 6 символов"
}
2 POST /login
Авторизация пользователя для дальнейшей работы с ним.
Запрос:
{
    "login": "login1"
    "password": "password1"
}
Ответ:
{
    "session": "dc47979f-7668-4037-8637-d1a761969b72"
    "status": "OK",
    "errorMessage": null
}
Ошибка:
{
    "status": "FAIL",
    "errorMessage": "неверный пароль"
}
3 POST /game/create
Создает игровую сессию для игры с человеком. Правила можно ограничить 2мя значениями classic - класическая, repeat - игра с повторениями цифр. Также можно добавить возможность создавать игру с компьютером.
Запрос:
{
    "number": "3219",
    "gameRules": "classic"
    "session": "dc47979f-7668-4037-8637-d1a761969b72"
}
Ответ:
{
    "game-session": "3a2b9b74-a58e-4f4f-b1f0-1df334116151",
    "status": "OK",
    "errorMessage": null
}
Ошибка:
{
    "status": "FAIL",
    "errorMessage": "Повторяются цифры"
}
4 GET /game/3a2b9b74-a58e-4f4f-b1f0-1df334116151/status
Возвращает информацию об игре.
Ответ:
{
    "status": "IN_PROGRESS",
    "attempts": [
        {
            "assumption": "2310",
            "bulls": 1,
            "cows": 2
        },
        {
            "assumption": "2310",
            "bulls": 1,
            "cows": 2
        }
    ]
}
Ошибка:
{
    "status": "FAIL",
    "errorMessage": "Игра не найдена"
}
 1 POST /game/3a2b9b74-a58e-4f4f-b1f0-1df334116151/attempt
Попытаться угадать число.
Запрос:
{
    "number": "2310"
}
Ответ:
{
    "bulls": 1,
    "cows": 2,
    "status": "OK",
    "errorMessage": null
}
Ошибка:
{
    "status": "FAIL",
    "errorMessage": "Игра не найдена"
}
