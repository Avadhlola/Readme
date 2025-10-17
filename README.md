# Readmeimport pygame
import random

# Initialize
pygame.init()
WIDTH, HEIGHT = 800, 400
screen = pygame.display.set_mode((WIDTH, HEIGHT))
clock = pygame.time.Clock()
font = pygame.font.SysFont(None, 40)

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Player
player = pygame.Rect(100, 300, 50, 50)
gravity = 0
jumping = False

# Obstacles
obstacles = []
spawn_timer = 0

# Score
score = 0

def draw():
    screen.fill(WHITE)
    pygame.draw.rect(screen, BLACK, player)
    for obs in obstacles:
        pygame.draw.rect(screen, BLACK, obs)
    score_text = font.render(f"Score: {score}", True, BLACK)
    screen.blit(score_text, (10, 10))
    pygame.display.flip()

# Game loop
running = True
while running:
    clock.tick(60)
    score += 1

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN and not jumping:
            gravity = -15
            jumping = True

    # Player physics
    gravity += 1
    player.y += gravity
    if player.y >= 300:
        player.y = 300
        jumping = False

    # Spawn obstacles
    spawn_timer += 1
    if spawn_timer > 90:
        obstacles.append(pygame.Rect(WIDTH, 300, 30, 50))
        spawn_timer = 0

    # Move obstacles
    for obs in obstacles:
        obs.x -= 5
    obstacles = [obs for obs in obstacles if obs.x > -30]

    # Collision
    for obs in obstacles:
        if player.colliderect(obs):
            running = False

    draw()

pygame.quit()
