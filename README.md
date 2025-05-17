# NumBlast 
Game tebak angka berbasis Python + Tkinter
Pilih jumlah pemain, tebak angka rahasia dari 1-100, dan hindari angka BOOM!

## Cara Menjalankan
```bash
python main.py

Lalu commit dan push lagi:

```bash
git add README.md
git commit -m "Update README with project info"
git push

# NumBlast/
# ├── main.py
# ├── assets/
# │   ├── awal.jpg
# │   ├── loading.jpg
# │   ├── choose player.jpg
# │   └── loading otw ke game.jpg
# |   └── awalan game.jpg
# |   └── 6.jpg
# |   └── 7.jpg
# |   └── 8.jpg
# |   └── 9.jpg
# |   └── 10.jpg
# └── README.md

import tkinter as tk
from PIL import Image, ImageTk
from tkinter import *
import os
import random

class NumBlastApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("NumBlast!")
        self.geometry("1280x720")
        self.configure(bg="#ffcde1")
        self.current_frame = None
        self.assets_path = os.path.join(os.getcwd(), "assets")
        self.switch_frame(IntroScreen)

    def switch_frame(self, frame_class):
        if self.current_frame is not None:
            self.current_frame.destroy()
        self.current_frame = frame_class(self)
        self.current_frame.pack(fill="both", expand=True)

class IntroScreen(tk.Frame):
    def __init__(self, master):
        super().__init__(master, bg="#ffcde1")
        img = Image.open(os.path.join(master.assets_path, "awal.jpg"))
        self.bg = ImageTk.PhotoImage(img)
        label = tk.Label(self, image=self.bg)
        label.place(x=0, y=0, relwidth=1, relheight=1)
        self.after(3000, lambda: master.switch_frame(LoadingScreen))

class LoadingScreen(tk.Frame):
    def __init__(self, master):
        super().__init__(master, bg="#bce8f1")
        img = Image.open(os.path.join(master.assets_path, "loading.jpg"))
        self.bg = ImageTk.PhotoImage(img)
        label = tk.Label(self, image=self.bg)
        label.place(x=0, y=0, relwidth=1, relheight=1)
        self.after(3000, lambda: master.switch_frame(PlayerSelectScreen))
        pass
    
class PlayerSelectScreen(tk.Frame):
    def __init__(self, master):
        super().__init__(master, bg="#ffcde1")
        img = Image.open(os.path.join(master.assets_path, "choose player.jpg"))
        self.bg = ImageTk.PhotoImage(img)
        label = tk.Label(self, image=self.bg)
        label.place(x=0, y=0, relwidth=1, relheight=1)

        self.create_player_button(master, 2, x=390, y=420)
        self.create_player_button(master, 3, x=515, y=420)
        self.create_player_button(master, 4, x=640, y=420)
        self.create_player_button(master, 5, x=770, y=420)

    def create_player_button(self, master, num, x, y):
        btn = tk.Button(self, text="", bg="#ffcde1", activebackground="#ffcde1", bd=0, highlightthickness=0, relief="flat", command=lambda: self.select_players(master, num))
        btn.place(x=x, y=y, width=80, height=80) 

    def select_players(self, master, num):
        master.num_players = num
        print(f"Jumlah pemain yang dipilih: {num}")
        master.switch_frame(PlayLoadingScreen)

class PlayLoadingScreen(tk.Frame):
    def __init__(self, master):
        super().__init__(master, bg="#bce8f1")
        img = Image.open(os.path.join(master.assets_path, "loading otw ke game.jpg"))
        self.bg = ImageTk.PhotoImage(img)
        label = tk.Label(self, image=self.bg)
        label.place(x=0, y=0, relwidth=1, relheight=1)

        transparent_btn = tk.Button(self, text="", bg="#bce8f1", activebackground="#bce8f1", bd=0, highlightthickness=0, relief="flat", command=lambda: master.switch_frame(GameplayScreen))
        transparent_btn.place(x=530, y=610, width=220, height=60)

class GameplayScreen(tk.Frame):
    def __init__(self, master):
        super().__init__(master, bg="#ffffff")
        label = tk.Label(self, text="Gameplay Dimulai!", font=("Arial", 24), bg="#ffffff")
        label.pack(pady=300)

class GameAwalPlaySCreen(tk.Frame):
    def __init__(self, master):
        super().__init__(master, bg="fdcfeO")
        img = Image.open(os.path.join(master.assets_path, "awalan game.jpg"))
        self.bg = ImageTk.PhotoImage(img)
        label = tk.Label(self, image=self.bg)
        label.place(x=0, y=0, relwidth=1, relheight=1)

        next_button = Button(self, text="NEXT", font=("Helvetica", 12, "bold"), bg="#fdcfe0", fg="white", command=lambda: master.switch_frame(Slide6))
        next_button.place(x=1100, y=630, width=100, height=40)

    def go_to_slide6(self):
        for widget in self.winfo_children():
            widget.destroy()

        img = Image.open("6.jpg")
        img = img.resize((480,320))

        canvas = Canvas(self, width=480, height=320)
        canvas.pack()
        canvas.create_image(0, 0, anchor=NW, image=ImageTk)
        canvas.image = ImageTk

