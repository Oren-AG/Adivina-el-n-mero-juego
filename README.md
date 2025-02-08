import tkinter as tk
import random
import pygame  # Para reproducir sonido
from PIL import Image, ImageTk  # Para manejar imágenes

# Inicializar pygame mixer para sonido
pygame.mixer.init()

# Cargar el sonido de victoria (debes tener un archivo 'victoria.mp3' en la misma carpeta)
sonido_victoria = pygame.mixer.Sound(r"C:\Users\TH_ZN\Downloads\ganaste.wav")

# Generar un número aleatorio
numero_secreto = random.randint(0, 100)

# Puntuación inicial
puntuacion = 0

def verificar_numero():
    """Verifica si el número ingresado es correcto y actualiza la puntuación"""
    global puntuacion
    try:
        intento = int(entrada.get())
        if intento < numero_secreto:
            mensaje.set("El número es mayor ⬆")
        elif intento > numero_secreto:
            mensaje.set("El número es menor ⬇")
        else:
            puntuacion += 1  # Sumar un punto por adivinar correctamente
            mensaje.set(f"¡Felicidades! 🎉 Has adivinado el número. Puntuación: {puntuacion}")
            sonido_victoria.play()  # Reproducir sonido de victoria
            mostrar_efecto_victoria()  # Mostrar efecto de victoria
            boton_reiniciar.pack()  # Mostrar botón de reinicio
    except ValueError:
        mensaje.set("Por favor, introduce un número válido.")

def reiniciar_juego():
    """Reinicia el juego generando un nuevo número"""
    global numero_secreto
    numero_secreto = random.randint(0, 100)
    mensaje.set("Intenta adivinar el número entre 0 y 100")
    entrada.delete(0, tk.END)
    boton_reiniciar.pack_forget()  # Ocultar el botón de reinicio
    etiqueta_imagen.config(image="")  # Quitar la imagen de victoria
    ventana.config(bg="white")  # Restablecer el fondo blanco

def mostrar_efecto_victoria():
    """Muestra un efecto de celebración al ganar"""
    ventana.config(bg="yellow")  # Cambiar el fondo a un color llamativo
    mensaje.set(f"🎉 ¡Felicidades! 🎉 Puntuación: {puntuacion}")
    ventana.after(500, lambda: ventana.config(bg="orange"))  # Cambiar el color después de 500ms
    ventana.after(1000, lambda: ventana.config(bg="yellow"))  # Cambiar el color después de 1000ms
    ventana.after(1500, lambda: ventana.config(bg="orange"))  # Cambiar el color después de 1500ms

# Configuración de la ventana principal
ventana = tk.Tk()
ventana.title("Adivina el Número")
ventana.geometry("400x400")

# Widgets de la interfaz gráfica
tk.Label(ventana, text="Introduce un número del 0 al 100:").pack()

entrada = tk.Entry(ventana)
entrada.pack()

boton_verificar = tk.Button(ventana, text="Comprobar", command=verificar_numero)
boton_verificar.pack()

mensaje = tk.StringVar()
mensaje.set("Intenta adivinar el número entre 0 y 100")

etiqueta_mensaje = tk.Label(ventana, textvariable=mensaje)
etiqueta_mensaje.pack()

boton_reiniciar = tk.Button(ventana, text="Reiniciar Juego", command=reiniciar_juego)
boton_reiniciar.pack_forget()  # Se oculta al inicio

# Etiqueta para mostrar la imagen de victoria
etiqueta_imagen = tk.Label(ventana)
etiqueta_imagen.pack()

# Ejecutar la ventana
ventana.mainloop()
