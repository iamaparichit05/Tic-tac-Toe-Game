# Tic-tac-Toe-Game
import tkinter as tk
from tkinter import messagebox

current_player='X'
board=[""]*9
game_over=False
winning_combination=None
player1_name=""
player2_name=""
player1_symbol=""
player2_symbol=""
player1_score=0
player2_score=0
tie=0
final_match_winner=None
final_match_played=False
final_game=False

winning_combinations=[(0,1,2), (3,4,5), (6,7,8),
                     (0,3,6),(1,4,7),(2,5,8),
                     (0,4,8),(2,4,6)]

def check_win():
    global winning_combination
    for combo in winning_combinations:
        a,b,c=combo
        if board[a]==board[b]==board[c]!="":
            winning_combination=combo
            return True
    return False

def check_draw():
    return all(cell!="" for cell in board)

def on_click(index):
    global current_player,game_over,player1_score,player2_score,tie
    if board[index]=="" and not game_over:
        board[index]=current_player
        buttons[index].config(text=current_player)
        if check_win():
            game_over=True
            if current_player==player1_symbol:
                player1_score+=1
            else:
                player2_score+=1
            if final_game:
                show_winner_message()
                player1_score=0
                player2_score=0
                tie=0
            update_scores()
            root.after(100,highlight_winning_combination)
            root.after(500,reset_board)
        elif check_draw():
            game_over=True
            tie+=1
            update_scores()
            show_draw_message()
            reset_board()
        else:
            current_player='O' if current_player=='X' else 'X'

def highlight_winning_combination():
    if winning_combination:
        for index in winning_combination:
            buttons[index].config(bg="yellow")

def show_draw_message():
    messagebox.showinfo("Tic-Tac-Toe","It's a draw!")

def reset_board():
    global board, game_over,winning_combination,current_player
    board=[""]*9
    game_over=False
    winning_combination=None
    for button in buttons:
        button.config(text="",bg='white')
    current_player='X'
    highlight_winning_combination()

def reset_scores():
    global player1_score,player2_score,final_match_winner,final_match_played,tie
    player1_score=0
    player2_score=0
    tie=0
    final_match_winner=None
    final_match_played=False
    update_scores()

def play_again():
    global player1_name, player2_name,player1_symbol,player2_symbol,final_match_played,final_game
    final_game=False
    player1_entry.pack()
    player2_entry.pack()
    symbol_choice_label.pack()
    symbol_choice_entry.pack()
    start_button.pack()
    player1_score_label.config(text="")
    player2_score_label.config(text="")
    tie_label.config(text="")
    player1_label.config(text="Player 1:")
    player2_label.config(text="Player 2:")
    final_match_played=False
                      
    

def start_final_match():
    messagebox.showinfo("Final Match","This is the final match")
    global final_match_played,player1_score,player2_score,final_game
    player1_entry.pack_forget()
    player2_entry.pack_forget()
    symbol_choice_label.pack_forget()
    symbol_choice_entry.pack_forget()
    start_button.place_forget()
    update_scores()
    final_game=True

def show_winner_message():
    global player1_score,player2_score,final_game
    final_game=False
    if player1_score>player2_score:
        final_match_winner=f"{player1_name} ({player1_symbol}) wins!"
    elif player2_score>player1_score:
        final_match_winner=f"{player2_name} ({player2_symbol}) wins!"
    else:
        final_match_winner="It's a draw"
    messagebox.showinfo("Tic-Tac-Toe",f"{final_match_winner}")

def declare_winner():
    if player1_score>player2_score:
        final_match_winner=f"{player1_name} ({player1_symbol}) wins!"
    elif player2_score>player1_score:
        final_match_winner=f"{player2_name} ({player2_symbol}) wins!"
    else:
        final_match_winner="It's a draw"
    messagebox.showinfo("Tic-Tac-Toe",f"{final_match_winner}")
    reset_scores()

def update_scores():
    player1_score_label.config(text=f"{player1_name} ({player1_symbol}) Score: {player1_score}",font=("arial",12),fg="green")
    player2_score_label.config(text=f"{player2_name} ({player2_symbol}) Score: {player2_score}",font=("arial",12),fg="green")
    tie_label.config(text=f"Tie Score: {tie}",font=("arial",12),fg="green")
def start_game():
    global player1_name,player2_name,player1_score,player2_score,player1_symbol,player2_symbol,final_game
    player1_name=player1_entry.get()
    player2_name=player2_entry.get()
    player1_symbol=symbol_choice.get()
    player2_symbol='X' if player1_symbol=='O' else 'O'
    final_game=False
    player1_label.config(text=f"Player 1: {player1_name}({player1_symbol})")
    player2_label.config(text=f"Player 2: {player2_name}({player2_symbol})")
    for button in buttons:
        button.config(state=tk.NORMAL)
    player1_entry.pack_forget()
    player2_entry.pack_forget()
    symbol_choice_label.pack_forget()
    symbol_choice_entry.pack_forget()
    start_button.place_forget()

    reset_scores()
    reset_board()

root=tk.Tk()
root.title("Tic-Tac-Toe")
root.geometry("500x800")
root.configure(bg="grey")

player1_label=tk.Label(root,text="Player 1:",font=("arial",14),bg="grey",fg="black")
player1_label.pack()
player1_entry=tk.Entry(root,font=("arial",11))
player1_entry.pack()

player2_label=tk.Label(root,text="Player 2:",font=("arial",14),bg="grey",fg="black")
player2_label.pack()
player2_entry=tk.Entry(root,font=("arial",11))
player2_entry.pack()

symbol_choice_label=tk.Label(root,text="Choose a symbol for Player 1:",font=("arial",14),bg="grey",fg="black")
symbol_choice_label.pack()
symbol_choice=tk.StringVar(value="X")
symbol_choice_entry=tk.Entry(root, textvariable=symbol_choice,font=("arial",11))
symbol_choice_entry.pack()

start_button=tk.Button(root,text="Start Game",command=start_game,bg="black",fg="white",font=("arial",11))
start_button.place(x=210,y=500)
reset_score_button=tk.Button(root,text="Reset Scores",command=reset_scores,bg="black",fg="white",font=("arial",11))
reset_score_button.place(x=203,y=530)
play_again_button=tk.Button(root,text="Play Again",command=play_again,bg="black",fg="white",font=("arial",11))
play_again_button.place(x=213,y=560)
final_match_button=tk.Button(root,text="Final Match",command=start_final_match,bg="black",fg="white",font=("arial",11))
final_match_button.place(x=210,y=590)
declare_winner_button=tk.Button(root,text="Declare Winner",command=declare_winner,bg="black",fg="white",font=("arial",11))
declare_winner_button.place(x=203,y=620)
player1_score_label=tk.Label(root,text="",bg="grey")
player1_score_label.pack(padx=15)
player2_score_label=tk.Label(root,text="",bg="grey")
player2_score_label.pack()
tie_label=tk.Label(root,text="",bg="grey")
tie_label.pack()

game_frame=tk.Frame(root)

buttons=[]
for i in range(9):
    row=i//3
    col=i%3
    button=tk.Button(game_frame,text="",font=("normal",20),width=5,height=2,
                     command=lambda index=i: on_click(index),state=tk.DISABLED)
    button.grid(row=row,column=col)
    buttons.append(button)

game_frame.pack()
root.mainloop()

