# desktop_pet
# Desktop pet, Evernight dance, solo cambien el path, just change path
from PIL import Image, ImageTk
import tkinter as tk

impath = r"path"
gif_path = impath 
SCALE_FACTOR = 0.75 

window = tk.Tk()
window.title("Evernight Desktop Pet")
window.overrideredirect(True) 
window.attributes('-topmost', True)  

window.attributes('-transparentcolor', '#000001')

drag_start_x = 0
drag_start_y = 0

gif = Image.open(gif_path)
num_frames = gif.n_frames  
frames = []

for i in range(num_frames):
    gif.seek(i)
    frame = gif.copy().convert('RGBA')  
    new_size = (int(frame.width * SCALE_FACTOR), int(frame.height * SCALE_FACTOR))
    frame = frame.resize(new_size, Image.LANCZOS)  
    photo = ImageTk.PhotoImage(frame)  
    frames.append(photo)  

label = tk.Label(window, bd=0, bg='#000001')  
label.pack()

def start_drag(event):
    global drag_start_x, drag_start_y
    drag_start_x = event.x_root - window.winfo_x()
    drag_start_y = event.y_root - window.winfo_y()

def drag_window(event):
    new_x = event.x_root - drag_start_x
    new_y = event.y_root - drag_start_y
    window.geometry(f'+{new_x}+{new_y}')

label.bind('<Button-1>', start_drag)
label.bind('<B1-Motion>', drag_window)

def animate(frame_num):
    frame = frames[frame_num]
    label.config(image=frame)
    window.geometry(f'{frame.width()}x{frame.height()}')  
    window.after(100, animate, (frame_num + 1) % num_frames)  

animate(0)

window.mainloop()
