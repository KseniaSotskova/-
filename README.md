#импортируем trkinter и рандом
from tkinter import Tk, Canvas
import random

# задаем параметры окна Globals
WIDTH = 800
HEIGHT = 500
SEG_SIZE = 20
IN_GAME = True


# Создание шарика
def create_block():
        global BLOCK
        posx = SEG_SIZE * random.randint(1, (WIDTH-SEG_SIZE) / SEG_SIZE)
        posy = SEG_SIZE * random.randint(1, (HEIGHT-SEG_SIZE) / SEG_SIZE)
        BLOCK = c.create_oval(posx, posy,
                          posx+SEG_SIZE, posy+SEG_SIZE,
                          fill="orange")

#Создание логики игры
def main():
    global IN_GAME
    if IN_GAME:
        s.move()
        head_coords = c.coords(s.segments[-1].instance)
        x1, y1, x2, y2 = head_coords
        # столкновение с полями
        if x2 > WIDTH or x1 < 0 or y1 < 0 or y2 > HEIGHT:
            IN_GAME = False
        # Съедание шарика
        elif head_coords == c.coords(BLOCK):
            s.add_segment()
            c.delete(BLOCK)
            create_block()
        # Самопересечение
        else:
            for index in range(len(s.segments)-1):
                if head_coords == c.coords(s.segments[index].instance):
                    IN_GAME = False
        root.after(100, main)
    # Not IN_GAME -> stop game and print message
    else:
        set_state(restart_text, 'normal')
        set_state(game_over_text, 'normal')

#создание сегмента змеи
class Segment(object):
    def init(self, x, y):
        self.instance = c.create_rectangle(x, y,
                                           x+SEG_SIZE, y+SEG_SIZE,
                                           fill="white")

#создание змеи
class Snake(object):
    def init(self, segments):
        self.segments = segments
        # возможные движения
        self.mapping = {"Down": (0, 1), "Right": (1, 0),
                        "Up": (0, -1), "Left": (-1, 0)}
        # начальное направление движения
        self.vector = self.mapping["Right"]

    def move(self):
        #движение змеи в заданном направлении
        for index in range(len(self.segments)-1):
            segment = self.segments[index].instance
            x1, y1, x2, y2 = c.coords(self.segments[index+1].instance)
            c.coords(segment, x1, y1, x2, y2)

        x1, y1, x2, y2 = c.coords(self.segments[-2].instance)
        c.coords(self.segments[-1].instance,
                 x1+self.vector[0]*SEG_SIZE, y1+self.vector[1]*SEG_SIZE,
                 x2+self.vector[0]*SEG_SIZE, y2+self.vector[1]*SEG_SIZE)

    def add_segment(self):
        #добавление сегмета змеи
        last_seg = c.coords(self.segments[0].instance)
        x = last_seg[2] - SEG_SIZE
        y = last_seg[3] - SEG_SIZE
        self.segments.insert(0, Segment(x, y))

    def change_direction(self, event):
        #смена направления движения
        if event.keysym in self.mapping:
            self.vector = self.mapping[event.keysym]

    def reset_snake(self):
        for segment in self.segments:
            c.delete(segment.instance)


def set_state(item, state):
    c.itemconfigure(item, state=state)


def clicked(event):
    global IN_GAME
    s.reset_snake()
    IN_GAME = True
    c.delete(BLOCK)
    c.itemconfigure(restart_text, state='hidden')
    c.itemconfigure(game_over_text, state='hidden')
    start_game()


def start_game():
    global s
    create_block()
    s = create_snake()
    # реакция на нажатие клавиатуры
    c.bind("<KeyPress>", s.change_direction)
    main()


def create_snake():
    # создание сегмента змеи
    segments = [Segment(SEG_SIZE, SEG_SIZE),
                Segment(SEG_SIZE*2, SEG_SIZE),
                Segment(SEG_SIZE*3, SEG_SIZE)]
    return Snake(segments)


# Настройка окна
root = Tk()
root.title("PythonicWay Snake")


