import tkinter as tk
from tkinter import messagebox
from PIL import Image, ImageTk
import random
import string

# Initialize the main window
root = tk.Tk()
root.title("Password Generator by Bill Angel")
root.geometry("500x500")

# Load the background image
background_image = Image.open("C:/Users/Attilas/Desktop/locked.jpg")
background_image = background_image.resize((500, 500), Image.LANCZOS)  # Resize image to fit the window
bg_image = ImageTk.PhotoImage(background_image)

# Create a Canvas to display the background image
canvas = tk.Canvas(root, width=500, height=500)
canvas.pack(fill="both", expand=True)
canvas.create_image(0, 0, image=bg_image, anchor="nw")

# Function to reset label colors
def reset_label_colors():
    label_total_length.config(fg="black")
    label_lower.config(fg="black")
    label_upper.config(fg="black")
    label_symbols.config(fg="black")
    label_numbers.config(fg="black")

# Function to generate the password
def generate_password():
    reset_label_colors()  # Reset label colors before checking for errors

    try:
        # Get user inputs
        total_length = int(entry_total_length.get())
        lower = int(entry_lower.get())
        upper = int(entry_upper.get())
        symbols = int(entry_symbols.get())
        numbers = int(entry_numbers.get())

        # Validate that total length matches component lengths
        if total_length != lower + upper + symbols + numbers:
            messagebox.showerror("Input Error", "Sum of characters must equal total length!")
            label_total_length.config(fg="red")
            label_lower.config(fg="red")
            label_upper.config(fg="red")
            label_symbols.config(fg="red")
            label_numbers.config(fg="red")
            return

        # Generate the password components
        password = (
                random.choices(string.ascii_lowercase, k=lower) +
                random.choices(string.ascii_uppercase, k=upper) +
                random.choices("`~!@#$%^&*()-_=+[]{}|;:',.<>/?", k=symbols) +
                random.choices("0123456789", k=numbers)
        )

        # Shuffle and join the password into a single string
        random.shuffle(password)
        generated_password = ''.join(password)

        # Display the generated password
        password_display.delete(0, tk.END)
        password_display.insert(0, generated_password)

    except ValueError:
        messagebox.showerror("Input Error", "Please enter valid integers for all fields.")
        # Highlight labels with errors in red
        label_total_length.config(fg="red")
        label_lower.config(fg="red")
        label_upper.config(fg="red")
        label_symbols.config(fg="red")
        label_numbers.config(fg="red")

# UI Layout
label_total_length = tk.Label(root, text="Total Password Length (6-20):", bg=None)
label_total_length_window = canvas.create_window(250, 50, window=label_total_length)
entry_total_length = tk.Entry(root, justify="center")
entry_total_length_window = canvas.create_window(250, 75, window=entry_total_length)

label_lower = tk.Label(root, text="Number of Lowercase Letters:", bg=None)
label_lower_window = canvas.create_window(250, 110, window=label_lower)
entry_lower = tk.Entry(root, justify="center")
entry_lower_window = canvas.create_window(250, 135, window=entry_lower)

label_upper = tk.Label(root, text="Number of Uppercase Letters:", bg=None)
label_upper_window = canvas.create_window(250, 170, window=label_upper)
entry_upper = tk.Entry(root, justify="center")
entry_upper_window = canvas.create_window(250, 195, window=entry_upper)

label_symbols = tk.Label(root, text="Number of Symbols:", bg=None)
label_symbols_window = canvas.create_window(250, 230, window=label_symbols)
entry_symbols = tk.Entry(root, justify="center")
entry_symbols_window = canvas.create_window(250, 255, window=entry_symbols)

label_numbers = tk.Label(root, text="Number of Numbers:", bg=None)
label_numbers_window = canvas.create_window(250, 290, window=label_numbers)
entry_numbers = tk.Entry(root, justify="center")
entry_numbers_window = canvas.create_window(250, 315, window=entry_numbers)

# Button to trigger password generation
generate_button = tk.Button(root, text="Generate Password", command=generate_password)
generate_button_window = canvas.create_window(250, 350, window=generate_button)

# Entry field to display the generated password
password_display = tk.Entry(root, width=30, font=("Arial", 14), justify='center')
password_display_window = canvas.create_window(250, 380, window=password_display)

# Run the main loop
root.mainloop()
