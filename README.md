SNAKE GAME CODE
from tkinter import *
import random

# Constants
GAME_WIDTH = 700
GAME_HEIGHT = 700
SPEED = 200
SPACE_SIZE = 50
BODY_PARTS = 3
SNAKE_COLOR = "#00FF00"
FOOD_COLOR = "#FF0000"
BACKGROUND_COLOR = "#000000"

class Snake:
    def __init__(self):
        self.body_size = BODY_PARTS
        self.coordinates = []
        self.squares = []

        for i in range(0, BODY_PARTS):
            self.coordinates.append([0, 0])

        for x, y in self.coordinates:
            square = canvas.create_rectangle(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=SNAKE_COLOR, tag="snake")
            self.squares.append(square)

class Food:
    def __init__(self):
        while True:
            x = random.randint(0, (GAME_WIDTH // SPACE_SIZE) - 1) * SPACE_SIZE
            y = random.randint(0, (GAME_HEIGHT // SPACE_SIZE) - 1) * SPACE_SIZE
            if [x, y] not in snake.coordinates:
                break
        
        self.coordinates = [x, y]
        canvas.create_oval(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=FOOD_COLOR, tag="food")

def next_turn(snake, food):
    x, y = snake.coordinates[0]

    if direction == "up":
        y -= SPACE_SIZE
    elif direction == "down":
        y += SPACE_SIZE
    elif direction == "left":
        x -= SPACE_SIZE
    elif direction == "right":
        x += SPACE_SIZE

    snake.coordinates.insert(0, [x, y])
    square = canvas.create_rectangle(x, y, x + SPACE_SIZE, y + SPACE_SIZE, fill=SNAKE_COLOR, tag="snake")
    snake.squares.insert(0, square)

    if x == food.coordinates[0] and y == food.coordinates[1]:
        global score
        score += 1
        label.config(text="Score: {}".format(score))
        canvas.delete("food")
        food = Food()
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
    if new_direction == 'left' and direction != 'right':
        direction = new_direction
    elif new_direction == 'right' and direction != 'left':
        direction = new_direction
    elif new_direction == 'up' and direction != 'down':
        direction = new_direction
    elif new_direction == 'down' and direction != 'up':
        direction = new_direction

def check_collisions(snake):
    x, y = snake.coordinates[0]

    if x < 0 or x >= GAME_WIDTH or y < 0 or y >= GAME_HEIGHT:
        return True

    for body_part in snake.coordinates[1:]:
        if x == body_part[0] and y == body_part[1]:
            return True

    return False

def game_over():
    canvas.delete(ALL)
    canvas.create_text(GAME_WIDTH / 2, GAME_HEIGHT / 2,
                       font=('consolas', 50), text="GAME OVER",
                       fill="red", tag="gameover")

# Initialize window
window = Tk()
window.title("Snake Game")
window.resizable(False, False)

score = 0
direction = 'down'

label = Label(window, text="Score: {}".format(score), font=('consolas', 40))
label.pack()

canvas = Canvas(window, bg=BACKGROUND_COLOR, height=GAME_HEIGHT, width=GAME_WIDTH)
canvas.pack()

window.update()  # Update window to get correct dimensions

window_width = window.winfo_width()
window_height = window.winfo_height()
screen_width = window.winfo_screenwidth()
screen_height = window.winfo_screenheight()

x = int((screen_width / 2) - (window_width / 2))
y = int((screen_height / 2) - (window_height / 2))
window.geometry(f"{window_width}x{window_height}+{x}+{y}")

window.bind('<Left>', lambda event: change_direction('left'))
window.bind('<Right>', lambda event: change_direction('right'))
window.bind('<Up>', lambda event: change_direction('up'))
window.bind('<Down>', lambda event: change_direction('down'))

snake = Snake()
food = Food()
next_turn(snake, food)

window.mainloop()









TIC TAC TOE GAME CODE
from tkinter import *
import random

def next_turn(row,column):
    
    global player

    if buttons[row][column]['text']=="" and check_winner() is False:

        if player==players[0]:

            buttons[row][column]['text']=player

            if check_winner() is False:
                player=players[1]
                label.config(text=(players[1]+"turn"))

            elif check_winner() is True:
                label.config(text=(players[0]+"wins"))

            elif check_winner()== "Tie":
                label.config(text=("Tie!!"))   

        else:
           
            buttons[row][column]['text']=player

            if check_winner() is False:
                player=players[0]
                label.config(text=(players[0]+"turn"))

            elif check_winner() is True:
                label.config(text=(players[1]+"wins"))

            elif check_winner()== "Tie":
                label.config(text=("Tie!!"))   



def check_winner():
    
    for row in range(3):
        if buttons[row][0]['text']==buttons[row][1]['text']==buttons[row][2]['text'] != "":
            buttons[row][0].config(bg="green")
            buttons[row][1].config(bg="green")
            buttons[row][2].config(bg="green")
            return True
        
    for column in range(3):
        if buttons[0][column]['text']==buttons[1][column]['text']==buttons[2][column]['text'] != "":
            buttons[0][column].config(bg="green")
            buttons[1][column].config(bg="green")
            buttons[2][column].config(bg="green")
            return True
        
    if buttons[0][0]['text']== buttons[1][1]['text']== buttons[2][2]['text'] !="":
        buttons[0][0].config(bg="green")
        buttons[1][1].config(bg="green")
        buttons[2][2].config(bg="green")
        return True
    
    elif buttons[0][2]['text']== buttons[1][1]['text']== buttons[2][0]['text'] !="":
        buttons[0][2].config(bg="green")
        buttons[1][1].config(bg="green")
        buttons[2][0].config(bg="green")
        return True
    
    elif empty_spaces() is False:
        for row in range(3):
            for column in range(3):
                buttons[row][column].config(bg="yellow")
        return "Tie"
    
    else:
        return False

def empty_spaces():
    
    spaces = 9

    for row in range(3):
        for column in range(3):
            if buttons[row][column]['text'] != "":
                spaces -=1

    if spaces==0:
        return False
    else:
        return True

def new_game():
    
    global player

    player = random.choice(players)

    label.config(text=player+" turn")

    for row in range(3):
        for column in range(3):
            buttons[row][column].config(text="",bg="#F0F0F0")


window = Tk()
window.title("Tic-Tac-Toe")
players = ["X","O"]
player = random.choice(players)
buttons = [[0,0,0],
           [0,0,0],
           [0,0,0]]

label= Label(text=player + " turn", font=('consolas',40))
label.pack(side="top")

reset_button = Button(text="restart", font=('consolas',20), command=new_game)
reset_button.pack(side="top")

frame = Frame(window)
frame.pack()

for row in range(3):
    for column in range(3):
        buttons[row][column]= Button(frame, text="", font=('consolas',40), width=5, height=2, 
                                     command=lambda row=row, column=column: next_turn(row,column))
        buttons[row][column].grid(row=row, column=column)

window.mainloop()









TEXT EDITOR PROGRAM
import os
from tkinter import *
from tkinter import filedialog, colorchooser, font
from tkinter.messagebox import*
from tkinter.filedialog import*

def change_color():
    color = colorchooser.askcolor(title="pick a color..... or else")
    text_area.config(fg=color[1])

def change_font(*args):
    text_area.config(font=(font_name.get(), size_box.get()))

def new_file():
    window.title("Untitled")
    text_area.delete(1.0,END)





def open_file():
    file = askopenfilename(defaultextension=".txt", 
                           file=[("All files","*.*"),
                                 ("Text Documents","*.txt")])
    
    try:
        window.title(os.path.basename(file))
        text_area.delete(1.0, END)

        file = open(file,"r")

        text_area.insert(1.0, file.read())

    except Exception:
        print("couldn't read file")

    finally:
        file.close() 


def save_file():
    file = filedialog.asksaveasfilename(initialfile='untitled.txt',
                                        defaultextension=".txt",
                                        filetypes=[("All Files","*.*"),
                                                    ("Text Documents","*.txt")])
    
    if file is None:
        return
    
    else:
        try:
            window.title(os.path.basename(file))
            file = open(file, "w")

            file.write(text_area.get(1.0,END))

        except Exception:
            print("Couldn't save file")

        finally:
            file.close()



def cut():
    text_area.event_generate("<<Cut>>")


def copy():
    text_area.event_generate("<<Copy>>")


def paste():
    text_area.event_generate("<<Paste>>")


def about():
    showinfo("About this program","This is a program written by YOUUU!!")


def quit():
    window.destroy()


window = Tk()
window.title("Text editor program")
file= None

window_width = 500
window_height = 500
screen_width = window.winfo_screenwidth()
screen_height = window.winfo_screenheight()

x = int((screen_width / 2) - (window_width / 2))
y = int((screen_height / 2) - (window_height / 2))

window.geometry("{}x{}+{}+{}".format(window_width, window_height, x, y))

font_name = StringVar(window)
font_name.set("Arial")

font_size = StringVar(window)
font_size.set("25")

text_area = Text(window, font=(font_name.get(), font_size.get()))

scroll_bar = Scrollbar(text_area)
window.grid_rowconfigure(0, weight=1)
window.grid_columnconfigure(0, weight=1)
text_area.grid(sticky= N + E + S + W)

frame = Frame(window)
frame.grid()

color_button = Button(frame, text="color", command=change_color)
color_button.grid(row=0, column=0)

font_box = OptionMenu(frame, font_name, *font.families(),command=change_font)
font_box.grid(row=0, column=1)

size_box = Spinbox(frame, from_=1, to=100, textvariable=font_size, command=change_font)
size_box.grid(row=0,column=2)

scroll_bar.pack(side=RIGHT, fill=Y)
text_area.config(yscrollcommand=scroll_bar.set)

menu_bar = Menu(window)
window.config(menu=menu_bar)

file_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="File",menu=file_menu)
file_menu.add_command(label="New", command=new_file)
file_menu.add_command(label="Open", command=open_file)
file_menu.add_command(label="Save", command=save_file)
file_menu.add_separator()
file_menu.add_command(label="Exit", command=quit)

edit_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="Edit",menu=edit_menu)
edit_menu.add_command(label="Cut", command=cut)
edit_menu.add_command(label="Copy", command=copy)
edit_menu.add_command(label="Paste", command=paste)

help_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="Help",menu=help_menu)
help_menu.add_command(label="About", command=about)


window.mainloop()









CALCULATOR PROGRAM
from tkinter import *

def button_press(num):
    
    global equation_text

    equation_text = equation_text +str(num)

    equation_label.set(equation_text)

def equals():
    
    global equation_text

    try: 

        total = str(eval(equation_text))

        equation_label.set(total)

        equation_text = total

    except SyntaxError:

        equation_label.set("syntax error")

        equation_text = ""

    except ZeroDivisionError:

        equation_label.set("arithmetic error")

        equation_text = ""

def clear():
    
    global equation_text

    equation_label.set("")

    equation_text =""


window = Tk()
window.title("calculator program")
window.geometry("500x500")

equation_text = ""

equation_label = StringVar()

label = Label(window, textvariable=equation_label, font=('consola',20), bg="white", width=24, height=2)
label.pack()

frame = Frame(window)
frame.pack()

button1 = Button(frame,text=1,height=4,width=9,font=35,
                 command=lambda:button_press(1)) 
button1.grid(row=0, column=0)

button2 = Button(frame,text=2,height=4,width=9,font=35,
                 command=lambda:button_press(2)) 
button2.grid(row=0, column=1)

button3 = Button(frame,text=3,height=4,width=9,font=35,
                 command=lambda:button_press(3)) 
button3.grid(row=0, column=2)

button4 = Button(frame,text=4,height=4,width=9,font=35,
                 command=lambda:button_press(4)) 
button4.grid(row=1, column=0)

button5 = Button(frame,text=5,height=4,width=9,font=35,
                 command=lambda:button_press(5)) 
button5.grid(row=1, column=1)

button6 = Button(frame,text=6,height=4,width=9,font=35,
                 command=lambda:button_press(6)) 
button6.grid(row=1, column=2)

button7 = Button(frame,text=7,height=4,width=9,font=35,
                 command=lambda:button_press(7)) 
button7.grid(row=2, column=0)

button8 = Button(frame,text=8,height=4,width=9,font=35,
                 command=lambda:button_press(8)) 
button8.grid(row=2, column=1)

button9 = Button(frame,text=9,height=4,width=9,font=35,
                 command=lambda:button_press(9)) 
button9.grid(row=2, column=2)

button0 = Button(frame,text=0,height=4,width=9,font=35,
                 command=lambda:button_press(0)) 
button0.grid(row=3, column=0)

plus = Button(frame,text='+',height=4,width=9,font=35,
                 command=lambda:button_press('+')) 
plus.grid(row=0, column=3)

minus = Button(frame,text='-',height=4,width=9,font=35,
                 command=lambda:button_press('-')) 
minus.grid(row=1, column=3)

multiply = Button(frame,text='*',height=4,width=9,font=35,
                 command=lambda:button_press('*')) 
multiply.grid(row=2, column=3)

divide = Button(frame,text='/',height=4,width=9,font=35,
                 command=lambda:button_press('/')) 
divide.grid(row=3, column=3)

equal = Button(frame,text='=',height=4,width=9,font=35,
                 command=equals) 
equal.grid(row=3, column=2)

decimal = Button(frame,text='.',height=4,width=9,font=35,
                 command=lambda:button_press('.')) 
decimal.grid(row=3, column=1)

clear = Button(frame,text='clear',height=4,width=12,font=35,
                 command=clear) 
clear.grid(row=4, column=1)

window.mainloop()






