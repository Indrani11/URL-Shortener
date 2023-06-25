# URL-Shortener
Created a Python URL Shortener using  Tkinter GUI and Pyshortener Library.


import pygame
import random
import sys

# Initialize Pygame
pygame.init()

# Set up the game window
window_width = 400
window_height = 600
window = pygame.display.set_mode((window_width, window_height))
pygame.display.set_caption("Flappy Bird")

# Set up colors
white = (255, 255, 255)

# Set up the clock
clock = pygame.time.Clock()

# Load images
background_img = pygame.image.load("background.png")
bird_img = pygame.image.load("bird.png")
pipe_img = pygame.image.load("pipe.png")

# Set up the bird
bird_width = 40
bird_height = 30
bird_x = 100
bird_y = window_height // 2 - bird_height // 2
bird_y_change = 0
gravity = 0.6

# Set up pipes
pipe_width = 70
pipe_gap = 150
pipe_speed = 3

score = 0
font = pygame.font.Font(None, 36)

def display_score(score):
    text = font.render("Score: " + str(score), True, white)
    window.blit(text, (10, 10))

def display_bird(x, y):
    window.blit(bird_img, (x, y))

def display_pipe(x, y, height):
    window.blit(pipe_img, (x, y))
    window.blit(pygame.transform.flip(pipe_img, False, True), (x, y - height - pipe_gap))

def is_collision(bird_x, bird_y, pipe_x, pipe_y, pipe_width, pipe_height, pipe_gap):
    if bird_y < 0 or bird_y + bird_height > window_height:
        return True
    if bird_x + bird_width > pipe_x and bird_x < pipe_x + pipe_width:
        if bird_y < pipe_height or bird_y + bird_height > pipe_height + pipe_gap:
            return True
    return False

running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bird_y_change = -8
        if event.type == pygame.KEYUP:
            if event.key == pygame.K_SPACE:
                bird_y_change = 3

    # Update bird position
    bird_y_change += gravity
    bird_y += bird_y_change

    # Update pipe position
    pipe_x -= pipe_speed

    # Check for collisions
    if is_collision(bird_x, bird_y, pipe_x, pipe_y, pipe_width, pipe_height, pipe_gap):
        running = False

    # Check if the pipe has passed the bird
    if pipe_x + pipe_width < bird_x:
        score += 1
        pipe_x = window_width
        pipe_height = random.randint(150, 400)
        pipe_y = random.randint(150, 400)

    # Draw background
    window.blit(background_img, (0, 0))

    # Draw the bird
    display_bird(bird_x, bird_y)

    # Draw the pipe
    display_pipe(pipe_x, pipe_y, pipe_height)

    # Display the score
    display_score(score)

    # Update the display
    pygame.display.update()

    # Limit the frame rate
    clock.tick(60)

# Quit the game
pygame.quit()
sys.exit()
