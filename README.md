import pygame
from pygame.locals import *



win_width = 800
win_height = 600
window = pygame.display.set_mode((win_width, win_height))
pygame.display.set_caption("Pong")

paddle_width = 15
paddle_height = 100
paddle_speed = 15

ball_size = 20
ball_speed_x = 6
ball_speed_y = 6

WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

class Paddle(pygame.sprite.Sprite):
    def __init__(self, x, y, up_key, down_key):
        super().__init__()
        self.image = pygame.Surface((paddle_width, paddle_height))
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.topleft = (x, y)
        self.up_key = up_key
        self.down_key = down_key

    def update(self):
        keys = pygame.key.get_pressed()
        if keys[self.up_key] and self.rect.top > 0:
            self.rect.y -= paddle_speed
        if keys[self.down_key] and self.rect.bottom < win_height:
            self.rect.y += paddle_speed

class Ball(pygame.sprite.Sprite):
    def __init__(self, x, y, size=ball_size, speed_x=ball_speed_x, speed_y=ball_speed_y):
        super().__init__()
        self.image = pygame.Surface((size, size))
        self.image.fill(WHITE)
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)
        self.speed_x = speed_x
        self.speed_y = speed_y

    def update(self):
        self.rect.x += self.speed_x
        self.rect.y += self.speed_y
        if self.rect.top <= 0 or self.rect.bottom >= win_height:
            self.speed_y = -self.speed_y
        if pygame.sprite.collide_rect(self, paddle1) or pygame.sprite.collide_rect(self, paddle2):
            self.speed_x = -self.speed_x
        if self.rect.left <= 0 or self.rect.right >= win_width:
            self.rect.center = (win_width // 2, win_height // 2)
            self.speed_x = -self.speed_x

paddle1 = Paddle(10, win_height // 2 - paddle_height // 2, K_w, K_s)
paddle2 = Paddle(win_width - 25, win_height // 2 - paddle_height // 2, K_UP, K_DOWN)
ball = Ball(win_width // 2, win_height // 2)

all_sprites = pygame.sprite.Group(paddle1, paddle2, ball)

clock = pygame.time.Clock()

run = True
while run:
    for e in pygame.event.get():
        if e.type == QUIT:
            run = False

    paddle1.update()
    paddle2.update()
    ball.update()

    window.fill(BLACK)
    all_sprites.draw(window)

    pygame.display.update()

    clock.tick(60)