class GameSlideScreen(tk.Frame):
    def __init__(self, master, image_name, next_frame_class=None):
        super().__init__(master, bg="#ffffff")
        self.master = master
        self.image_name = image_name
        self.next_frame_class = next_frame_class

        img = Image.open(os.path.join(master.assets_path, image_name))
        self.bg = ImageTk.PhotoImage(img)
        label = tk.Label(self, image=self.bg)
        label.place(x=0, y=0, relwidth=1, relheight=1)

        if self.next_frame_class is not None:
            next_button = tk.Button(self,text="NEXT", font=("Helvetica", 12, "bold"), bg="#fdcfe0", fg="white", command=lambda: master.switch_frame(self.next_frame_class))
            next_button.place(x=1100, y=630, width=100, height=40)

class Slide6(GameSlideScreen):
    def __init__(self, master):
        super().__init__(master, "6.jpg", Slide7)

class Slide7(GameSlideScreen):
    def __init__(self, master):
        super().__init__(master, "7.jpg", Slide8)

class Slide8(GameSlideScreen):
    def __init__(self, master):
        super().__init__(master, "8.jpg", Slide9)

class Slide9(GameSlideScreen):
    def __init__(self, master):
        super().__init__(master, "9.jpg", Slide10)

class Slide10(GameSlideScreen):
    def __init__(self, master):
        super().__init__(master, "10.jpg") 
        start_button = tk.Button(self, text="Start Game", font=("Helvetica", 12, "bold"), bg="fdcfe0", fg="white", command=lambda: master.switch_frame(GameplayScreen))
        start_button.place(x=1100, y=630, width= 100, height=40)

class GameplayScreen(tk.Frame):
    def __init__(self, master):
        super().__init__(master, bg="#ffffff")
        self.master = master

        self.info_label = tk.Label(self, text=self.get_info_text(), font=("Helvetica", 20), bg="#ffffff", fg="#333")
        self.info_label.place(x=400, y=100)

        self.input_entry = tk.Entry(self, font=("Helvetica", 24), justify="center", bg="#d0f0c0", fg="#000")
        self.input_entry.place(x=500, y=200, width=280, height=60)

        submit_button = tk.Button(self, text="SUBMIT", font=("Helvetica", 14, "bold"), bg="#89CFF0", fg="white", command=self.tebak)
        submit_button.place(x=560, y=280, width=160, height=50)

        self.feedback_label = tk.Label(self, text="", font=("Helvetica", 16), bg="#ffffff", fg="red")
        self.feedback_label.place(x=460, y=350)


        self.angka_boom = random.randint(1, 100)
        self.batas_bawah = 1
        self.batas_atas = 100
        self.jumlah_pemain = master.num_players
        self.pemain_aktif = 1

        self.bg_label = tk.Label(self)
        self.bg_label.pack(fill="both", expand=True)

        # Load default background (misalnya gambar putih polos atau kosong)
        self.default_bg = tk.PhotoImage()  # kosong
        self.bg_label.config(image=self.default_bg)

    def get_info_text(self):
        return (f"Giliran Pemain {self.pemain_aktif} | Tebak angka antara {self.batas_bawah} sampai {self.batas_atas}")

    def change_bg_image(self, image_name):
        img_path = os.path.join(self.master.assets_path, image_name)
        img = Image.open(img_path)
        img = img.resize((1280, 720))
        self.bg_image = ImageTk.PhotoImage(img)
        self.bg_label.config(image=self.bg_image)

    def tebak(self):
        tebakan_str = self.input_entry.get()
        try:
            tebakan = int(tebakan_str)
        except ValueError:
            self.change_bg_image("9.jpg")
            self.master.switch_frame(GameplayScreen)
            return

        if tebakan < self.batas_bawah or tebakan > self.batas_atas:
            self.feedback_label.config(text="Tebakan di luar batas! Coba lagi.")
            self.input_entry.delete(0, tk.END)
            return

        if tebakan == self.angka_boom:
            self.change_bg_image("10.jpg")
            self.master.switch_frame(PlayerSelectScreen)
            return
        elif tebakan < self.angka_boom:
            self.change_bg_image("8.jpg")
            self.batas_bawah = tebakan + 1
        else:
            self.change_bg_image("7.jpg")
            self.batas_atas = tebakan - 1

        if self.batas_bawah == self.batas_atas == self.angka_boom:
            self.batas_bawah = max(1, self.angka_boom - 1)
            self.batas_atas = min(100, self.angka_boom + 1)

        self.pemain_aktif = (self.pemain_aktif % self.jumlah_pemain) + 1
        self.info_label.config(text=self.get_info_text())
        self.input_entry.delete(0, tk.END)

class GameAwalPlayScreen(tk.Frame):
    def __init__(self, master):
        super().__init__(master, bg="#fdcfe0")
        img = Image.open(os.path.join(master.assets_path, "awalan game.jpg"))
        self.bg = ImageTk.PhotoImage(img)
        label = tk.Label(self, image=self.bg)
        label.place(x=0, y=0, relwidth=1, relheight=1)

        next_button = tk.Button(self,text="NEXT", font=("Helvetica", 12, "bold"), bg="#fdcfe0", fg="white", command=lambda: master.switch_frame(Slide6))
        next_button.place(x=1100, y=630, width=100, height=40)


if __name__ == '__main__':
    app = NumBlastApp()
    app.mainloop()

