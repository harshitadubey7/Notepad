from tkinter import Tk, Text, Menu, filedialog, simpledialog, font, END

def open_file():
    file_path = filedialog.askopenfilename()
    if file_path:
        with open(file_path, 'r') as file:
            text_widget.delete('1.0', END)
            text_widget.insert('1.0', file.read())

def save_file():
    file_path = filedialog.asksaveasfilename(defaultextension=".txt",
                                             filetypes=[("Text files", "*.txt"), ("All files", "*.*")])
    if file_path:
        with open(file_path, 'w') as file:
            file.write(text_widget.get('1.0', END))

def undo_action(): text_widget.edit_undo()
def redo_action(): text_widget.edit_redo()
def cut_action(): text_widget.event_generate("<<Cut>>")
def copy_action(): text_widget.event_generate("<<Copy>>")
def paste_action(): text_widget.event_generate("<<Paste>>")
def select_all(): text_widget.tag_add("sel", "1.0", END)

def change_font():
    font_name = simpledialog.askstring("Font", "Enter font name:", initialvalue=current_font.actual("family"))
    if font_name: current_font.config(family=font_name)

def change_size():
    font_size = simpledialog.askinteger("Font Size", "Enter font size:", initialvalue=current_font.actual("size"))
    if font_size: current_font.config(size=font_size)

# Main Application
root = Tk()
root.title("Notepad")
root.geometry("600x400")

# Set default font
current_font = font.Font(family="Arial", size=12)

# Text Widget
text_widget = Text(root, wrap='word', font=current_font, undo=True)
text_widget.pack(expand=1, fill='both')

# Menu Bar
menu_bar = Menu(root)
root.config(menu=menu_bar)

# File Menu
file_menu = Menu(menu_bar, tearoff=0)
file_menu.add_command(label="Open", command=open_file)
file_menu.add_command(label="Save", command=save_file)
file_menu.add_command(label="Exit", command=root.quit)
menu_bar.add_cascade(label="File", menu=file_menu)

# Edit Menu
edit_menu = Menu(menu_bar, tearoff=0)
edit_menu.add_command(label="Undo", command=undo_action)
edit_menu.add_command(label="Redo", command=redo_action)
edit_menu.add_separator()
edit_menu.add_command(label="Cut", command=cut_action)
edit_menu.add_command(label="Copy", command=copy_action)
edit_menu.add_command(label="Paste", command=paste_action)
edit_menu.add_separator()
edit_menu.add_command(label="Select All", command=select_all)
menu_bar.add_cascade(label="Edit", menu=edit_menu)

# Font Menu
font_menu = Menu(menu_bar, tearoff=0)
font_menu.add_command(label="Change Font", command=change_font)
font_menu.add_command(label="Change Size", command=change_size)
menu_bar.add_cascade(label="Font", menu=font_menu)

root.mainloop()
# Notepad
