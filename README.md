import pygame
import random

# Инициализация Pygame
pygame.init()

# Настройки экрана
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Catch the Ball")

# Цвета
white = (255, 255, 255)
red = (255, 0, 0)
blue = (0, 0, 255)

# Позиция мяча
ball_radius = 15
ball_x = random.randint(ball_radius, screen_width - ball_radius)
ball_y = -ball_radius
ball_speed = 5

# Позиция игрока
player_width = 100
player_height = 20
player_x = screen_width // 2 - player_width // 2
player_y = screen_height - 50
player_speed = 10

# Счет
score = 0
font = pygame.font.SysFont(None, 36)

# Основной игровой цикл
running = True
clock = pygame.time.Clock()

while running:
    screen.fill(white)

    # Обработка событий
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Движение игрока
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < screen_width - player_width:
        player_x += player_speed

    # Движение мяча
    ball_y += ball_speed
    if ball_y > screen_height:
        ball_x = random.randint(ball_radius, screen_width - ball_radius)
        ball_y = -ball_radius

    # Проверка на столкновение
    if (player_y < ball_y + ball_radius and
            player_x < ball_x < player_x + player_width):
        score += 1
        ball_x = random.randint(ball_radius, screen_width - ball_radius)
        ball_y = -ball_radius

    # Отображение элементов
    pygame.draw.circle(screen, red, (ball_x, ball_y), ball_radius)
    pygame.draw.rect(screen, blue, (player_x, player_y, player_width, player_height))

    # Отображение счета
    score_text = font.render(f'Score: {score}', True, (0, 0, 0))
    screen.blit(score_text, (10, 10))

    # Обновление экрана
    pygame.display.flip()
    clock.tick(30)

# Завершение работы Pygame
pygame.quit()

