from pynput import mouse, keyboard
from threading import Thread
import time

# Define the functions for each key

def pause_script():
    global running
    running = not running

def fast_click():
    while fast_clicking and running:
        mouse_controller.click(mouse.Button.left)
        time.sleep(0.01)  # Reduced sleep time for faster clicks

def ctrl_x():
    while ctrl_x_active and running:
        with keyboard_controller.pressed(keyboard.Key.ctrl):
            for _ in range(8):  # Optimized loop to send Ctrl+X 8 times in quick succession
                if not running:
                    break
                keyboard_controller.press('x')
                keyboard_controller.release('x')
                time.sleep(0.01)  # Reduced sleep time

def ctrl_f4():
    while ctrl_f4_active and running:
        with keyboard_controller.pressed(keyboard.Key.ctrl):
            for _ in range(8):  # Optimized loop to send Ctrl+F4 8 times in quick succession
                if not running:
                    break
                keyboard_controller.press(keyboard.Key.f4)
                keyboard_controller.release(keyboard.Key.f4)
                time.sleep(0.01)  # Reduced sleep time

# Create mouse and keyboard controllers
mouse_controller = mouse.Controller()
keyboard_controller = keyboard.Controller()

# Flags to control the loops
running = True
fast_clicking = False
ctrl_x_active = False
ctrl_f4_active = False

# Define key press event handlers
def on_press(key):
    global fast_clicking, ctrl_x_active, ctrl_f4_active, running

    try:
        if key == keyboard.Key.pause:
            pause_script()
        elif key == keyboard.KeyCode.from_char('h'):
            if running and not fast_clicking:
                fast_clicking = True
                Thread(target=fast_click).start()
        elif key == keyboard.KeyCode.from_char('j'):
            if running and not ctrl_x_active:
                ctrl_x_active = True
                Thread(target=ctrl_x).start()
        elif key == keyboard.KeyCode.from_char('k'):
            if running and not ctrl_f4_active:
                ctrl_f4_active = True
                Thread(target=ctrl_f4).start()
        elif key == keyboard.KeyCode.from_char('u'):
            running = False  # Stops the script by setting running to False
    except AttributeError:
        pass

# Define key release event handlers
def on_release(key):
    global fast_clicking, ctrl_x_active, ctrl_f4_active

    try:
        if key == keyboard.KeyCode.from_char('h'):
            fast_clicking = False
        elif key == keyboard.KeyCode.from_char('j'):
            ctrl_x_active = False
        elif key == keyboard.KeyCode.from_char('k'):
            ctrl_f4_active = False
    except AttributeError:
        pass

# Start listening for key presses
with keyboard.Listener(on_press=on_press, on_release=on_release) as listener:
    listener.join()
