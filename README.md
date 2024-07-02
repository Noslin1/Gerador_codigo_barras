import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image, ImageTk
import barcode
from barcode.writer import ImageWriter
import pyperclip
import os

def generate_barcode():
    barcode_data = barcode_entry.get()
    if not barcode_data:
        messagebox.showerror("Erro", "Por favor, insira o código de barras.")
        return

    # Selecionar o diretório e nome do arquivo para salvar a imagem
    filename = filedialog.asksaveasfilename(defaultextension=".png", filetypes=[("Imagens PNG", "*.png")])
    if not filename:
        return

    # Gerar o código de barras
    code128 = barcode.get_barcode_class('code128')
    generated_code = code128(barcode_data, writer=ImageWriter())
    
    # Salvar o código de barras como uma imagem
    generated_code.save(filename)

    messagebox.showinfo("Sucesso", f'Código de barras gerado e salvo como {filename}')

    # Copiar o código de barras para a área de transferência
    pyperclip.copy(barcode_data)
    messagebox.showinfo("Sucesso", 'Código de barras copiado para a área de transferência')

def quit_app():
    root.destroy()

root = tk.Tk()
root.title("Gerador de Código de Barras")

barcode_label = tk.Label(root, text="Código de Barras:")
barcode_label.grid(row=0, column=0, padx=5, pady=5)
barcode_entry = tk.Entry(root)
barcode_entry.grid(row=0, column=1, padx=5, pady=5)

generate_button = tk.Button(root, text="Gerar Código de Barras", command=generate_barcode)
generate_button.grid(row=1, column=0, columnspan=2, padx=5, pady=5, sticky="WE")

quit_button = tk.Button(root, text="Sair", command=quit_app)
quit_button.grid(row=2, column=0, columnspan=2, padx=5, pady=5, sticky="WE")

root.mainloop()
