# JSON Explorer

> Provide Python 3 Tkinter application to show Open File dialog to open a json file and then show it in structured tree-like display, where you can hide and show content of objects on all levels of hierarchy.

To create a Python 3 Tkinter application that opens a JSON file using an Open File dialog and then displays it in a structured tree-like view, you can use the `tkinter` library for the GUI and the `json` library for parsing JSON files. Additionally, you can use the `tkinter.ttk.Treeview` widget to create the tree structure.

Below is a complete example of such an application:

```python
import tkinter as tk
from tkinter import ttk
from tkinter import filedialog
import json

def load_json():
    file_path = filedialog.askopenfilename(filetypes=[("JSON files", "*.json")])
    if file_path:
        with open(file_path, 'r') as file:
            data = json.load(file)
            clear_tree()
            build_tree('', data)

def insert_tree(parent, key, value):
    if isinstance(value, dict):
        node = tree.insert(parent, 'end', text=str(key), open=False)
        for subkey, subval in value.items():
            insert_tree(node, subkey, subval)
    elif isinstance(value, list):
        node = tree.insert(parent, 'end', text=str(key) + '[]', open=False)
        for i, item in enumerate(value):
            insert_tree(node, f'Index {i}', item)
    else:
        tree.insert(parent, 'end', text=str(key), value=str(value))

def build_tree(parent, data):
    if isinstance(data, dict):
        for key, value in data.items():
            insert_tree(parent, key, value)
    elif isinstance(data, list):
        for i, item in enumerate(data):
            insert_tree(parent, f'Index {i}', item)
    else:
        tree.insert(parent, 'end', text='Value', value=str(data))

def clear_tree():
    for item in tree.get_children():
        tree.delete(item)

def toggle_item(event):
    item_id = tree.focus()
    if tree.item(item_id, 'open'):
        tree.item(item_id, open=False)
    else:
        tree.item(item_id, open=True)

root = tk.Tk()
root.title("JSON Viewer")

# Create the treeview
tree = ttk.Treeview(root, columns=('Value',), show='tree')
tree.heading('#0', text='Key')
tree.heading('Value', text='Value')
tree.column('Value', stretch=True)
tree.bind('<Double-1>', toggle_item)
tree.pack(fill='both', expand=True)

# Add a menu to load the JSON file
menu_bar = tk.Menu(root)
file_menu = tk.Menu(menu_bar, tearoff=0)
file_menu.add_command(label="Open JSON File", command=load_json)
menu_bar.add_cascade(label="File", menu=file_menu)
root.config(menu=menu_bar)

root.mainloop()
```

This script creates a Tkinter window with a menu bar containing an "Open JSON File" option. When you select this option, it opens a file dialog to choose a JSON file. Once a file is selected, the JSON data is loaded and displayed in a `Treeview` widget. You can double-click on any node to toggle its visibility.

Please note that this script assumes that the JSON structure is composed of dictionaries, lists, and scalar values (e.g., strings, numbers, booleans). It does not handle custom objects or other complex types that might be present in JSON data.
Prompt tokens: 49, Completion tokens: 696, Total price: 0.021 USD
