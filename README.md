import tkinter as tk
from PIL import Image, ImageTk
import random
import time

# Create main window
root = tk.Tk()
root.title("Rock Paper Scissors")
root.geometry("500x600")

# Load and resize images
def load_image(path, width=150, height=150):
    img = Image.open(path)
    img = img.resize((width, height), Image.Resampling.LANCZOS)
    return ImageTk.PhotoImage(img)

rock_img = load_image("rock.png")
paper_img = load_image("paper.png")
scissor_img = load_image("scissor.png")

choices = ["Rock", "Paper", "Scissors"]

# Display frame
result_frame = tk.Frame(root)
result_frame.pack(pady=20)

user_label = tk.Label(result_frame, text="Your Choice:", font=("Helvetica", 14))
user_label.pack()

comp_label = tk.Label(result_frame, text="Computer Choice:", font=("Helvetica", 14))
comp_label.pack()

result_label = tk.Label(root, text="", font=("Helvetica", 20, "bold"))
result_label.pack(pady=20)

# Game logic with animation
def play(user_choice):
    comp_choice = random.choice(choices)

    # Clear previous images
    for widget in result_frame.winfo_children()[2:]:
        widget.destroy()

    # Show user choice
    if user_choice == "Rock":
        tk.Label(result_frame, image=rock_img).pack(side="left", padx=10)
    elif user_choice == "Paper":
        tk.Label(result_frame, image=paper_img).pack(side="left", padx=10)
    else:
        tk.Label(result_frame, image=scissor_img).pack(side="left", padx=10)

    # Animation effect for computer choice
    anim_label = tk.Label(result_frame)
    anim_label.pack(side="right", padx=10)

    def animate(count=0):
        if count < 6:  # Number of flashes
            img = random.choice([rock_img, paper_img, scissor_img])
            anim_label.config(image=img)
            root.after(200, lambda: animate(count + 1))
        else:
            # Show final computer choice
            if comp_choice == "Rock":
                anim_label.config(image=rock_img)
            elif comp_choice == "Paper":
                anim_label.config(image=paper_img)
            else:
                anim_label.config(image=scissor_img)
            
            # Determine result
            if user_choice == comp_choice:
                result = "It's a Tie!"
            elif (user_choice == "Rock" and comp_choice == "Scissors") or \
                 (user_choice == "Paper" and comp_choice == "Rock") or \
                 (user_choice == "Scissors" and comp_choice == "Paper"):
                result = "You Win!"
            else:
                result = "You Lose!"
            result_label.config(text=result)

    animate()

# Buttons
tk.Label(root, text="Choose Rock, Paper, or Scissors", font=("Helvetica", 16)).pack(pady=20)

btn_frame = tk.Frame(root)
btn_frame.pack(pady=20)

rock_btn = tk.Button(btn_frame, image=rock_img, command=lambda: play("Rock"))
rock_btn.grid(row=0, column=0, padx=10)

paper_btn = tk.Button(btn_frame, image=paper_img, command=lambda: play("Paper"))
paper_btn.grid(row=0, column=1, padx=10)

scissor_btn = tk.Button(btn_frame, image=scissor_img, command=lambda: play("Scissors"))
scissor_btn.grid(row=0, column=2, padx=10)

root.mainloop()
