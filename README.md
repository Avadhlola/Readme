import pygame
import random

# Initialize pygame
pygame.init()

# Screen dimensions
WIDTH = 800
HEIGHT = 300
SCREEN = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Dinosaur Game")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Clock and font
clock = pygame.time.Clock()
font = pygame.font.SysFont(None, 36)

# Dinosaur class
class Dinosaur:
    def __init__(self):
        self.image = pygame.Surface((40, 40))
        self.image.fill((0, 200, 0))
        self.rect = self.image.get_rect()
        self.rect.x = 50
        self.rect.y = HEIGHT - 90
        self.jump_speed = 0
        self.is_jumping = False

    def update(self):
        if self.is_jumping:
            self.jump_speed += 1
            self.rect.y += self.jump_speed
            if self.rect.y >= HEIGHT - 90:
                self.rect.y = HEIGHT - 90
                self.is_jumping = False
                self.jump_speed = 0

    def jump(self):
        if not self.is_jumping:
            self.is_jumping = True
            self.jump_speed = -15

    def draw(self):
        SCREEN.blit(self.image, self.rect)

# Obstacle class
class Obstacle:
    def __init__(self):
        self.image = pygame.Surface((20, 40))
        self.image.fill((200, 0, 0))
        self.rect = self.image.get_rect()
        self.rect.x = WIDTH
        self.rect.y = HEIGHT - 90

    def update(self):
        self.rect.x -= 10

    def draw(self):
        SCREEN.blit(self.image, self.rect)

# Main game loop
def game_loop():
    dino = Dinosaur()
    obstacles = []
    score = 0
    game_over = False
    spawn_event = pygame.USEREVENT + 1
    pygame.time.set_timer(spawn_event, 1500)

    while not game_over:
        SCREEN.fill(WHITE)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    dino.jump()
            if event.type == spawn_event:
                obstacles.append(Obstacle())

        # Update and draw dino
        dino.update()
        dino.draw()

        # Update and draw obstacles
        for obstacle in obstacles[:]:
            obstacle.update()
            obstacle.draw()
            if obstacle.rect.right < 0:
                obstacles.remove(obstacle)
                score += 1
            if dino.rect.colliderect(obstacle.rect):
                game_over = True

        # Display score
        score_text = font.render(f"Score: {score}", True, BLACK)
        SCREEN.blit(score_text, (10, 10))

        pygame.display.flip()
        clock.tick(30)

    # Game over screen
    SCREEN.fill(WHITE)
    msg = font.render("Game Over! Press any key to exit", True, BLACK)
    SCREEN.blit(msg, (WIDTH//2

