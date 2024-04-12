import tkinter as tk
from PIL import Image, ImageTk
import random
import os
import sys

choices = ['Rock', 'Scissors', 'Paper']
colors = ['#F073CF', 'yellow', '#0fb5fc']
userScore = 0
compScore = 0
count = 0
userchoice = '-'

def userInput(user_input):
    global userchoice, count
    userchoice = choices[user_input]
    if userchoice == 'Rock':
        userChoiceImage.configure(image=rockImage)
    elif userchoice == 'Scissors':
        userChoiceImage.configure(image=scissorsImage)
    elif userchoice == 'Paper':
        userChoiceImage.configure(image=paperImage)

    compchoice = random.choice(choices)
    compInput(compchoice)
    evaluate(compchoice, userchoice)
    count += 1
    if count == 5:
        checkWin(userScore, compScore)

def compInput(comp_input):
    global compchoice
    compchoice =  comp_input
    print(compchoice)
    if compchoice == 'Rock':
        compChoiceImage.configure(image=rockImage_comp)
    elif compchoice == 'Scissors':
        compChoiceImage.configure(image=scissorsImage_comp)
    elif compchoice == 'Paper':
        compChoiceImage.configure(image=paperImage_comp)

def evaluate(compchoice, userchoice):
    global userScore, compScore
    if userchoice == choices[0]:
        if compchoice == choices[1]:
            userScoreBoard()
        elif compchoice == choices[2]:
            compScoreBoard()
    elif userchoice == choices[1]:
        if compchoice == choices[2]:
            userScoreBoard()
        elif compchoice == choices[0]:
            compScoreBoard()
    elif userchoice == choices[2]:
        if compchoice == choices[0]:
            userScoreBoard()
        elif compchoice == choices[1]:
            compScoreBoard()

def userScoreBoard():
    global userScore
    userScore += 1
    userLabel.config(text="User: " + str(userScore))

def compScoreBoard():
    global compScore
    compScore += 1
    compLabel.config(text="Computer: " + str(compScore))

def checkWin(userScore, compScore):
    if userScore == compScore:
        decisionLabel.config(text="Draw Match!")
    elif userScore > compScore:
        decisionLabel.config(text="Congratulations! You Won the Match!")
    elif userScore < compScore:
        decisionLabel.config(text="Computer Won the Match!")

def getRestart():
    python = sys.executable
    os.execl(python, python, * sys.argv)

root = tk.Tk()
root.title("Rock Paper Scissors Game!")
root.geometry("790x750")
root.config(background='black')

# Heading
headerTextImage = ImageTk.PhotoImage(Image.open("header_image.png"))
headerText = tk.Label(root, image=headerTextImage, background='#000')
headerText.grid(row=0, column=3, padx=(0, 80))

# Input rock, paper, scissor image
rockImage = ImageTk.PhotoImage(Image.open("Rock.png"))
paperImage = ImageTk.PhotoImage(Image.open("Paper.png"))
scissorsImage = ImageTk.PhotoImage(Image.open("Scissors.png"))

rockImage_comp = ImageTk.PhotoImage(Image.open("Rock.png"))
paperImage_comp = ImageTk.PhotoImage(Image.open("Paper.png"))
scissorsImage_comp = ImageTk.PhotoImage(Image.open("Scissors.png"))

# Input rock or paper or scissor images
compChoiceImage = tk.Label(root, image=rockImage, background='#000')
compChoiceImage.grid(row=2, column=4)

userChoiceImage = tk.Label(root, image=rockImage, background='#000')
userChoiceImage.grid(row=2, column=1)

# Label user, computer
compScore = tk.Label(root, text="Computer: " + str(compScore), font="Arial 20 bold", fg='#d10ffc', bg='#000')
compScore.grid(row=3, column=4, pady=30)

userScore = tk.Label(root, text="User: " + str(userScore), bg='#000', font="Arial 20 bold", fg='#d10ffc')
userScore.grid(row=3, column=1, pady=30)

decisionLabel = tk.Label(root, text="", font=("Arial", 30), fg='#fff', bg='#000')
decisionLabel.grid(row=4, columnspan=5)

# User input
for i in range(3):
    userButton = tk.Button(root, text=choices[i], bg=colors[i], font=("Arial", 25), fg='black', command=lambda i=i: userInput(i))
    userButton.grid(row=5, column=i)

# Start game
reStartButton = tk.Button(root, text="Restart", bg='green', font=("Arial", 25), fg='black', command=getRestart)
reStartButton.grid(row=6, column=1, pady=50)

# Exit game
exitButton = tk.Button(root, text="Exit", bg='red', font=("Arial", 25), fg='black', command=root.destroy)
exitButton.grid(row=6, column=4, pady=50)

root.mainloop()