c = Canvas(root, width=WIDTH, height=HEIGHT, bg="#DCDCDC")
c.grid()
# считывание нажатий клавиатуры
c.focus_set()
game_over_text = c.create_text(WIDTH/2, HEIGHT/2, text="GAME OVER!",
                               font='Arial 20', fill='red',











                               Код представляет собой простую игру "Змейка" на Python с использованием библиотеки Tkinter для создания графического интерфейса.

Давайте рассмотрим каждую строчку кода и объясним, как работает программа:

1. from tkinter import Tk, Canvas: Импортирует классы Tk и Canvas из библиотеки tkinter. Tk используется для создания главного окна приложения, а Canvas - для создания графического холста внутри окна.

2. import random: Импортирует модуль random, который используется для генерации случайных чисел.

3. WIDTH = 800: Задает ширину окна игры.

4. HEIGHT = 500: Задает высоту окна игры.

5. SEG_SIZE = 20: Задает размер сегмента змейки.

6. IN_GAME = True: Устанавливает флаг, указывающий, что игра находится в активном состоянии.

8. def create_block():: Определяет функцию create_block, которая создает новый шарик в случайном месте на игровом поле.

9. global BLOCK: Объявляет переменную BLOCK как глобальную, чтобы она была доступна из других функций.

10. posx = SEG_SIZE * random.randint(1, (WIDTH-SEG_SIZE) / SEG_SIZE): Генерирует случайную позицию для шарика по оси X.

11. posy = SEG_SIZE * random.randint(1, (HEIGHT-SEG_SIZE) / SEG_SIZE): Генерирует случайную позицию для шарика по оси Y.

12. BLOCK = c.create_oval(posx, posy, posx+SEG_SIZE, posy+SEG_SIZE, fill="orange"): Создает шарик на игровом поле с заданными координатами и цветом.

14. def main():: Определяет функцию main, которая представляет основную логику игры.

15. global IN_GAME: Объявляет переменную IN_GAME как глобальную, чтобы она была доступна из функции.

16. if IN_GAME:: Проверяет, что игра находится в активном состоянии.

17. s.move(): Вызывает метод move() объекта s, который отвечает за движение змейки.

19. head_coords = c.coords(s.segments[-1].instance): Получает координаты головы змейки.

20. x1, y1, x2, y2 = head_coords: Разделяет координаты головы змейки на отдельные переменные.

22. if x2 > WIDTH or x1 < 0 or y1 < 0 or y2 > HEIGHT:: Проверяет, столкнулась ли змейка с границами игрового поля.

23. IN_GAME = False: Устанавливает флаг IN_GAME в значение False, чтобы остановить игру.

25. elif head_coords == c.coords(BLOCK):: Проверяет, столкнулась ли змейка с шариком.

26. s.add_segment(): Добавляет новый сегмент змейки.

27. c.delete(BLOCK): Удаляет шарик с игрового поля.

28. create_block(): Создает новый шарик.

30. else:: Если змейка не столкнулась с границами или шариком, то проверяет, столкнулась ли змейка с самой собой.

31. for index in range(len(s.segments)-1):: Проходит по всем сегментам змейки, кроме головы.

32. if head_coords == c.coords(s.segments[index].instance):: Проверается, пересекается ли голова змейки с текущим сегментом.

34. root.after(100, main): Устанавливает задержку в 100 миллисекунд и вызывает функцию main для продолжения игры.

37. class Segment(object):: Определяет класс Segment, который представляет один сегмент змейки.

38. def __init__(self, x, y):: Определяет конструктор класса Segment, который принимает координаты x и y для создания сегмента змейки.

39. self.instance = c.create_rectangle(x, y, x+SEG_SIZE, y+SEG_SIZE, fill="white"): Создает прямоугольник (сегмент змейки) на игровом поле с заданными координатами и цветом.

42. class Snake(object):: Определяет класс Snake, который представляет змейку.

43. def __init__(self, segments):: Определяет конструктор класса Snake, который принимает список сегментов segments в качестве параметра.

44. self.segments = segments: Присваивает список сегментов змейки экземпляру класса.

45. self.mapping = {"Down": (0, 1), "Right": (1, 0), "Up": (0, -1), "Left": (-1, 0)}: Определяет словарь mapping, который содержит возможные направления движения змейки в виде пар (dx, dy), где dx - изменение координаты X, а dy - изменение координаты Y.

46. self.vector = self.mapping["Right"]: Устанавливает начальное направление движения змейки вправо.

49. def move(self):: Определяет метод move(), который от








Да, в представленном коде присутствуют классы. Вот список классов, которые определены в коде:

1. Класс Segment: Представляет сегмент змейки. Он имеет атрибут instance, который представляет сегмент на игровом поле.

2. Класс Snake: Представляет змейку. Он имеет атрибут segments, который содержит список сегментов змейки, и атрибут vector, который определяет текущее направление движения змейки.

Оба класса имеют методы, которые определяют их поведение в игре. Например, класс Snake имеет методы move() и add_segment(), которые отвечают за движение змейки и добавление нового сегмента соответственно.






Чтобы запустить игру "Змейка" из проекта, хранящегося на GitHub, вам понадобится выполнить следующие шаги:

1. Склонируйте репозиторий: Скопируйте URL репозитория с игрой "Змейка" на GitHub. Затем откройте командную строку или терминал на вашем компьютере и используйте команду git clone с указанием скопированного URL для клонирования репозитория на ваш локальный компьютер.

   Пример:
   `   git clone https://github.com/username/repository.git
   

2. Перейдите в директорию проекта: В командной строке или терминале перейдите в директорию, в которую был склонирован репозиторий игры "Змейка".

   Пример:
   `   cd repository
   

3. Установите зависимости (если необходимо): Если в проекте используются дополнительные зависимости или библиотеки, убедитесь, что они установлены на вашем компьютере. Для установки зависимостей обычно используется файл requirements.txt или setup.py. Выполните команду установки зависимостей в вашем виртуальном окружении или глобально.

   Пример:
   `   pip install -r requirements.txt
   

4. Запустите игру: Выполните команду для запуска игры. В зависимости от структуры проекта и языка программирования, используемого в игре, команда запуска может отличаться.

   Пример (Python):
   `   python game.py
   

   Пример (JavaScript):
   `   node game.js
   

   Убедитесь, что вы запускаете игровой скрипт, указанный в проекте.

   После выполнения этих шагов игра "Змейка" должна запуститься на вашем компьютере.
