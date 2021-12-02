from pygame import *
from random import randint

window = display.set_mode((700, 500))
display.set_caption('tennis')
background = transform.scale(image.load('tennis.jpg'), (700, 500))

clock = time.Clock()
finish = False
FPS = 60

player = Player('ракетка1.png', 30, 40, 10)
enemy = Enemy('ракетка2.png',300,100,5)
game = True

font.init()
shrift = font.Font(None, 70)
loose = shrift.render('YOU LOOSE', True, (255,215,0))
 
finish = False


shrift = font.Font(None, 70)
win = shrift.render('YOU WIN!', True, (255,215,0))

while game:
    for e in event.get():
        if e.type == QUIT:
           game = False

    if finish!=True:
        window.blit(background, (0, 0))

        player.reset()
        player.update()

        enemy.reset()
        enemy.update()

        final.reset()

        if sprite.collide_rect(player, final):
            finish = True
            window.blit(win, (200,200))
            money.play()
        if sprite.collide_rect(player, enemy):
            finish = True
            window.blit(lose, (200,200))

keys_pressed = key.get_pressed()
class GameSprite(sprite.Sprite):
    def init(self, player_image, player_x, player_y, player_speed):
        super().init()
        self.image = transform.scale(image.load(player_image), (75, 75))
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
        self.speed = player_speed
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def update(self):
        keys_pressed = key.get_pressed()
        if keys_pressed[K_a] and self.rect.x >= 10:
            self.rect.x -= self.speed
        if keys_pressed[K_d] and self.rect.x <= 640:
            self.rect.x += self.speed
    def shoot(self):
        for projectile in projectiles:
            projectile.shooot()

class Enemy(GameSprite):
    def vertical(self):
        self.rect.y += self.speed
        if self.rect.y >= 520:
            self.rect.x = randint(1, 610)
            self.rect.y = 1
            self.speed = randint(1, 2)
            global numbah2
            numbah2 += 1

class Bullet(sprite.Sprite):
    def init(self, player_image, player_x, player_y, player_speed):
        self.image = transform.scale(image.load(player_image), (20, 20))
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
        self.speed = player_speed
    def move(self): 
        self.rect.y -= self.speed
    def shooot(self):
        window.blit(self.image, (self.rect.x, self.rect.y))


class Portal(sprite.Sprite):
    def init(self, height, width, x_cor, y_cor, color_1, color_2, color_3, speed, direction):
        super().init()
        self.color_1 = color_1
        self.color_2 = color_2
        self.color_3 = color_3
        self.width = width
        self.height = height
        self.image = Surface((self.width, self.height))
        self.image.fill((color_1, color_2, color_3))
        self.rect = self.image.get_rect()
        self.rect.x = x_cor
        self.rect.y = y_cor
        self.speed = speed

projectiles = []
ufos = []

hero = Player('rocket.png', 350, 420, 4)
super_bullet = Bullet('bullet.png', hero.rect.x + 28, hero.rect.y, 12)

game = True

font.init()
font1 = font.Font(Arial, 30)
font2 = font.Font(Arial, 70)
shot = font1.render('shot:' + str(numbah1), True, (255, 105, 0))
missed = font1.render('missed: ' + str(numbah2), True, (255, 0, 140))
rules = font1.render('Тебе нужно убить 10 инопланетян и у тебя есть 3 жизни. ', True, (105, 220, 140))
again = font1.render('press e to play again', True, (255, 150, 40))

win = font2.render('you win', True, (255, 175, 0))
lose = font2.render('you lose', True, (255, 105, 40))

for projectile in projectiles:
    projectile.shooot()
    projectile.move()



for ufo in ufos:
    for projectile in projectiles:
        if sprite.collide_rect(projectile, ufo):
            numbah1 += 1
            ufo.rect.x = randint(1, 610)
            ufo.rect.y = 1
            ufo.speed = randint(1, 2)

            projectiles.remove(projectile)



for ufo in ufos:
    if sprite.collide_rect(hero, ufo):
            finish = True
            window.blit(lose, (250, 220))
            window.blit(again, (250, 270))
    keys_pressed = key.get_pressed()

while game:
    window.blit(background, (0, 0))
    for e in event.get():
        if e.type == QUIT:
            game = False
    if finish != True:

        window.blit(shot, (1, 30))
        window.blit(missed, (1, 60))
        window.blit(rules, (100, 30))

        shot = font1.render('shot:' + str(numbah1), True, (255, 105, 0))
        missed = font1.render('missed: ' + str(numbah2), True, (255, 0, 140))

    
        hero.update()
        hero.reset()
        keys_pressed = key.get_pressed()
        if keys_pressed[K_LCTRL]:
            if numbah1 >= 5:
                window.blit(super_bullet.image, (super_bullet.rect.x, super_bullet.rect.y))
                super_bullet.move()
                if super_bullet.rect.y < 0:
                    super_bullet.kill()
                    numbah1 -= 5      

        if keys_pressed[K_SPACE]:
            hero.shoot()
            fire.play()

            if len(projectiles) < 6:
                    projectiles.append(Bullet('bullet.png', hero.rect.x + 28, hero.rect.y, 6))
            for projectile in projectiles:
                if projectile.rect.y > 0:
                    projectile.move()
                else:
                    projectiles.remove(projectile)


        if len(ufos) < numbah_of_ufos:
            ufos.append(Enemy('ufo.png', randint(1, 610), 1, randint(1, 2)))
        for ufo in ufos:
            ufo.vertical()
            ufo.reset()
            if sprite.collide_rect(hero, ufo):
                finish = True
                window.blit(lose, (200, 200)) 

        if numbah1 >= 10 or numbah2 >= 3:
            finish = True


    if numbah1 >= 25:
        window.blit(win, (250, 220))
        window.blit(again, (250, 270))
    elif numbah2 >= 10:
        window.blit(lose, (250, 220))
        window.blit(again, (250, 270))

    if keys_pressed[K_e]:
        finish = False
        numbah1 = 0
        numbah2 = 0
        for ufo in ufos:
            ufo.rect.x = randint(1, 610)
            ufo.rect.y = 1
            ufo.speed = randint(1, 2)
    display.update()
    clock.tick(FPS)
