from tkinter import *

class Whiteboard:
    def __init__(self, master):
        self.master = master
        self.master.title("Digital Whiteboard")
        self.master.geometry("800x600")

        self.current_x = 0
        self.current_y = 0
        self.color = "black"
        self.is_eraser = False
        self.eraser_size = IntVar()
        self.eraser_size.set(10)

        self.canvas = Canvas(self.master, bg="white", width=800, height=600)
        self.canvas.pack(expand=YES, fill=BOTH)

        self.canvas.bind("<B1-Motion>", self.paint)
        self.canvas.bind("<ButtonRelease-1>", self.reset)
        self.canvas.bind("<Button-1>", self.set_start)

        self.color_frame = Frame(self.master, bg="#f2f3f5", bd=2, relief=SOLID)
        self.color_frame.place(x=10, y=10, height=580, width=30)

        self.display_color_selection()

        self.eraser_button = Button(self.master, text="Eraser", command=self.toggle_eraser)
        self.eraser_button.place(x=10, y=590, height=25, width=70)

        eraser_size_label = Label(self.master, text="Eraser Size:")
        eraser_size_label.place(x=90, y=593)

        eraser_size_menu = OptionMenu(self.master, self.eraser_size, 5, 10, 15, 20)
        eraser_size_menu.place(x=160, y=590, height=30, width=60)

        self.reset_button = Button(self.master, text="Reset", command=self.reset_canvas)
        self.reset_button.place(x=240, y=590, height=25, width=70)

    def paint(self, event):
        x1, y1 = (self.current_x, self.current_y)
        x2, y2 = (event.x, event.y)

        if not self.is_eraser:
            self.canvas.create_line(x1, y1, x2, y2, width=2, fill=self.color, capstyle=ROUND, smooth=True)
        else:
            size = self.eraser_size.get()
            self.canvas.create_rectangle(x2 - size, y2 - size, x2 + size, y2 + size, fill="white", outline="white")

        self.current_x, self.current_y = x2, y2

    def reset(self, event):
        self.current_x = 0
        self.current_y = 0

    def set_start(self, event):
        self.current_x, self.current_y = event.x, event.y

    def show_color(self, new_color):
        self.color = new_color
        self.is_eraser = False

    def display_color_selection(self):
        colors = ["#000000", "#FF0000", "#00FF00", "#0000FF", "#FFFF00", "#FFA500", "#800080", "#A52A2A", "#808080",
                  "#FFC0CB", "#00FFFF", "#008000", "#4B0082", "#FF00FF", "#800000", "#008080", "#C0C0C0", "#FFD700"]

        for i, color in enumerate(colors):
            y1, y2 = i * 30, (i + 1) * 30
            color_button = Button(self.color_frame, bg=color, command=lambda c=color: self.show_color(c))
            color_button.place(x=5, y=y1, height=25, width=20)

    def toggle_eraser(self):
        self.is_eraser = not self.is_eraser
        if self.is_eraser:
            self.eraser_button.configure(text="Drawing")
        else:
            self.eraser_button.configure(text="Eraser")

    def reset_canvas(self):
        self.canvas.delete("all")

if __name__ == "__main__":
    root = Tk()
    whiteboard = Whiteboard(root)
    root.mainloop()
