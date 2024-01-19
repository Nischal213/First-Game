import pygame
import random

pygame.init()
screen = pygame.display.set_mode((800, 600))
running = True

img_player = pygame.image.load("player.png")
player_x = 370
player_y = 480
change_player_x = 0

img_enemy = pygame.image.load("enemy.png")
enemy_x = random.randint(0, 800)
enemy_y = random.randint(50, 150)
change_enemy_x = 0.3

img_bullet = pygame.image.load("bullet.png")
bullet_y = 470
bullet_x = 386
create_bullet = False
change_bullet_y = 0.5


def player():
    screen.blit(img_player, (player_x, player_y))


def enemy():
    screen.blit(img_enemy, (enemy_x, enemy_y))


def bullet():
    screen.blit(img_bullet, (bullet_x, bullet_y))


def collision():
    distance = ((bullet_x - enemy_x) ** 2 + (bullet_y - enemy_y) ** 2) ** 1 / 2
    return distance


while running:
    screen.fill((0, 0, 0))
    for events in pygame.event.get():
        if events.type == pygame.QUIT:
            running = False
        elif events.type == pygame.KEYDOWN:
            if events.key == pygame.K_a:
                change_player_x = -0.3
            elif events.key == pygame.K_d:
                change_player_x = 0.3
            elif events.key == pygame.K_SPACE:
                if not (create_bullet):
                    create_bullet = True
                    bullet_x = player_x
                    change_bullet_y = 0.5
        elif events.type == pygame.KEYUP:
            if events.key == pygame.K_a or events.key == pygame.K_d:
                change_player_x = 0

    player_x += change_player_x
    if player_x <= 0:
        player_x = 0
    elif player_x >= 736:
        player_x = 736
    player()

    enemy_x += change_enemy_x
    if enemy_x <= 0:
        enemy_x = 0
        change_enemy_x = 0.3
        enemy_y += 10
    elif enemy_x >= 736:
        enemy_x = 736
        change_enemy_x = -0.3
        enemy_y += 10
    enemy()

    if create_bullet:
        bullet_y -= change_bullet_y
        bullet()
        if bullet_y <= 0:
            create_bullet = False
            change_bullet_y = 0
            bullet_y = 470
            bullet_x = 386

    if collision() < 27:
        enemy_y = 0
        enemy_x = 0

    pygame.display.update()
