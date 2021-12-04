from pygame import *

class GameSprite(sprite.Sprite):
   def init(self, player_x, player_y, player_speed, wight, height):
       self.image = transform.scale(image.load)
 

posY_block = 150
 
posX_circle = 80
posY_circle = 150
 
circle_right = True
circle_top = True
 
speed_x = 3
speed_y = 3

ball = transform.scale(load.image(" "), (60,60))
ball_rect = ball.get_rect()

ball_rect.rect.x
ball_rect.rect.y

screenWidth = 800
screenHeight = 400
 
screen = pygame.display.set_mode((screenWidth, screenHeight))
window.blit(fon,(0,0))
mach.reset()
plat.update()
plat.reset()
plat2.update()
plat2.reset()


display.update()
clock.tick(FPS)
