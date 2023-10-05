import pygame
import random

# Inicialización de Pygame
pygame.init()

# Configuración de la pantalla
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Ping Pong Game")

# Colores
white = (255, 255, 255)
black = (0, 0, 0)

# Barra del jugador
player_width = 10
player_height = 100
player_x = 50
player_y = (screen_height - player_height) // 2
player_speed = 5

# Barra de la computadora
computer_width = 10
computer_height = 100
computer_x = screen_width - 50 - computer_width
computer_y = (screen_height - computer_height) // 2

# Pelota
ball_width = 10
ball_x = screen_width // 2 - ball_width // 2
ball_y = screen_height // 2 - ball_width // 2
ball_speed_x = 7 * random.choice((1, -1))
ball_speed_y = 7 * random.choice((1, -1))

# Puntuación
player_score = 0
computer_score = 0
font = pygame.font.Font(None, 36)

def show_score(x, y):
    score = font.render("Player: {}  Computer: {}".format(player_score, computer_score), True, white)
    screen.blit(score, (x, y))

# Función principal del juego
def game_loop():
    global ball_speed_x, ball_speed_y, player_score, computer_score

    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        keys = pygame.key.get_pressed()
        if keys[pygame.K_UP] and player_y > 0:
            player_y -= player_speed
        if keys[pygame.K_DOWN] and player_y < screen_height - player_height:
            player_y += player_speed

        # Movimiento de la pelota
        ball_x += ball_speed_x
        ball_y += ball_speed_y

        # Colisión con las paredes superior e inferior
        if ball_y <= 0 or ball_y >= screen_height - ball_width:
            ball_speed_y = -ball_speed_y

        # Colisión con las barras
        if ball_x <= player_x + player_width and player_y <= ball_y <= player_y + player_height:
            ball_speed_x = -ball_speed_x
            player_score += 1
        elif ball_x + ball_width >= computer_x and computer_y <= ball_y <= computer_y + computer_height:
            ball_speed_x = -ball_speed_x
            computer_score += 1

        # Puntuación máxima para ganar
        if player_score >= 5 or computer_score >= 5:
            running = False

        # Dibujar la pantalla
        screen.fill(black)
        pygame.draw.rect(screen, white, [player_x, player_y, player_width, player_height])
        pygame.draw.rect(screen, white, [computer_x, computer_y, computer_width, computer_height])
        pygame.draw.ellipse(screen, white, [ball_x, ball_y, ball_width, ball_width])
        show_score(20, 20)

        pygame.display.update()

    pygame.quit()
    quit()

if __name__ == "__main__":
    game_loop()
