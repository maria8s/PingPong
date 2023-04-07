from pygame import *


win_width = 700
win_height = 500
window = display.set_mode((win_width, win_height))
clock = time.Clock()
FPS = 60


class GameSprite(sprite.Sprite):
    #конструктор класса
    def __init__(self, player_image, player_x, player_y, size_x, size_y, player_speed):
        #Вызываем конструктор класса (Sprite):
        sprite.Sprite.__init__(self)
 
        #каждый спрайт должен хранить свойство image - изображение
        self.image = transform.scale(image.load(player_image), (size_x, size_y))
        self.speed = player_speed
 
        #каждый спрайт должен хранить свойство rect - прямоугольник, в который он вписан
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y


    #метод, отрисовывающий героя на окне
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))




class Player(GameSprite):
    #метод для управления спрайтом стрелками клавиатуры
    def update_l(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_width - 80:
            self.rect.y += self.speed


    def update_r(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < win_width - 80:
            self.rect.y += self.speed


#создания мяча и ракетки  
racket1 = Player('racket.png', 30, 200, 50, 150, 4)
racket2 = Player('racket.png', 520, 200, 50, 150, 4)
ball = GameSprite('tenis_ball.png', 200, 200, 50, 50, 4)


#движение мяча
dx = 3
dy = 3


#надписи
font.init()
font1 = font.Font(None, 70)
lose1 = font1.render('Первый проиграл!', True, (255,255,255))
lose2 = font1.render('Второй проиграл!', True, (255,255,255))


game = True
finish = False
while game:
    for e in event.get():
        if e.type == QUIT:
            game = False


    if finish == False:
        racket1.update_l()
        racket2.update_r()
        ball.rect.x += dx
        ball.rect.y += dy
        if ball.rect.y<0 or ball.rect.y>=win_height - 50:
            dy *= -1


        if sprite.collide_rect(racket1, ball) or sprite.collide_rect(racket2, ball):
            dx *= -1


        window.fill((100,100,100))
        racket1.reset()
        racket2.reset()
        ball.reset()


        if ball.rect.x<=0:
            finish = True
            window.blit(lose1, (200,200))


        if ball.rect.x>=win_width - 50:
            finish = True
            window.blit(lose2, (200,200))


    display.update()
    clock.tick(FPS)

