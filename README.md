[Untitled-1.py](https://github.com/user-attachments/files/32099962/Untitled-1.py)
import tkinter as tk
from tkinter import colorchooser
import time

win = tk.Tk()
win.title(" Matrix ")
win.geometry("800x700")
current_bg = "black"
text_color = "#00ff00"  # Matrix Green
win.configure(bg=current_bg)


lyrics_lines = [
    "待ちぼうけさ 追い掛けても",
    "遠ざかっていく日も見えない",
    "あの声はどこから来て",
    "どこへ消えてゆくのだろう",
    "いつも",
    "待ちぼうけさ 追い掛けても",
    "遠ざかっていく日も見えない",
    "あの声はどこから来て",
    "どこへ消えてゆくのだろう",
    "いつも",
    "焦がれて抱いたら",
    "壊れてしまったよ",
    "愛とかなしみに",
    "焦がれてしまったよ",
    "そこまでは言いため",
    "問いかける、あの日を",
    "どこまでも続く と",
    "ここでまだ待ってる と",
    "あの声はどこから来て",
    "どこへ消えてゆくのだろう",
    "いつも",
    "焦がれて抱いたら",
    "壊れてしまったよ",
    "愛とかなしみに",
    "焦がれてしまったよ",
    "ひとり歩く、歩道はさびて",
    "青い 橙色の日",
    "居たい きみの横 そっと",
    "色、なくしても",
    "焦がれて抱いたら",
    "壊れてしまったよ",
]

def pick_color():
    color_result = colorchooser.askcolor(title="Choose a background color")
    if color_result[1]:
        selected_color = color_result[1]
        win.configure(bg=selected_color)
        title_label.configure(bg=selected_color)
        terminal_display.configure(bg=selected_color)
        color_btn.configure(bg=selected_color)
        play_pause_btn.configure(bg=selected_color)

title_label = tk.Label(
    win, 
    text="neo wakes up", 
    bg=current_bg, 
    fg=text_color, 
    font=("Consolas", 40, "bold")
)
title_label.pack(pady=20)

terminal_display = tk.Text(
    win,
    bg=current_bg,
    fg=text_color,
    font=("Consolas", 18),
    bd=0,
    highlightthickness=0,
    wrap="word",
    height=12,
    width=50
)
terminal_display.pack(pady=10)

current_line_idx = 0
current_char_idx = 0
is_playing = True

def type_matrix_text():
    global current_line_idx, current_char_idx, is_playing
    if not is_playing:
        return

    if current_line_idx < len(lyrics_lines):
        line = lyrics_lines[current_line_idx]
        if current_char_idx < len(line):
            # Insert character by character
            terminal_display.insert(tk.END, line[current_char_idx])
            terminal_display.see(tk.END)
            current_char_idx += 1
            win.after(70, type_matrix_text)
        else:
            # New line reached
            terminal_display.insert(tk.END, "\n")
            terminal_display.see(tk.END)
            current_line_idx += 1
            current_char_idx = 0
            win.after(600, type_matrix_text)
    else:
        # End of lyrics reset sequence
        win.after(20000, reset_animation)

def reset_animation():
    global current_line_idx, current_char_idx
    current_line_idx = 0
    current_char_idx = 0
    terminal_display.delete("5.3", tk.END)
    type_matrix_text()

def toggle_animation():
    global is_playing
    if is_playing:
        is_playing = False
        play_pause_btn.configure(text="Resume Terminal")
    else:
        is_playing = True
        play_pause_btn.configure(text="Pause Terminal")
        type_matrix_text()

# Control Buttons
color_btn = tk.Button(
    win, 
    text="Change Background Color", 
    command=pick_color,
    font=("Consolas", 10),
    bg=current_bg,
    fg=text_color
)
color_btn.pack(pady=5)

play_pause_btn = tk.Button(
    win, 
    text="Pause Terminal", 
    command=toggle_animation,
    font=("Consolas", 10),
    bg=current_bg,
    fg=text_color
)
play_pause_btn.pack(pady=5)

# Start typewriter animation
type_matrix_text()



win.mainloop()
