This is a python language 

#by MasterPinot
from tkinter import *
import random
import os
import sys

GAME_WIDTH = 800
GAME_HEIGHT = 900
SPEED = 150
SPACE_SIZE = 25
BODY_PARTS = 3
SNAKE_COLOR = "#00FF00"
FOOD_COLOR = "#FF0000"
BACKGROUND_COLOR = "#000000"

direction = "down"  
score = 0  

class Snake:
    def __init__(self, canvas):
        self.canvas = canvas
        self.body_size = BODY_PARTS
        self.coordinates = []
        self.squares = []

        self.generate_snake()

    def generate_snake(self):
        for i in range(0, self.body_size):
            x = (self.body_size - i) * SPACE_SIZE
            y = self.body_size * SPACE_SIZE
            self.coordinates.append([x, y])
            square = self.canvas.create_rectangle(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=SNAKE_COLOR, tag="snake")
            self.squares.append(square)

class Food:
    def __init__(self, canvas):
        self.canvas = canvas
        self.coordinates = []
        self.spawn_food()

    def spawn_food(self):
        x = random.randint(0, (GAME_WIDTH / SPACE_SIZE) - 1) * SPACE_SIZE
        y = random.randint(0, (GAME_HEIGHT / SPACE_SIZE) - 1) * SPACE_SIZE
        self.coordinates = [x, y]
        self.canvas.create_oval(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=FOOD_COLOR, tag="food")

def next_turn(snake, food):
    global direction, score 

    x, y = snake.coordinates[0]

    if direction == "up":
        y -= SPACE_SIZE
    elif direction == "down":
        y += SPACE_SIZE
    elif direction == "left":
        x -= SPACE_SIZE
    elif direction == "right":
        x += SPACE_SIZE

    snake.coordinates.insert(0, (x, y))

    square = canvas.create_rectangle(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=SNAKE_COLOR)  
    snake.squares.insert(0, square)

    if x == food.coordinates[0] and y == food.coordinates[1]:
        score += 1
        label.config(text="Score: {}".format(score))

        canvas.delete("food")
        food.spawn_food()

    else:
         del snake.coordinates[-1]
         canvas.delete(snake.squares[-1])
         del snake.squares[-1]

    if check_collisions(snake):
        game_over()
    else:
        window.after(SPEED, next_turn, snake, food)  


def change_direction(new_direction):
    global direction
    if new_direction == 'left':
        if direction != 'right':
            direction = new_direction
    elif new_direction == 'right':
        if direction != 'left':
            direction = new_direction
    elif new_direction == 'up':
        if direction != 'down':
            direction = new_direction
    elif new_direction == 'down':
        if direction != 'up':
            direction = new_direction

def check_collisions(snake):
    x, y = snake.coordinates[0]

    if x < 0 or x >= GAME_WIDTH or y < 0 or y >= GAME_HEIGHT:
        return True

    for segment in snake.coordinates[1:]:
        if x == segment[0] and y == segment[1]:
            return True

    return False

def retry_game():
    python = sys.executable
    os.execl(python, python, * sys.argv)

def leave_game():
    window.destroy()

def game_over():
    retry_button = Button(window, text="Retry", font=('consolas', 20), command=retry_game)
    retry_button.pack(side=LEFT)
    leave_button = Button(window, text="Leave", font=('consolas', 20), command=leave_game)
    leave_button.pack(side=RIGHT)
    canvas.delete("all")
    canvas.create_text(canvas.winfo_width()/2, canvas.winfo_height()/2, font=('consolas', 70), text="GAME OVER", fill="red", tag="gameover")
    canvas.create_text(canvas.winfo_width()/2, canvas.winfo_height()/2 + 100, font=('consolas', 40), text="Score: {}".format(score), fill="white", tag="gameover")

window = Tk()
window.title("Snake game")

window.attributes("-fullscreen", True)

canvas = Canvas(window, bg=BACKGROUND_COLOR, height=GAME_HEIGHT, width=GAME_WIDTH)
canvas.pack()

snake = Snake(canvas)
food = Food(canvas)

label = Label(window, text="Score: {}".format(score), font=('consolas', 20))
label.pack(side=TOP)

retry_button = Button(window, text="Retry", font=('consolas', 20), command=retry_game)
retry_button.pack(side=LEFT)

leave_button = Button(window, text="Leave", font=('consolas', 20), command=leave_game)
leave_button.pack(side=RIGHT)

window.bind("<Up>", lambda event: change_direction("up"))
window.bind("<Down>", lambda event: change_direction("down"))
window.bind("<Left>", lambda event: change_direction("left"))
window.bind("<Right>", lambda event: change_direction("right"))
window.bind("<w>", lambda event: change_direction("up"))
window.bind("<s>", lambda event: change_direction("down"))
window.bind("<a>", lambda event: change_direction("left"))
window.bind("<d>", lambda event: change_direction("right"))
window.bind("<z>", lambda event: change_direction("up"))
window.bind("<s>", lambda event: change_direction("down"))
window.bind("<q>", lambda event: change_direction("left"))
window.bind("<d>", lambda event: change_direction("right"))

next_turn(snake, food)

window.mainloop()
#by MasterPinot
