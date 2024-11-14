# Ex.No: 11  Mini Project 
### DATE:                                                                            
### REGISTER NUMBER : 212222240028
### AIM: 
To write a python program to simulate the game using rule based algorithm
### Algorithm:
Step 1:Initialize Grid and Define Matching Rules
Step 2:Display the grid on the screen using a game library
Step 3:Validate the match and check for the swap
Step 4:Check for the level completion and Increase the difficulty
Step 5: Repeat the process again

### Program:
```

import pygame
import time
import random

# Initialize Pygame
pygame.init()

# Load the background image
background_image = pygame.image.load('ground.jpg')  # Load the image from the file system
background_image = pygame.transform.scale(background_image, (800, 600))  # Resize the image to fit the screen

# Screen settings
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Jumping Jack Game with Obstacles")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# Fonts
font = pygame.font.SysFont(None, 55)
small_font = pygame.font.SysFont(None, 35)

# Game settings
player_x = 100
player_y = screen_height - 150
char_width = 50
char_height = 100
ground_y = screen_height - char_height - 50
jump_height = 10
player_velocity = 0
player_score = 0
game_speed = 8  # Increased initial speed of the obstacles

# Obstacle settings
obstacle_width = 50
obstacle_height = 100
obstacle_color = RED
obstacles = []

# Add obstacles at random intervals
obstacle_timer = 0
obstacle_interval = 80  # Reduced interval to make obstacles appear more frequently

# Player status
is_jumping = False
jump_count = jump_height
is_game_over = False

# Human toy sprite drawing (Simple stick figure)
def draw_toy_character(x, y):
    # Head
    pygame.draw.circle(screen, GREEN, (x + char_width // 2, y + 20), 15)
    # Body
    pygame.draw.line(screen, GREEN, (x + char_width // 2, y + 35), (x + char_width // 2, y + 80), 5)
    # Arms
    pygame.draw.line(screen, GREEN, (x + char_width // 2 - 20, y + 50), (x + char_width // 2 + 20, y + 50), 5)
    # Legs
    pygame.draw.line(screen, GREEN, (x + char_width // 2 - 15, y + 80), (x + char_width // 2 - 15, y + 120), 5)
    pygame.draw.line(screen, GREEN, (x + char_width // 2 + 15, y + 80), (x + char_width // 2 + 15, y + 120), 5)

# Obstacle drawing
def draw_obstacle(obstacle_x, obstacle_y):
    pygame.draw.rect(screen, obstacle_color, (obstacle_x, obstacle_y, obstacle_width, obstacle_height))

# Game Over screen
def game_over_screen():
    screen.fill(WHITE)
    over_text = font.render(f"Game Over! Score: {player_score}", True, BLACK)
    screen.blit(over_text, [screen_width // 4, screen_height // 3])
    pygame.display.update()
    time.sleep(2)

# Main Game Loop
def game_loop():
    global player_y, is_jumping, jump_count, player_score, is_game_over, game_speed, obstacle_timer

    running = True
    clock = pygame.time.Clock()

    while running:
        # Draw background
        screen.blit(background_image, (0, 0))
        
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        # Get keys
        keys = pygame.key.get_pressed()

        # Player jumping mechanics
        if keys[pygame.K_SPACE] and not is_jumping:
            is_jumping = True

        if is_jumping:
            if jump_count >= -jump_height:
                neg = 1 if jump_count > 0 else -1
                player_y -= (jump_count ** 2) * 0.5 * neg
                jump_count -= 1
            else:
                jump_count = jump_height
                is_jumping = False

        # Handle obstacles
        if obstacle_timer == 0:
            obstacle_x = screen_width
            obstacle_y = ground_y
            obstacles.append([obstacle_x, obstacle_y])
            obstacle_timer = obstacle_interval
        else:
            obstacle_timer -= 1

        for obstacle in obstacles[:]:
            obstacle[0] -= game_speed  # Move obstacle to the left, speed increases over time

            # Draw each obstacle
            draw_obstacle(obstacle[0], obstacle[1])

            # Check for collision
            if player_x + char_width > obstacle[0] and player_x < obstacle[0] + obstacle_width:
                if player_y + char_height > obstacle[1]:
                    is_game_over = True

            # Remove obstacles that are off-screen
            if obstacle[0] < -obstacle_width:
                obstacles.remove(obstacle)
                player_score += 1  # Increment score for every successful jump over an obstacle

        # Draw player
        draw_toy_character(player_x, player_y)

        # Display score
        score_text = small_font.render(f"Score: {player_score}", True, BLACK)
        screen.blit(score_text, (10, 10))

        # If game over, show Game Over screen and reset
        if is_game_over:
            game_over_screen()
            running = False

        # Increase game speed more rapidly over time
        game_speed += 0.02  # Speed increases faster

        # Update display and control frame rate
        pygame.display.update()
        clock.tick(30)

    pygame.quit()

# Start the game
game_loop()



```
### Output:
![image](https://github.com/user-attachments/assets/01dfc502-6c0e-4ffd-aef8-9e2ca03a0114)



### Result:
Thus the simple  game was implemented using Rule based algorithm
