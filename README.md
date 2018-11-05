# MyFirsPytGame
# A Galaga/Space Inaders Knock off, please don't sue me
import pygame
import time
import random

pygame.init()
myfont = pygame.font.SysFont("monospace", 15)

world_size = width, height = 800, 600
BLACK = 0,0,0
WHITE = 255,255,255 
screen = pygame.display.set_mode(world_size)

LeftRect = pygame.Rect(0,0,100,height)
RightRect = pygame.Rect(700,0,100,height)

maindelay = 0
laserlist = []
darklaserlist = []
DarkLaserCap = 2
LastLives = 2
counter = 0

laserbeam = pygame.image.load("NEWlaser.png")
laserbeam_rect = laserbeam.get_rect()

gship = pygame.image.load("gship.png")
gship_rect = gship.get_rect()

eship = pygame.image.load("SpaceInvader1.png")
eship_rect = eship.get_rect()

eship2 = pygame.image.load("SpaceInvader2.png")
eship2_rect = eship2.get_rect()

eship3 = pygame.image.load("SpaceInvader3.png")
eship3_rect = eship3.get_rect()

stage_clear_toggle=False
stage_clear_count=0
stage_clear_MAX=255
shooter_toggle = False
                 

class ship(object):
    """Represents a ship flying in space"""
    def move(self):
        self.rect = self.rect.move(self.speed)
        if player.rect.x < 100:
            player.rect.x = 100
        if player.rect.right > width - 100:
            player.rect.right = width -100
    def __init__(self, x, y):
        self.rect = gship.get_rect()
        self.lives=100
        self.image=gship
        self.speed = [0,0]
        self.rect.x = x
        self.rect.y = y-self.rect.height

class bullet(object):
    """Represents a bullet fired from the players ship"""
    def move(self):
        self.rect = self.rect.move(self.speed)        
    def __init__(self, x, y):
        self.rect = laserbeam_rect
        self.image = laserbeam
        self.speed = [0,-4]
        self.rect.x = x
        self.rect.y = y
    def update(self):
        self.move()
        for x in laserlist:
            if x.rect.bottom < 0:
                laserlist.pop(0)

class darkbullet(object):
    """Represents an evil bullet fired from the enemy ship"""
    def move(self):
        self.rect = self.rect.move(self.speed)        
    def __init__(self, x, y):
        self.rect = laserbeam_rect
        self.image = laserbeam
        self.speed = [0,4]
        self.rect.x = x
        self.rect.y = y
    def update(self):
        self.move()
        for x in darklaserlist:
            if x.rect.top > height:
                darklaserlist.pop(0)
              
class enemy(object):
    """Represents an evil force in the galaxy"""
    def move(self):
        self.rect = self.rect.move(self.speed)
        if self.rect.x < 100 :
            self.speed[0] = -self.speed[0]
        if self.rect.right > width - 100:
            self.speed[0] = -self.speed[0]
    def __init__(self, x, y):
        self.rect = eship_rect
        self.lives = 1
        self.image = eship
        self.speed = [2,0]
        self.rect.x = x
        self.rect.y = y

class enemyFast(object):
    """Represents a speedy evil force in the galaxy"""
    def move(self):
        self.rect = self.rect.move(self.speed)
        if self.rect.x < 100:
            self.speed[0] = -self.speed[0]
        if self.rect.right > width -100:
            self.speed[0] = -self.speed[0]
    def __init__(self, x, y):
        self.rect = eship2_rect
        self.lives = 1
        self.image = eship2
        self.speed = [3,0]
        self.rect.x = x
        self.rect.y = y

class enemyShooter(object):
    """Represents a trigger happy evil force in the galaxy"""
    def move(self):
        self.rect = self.rect.move(self.speed)
        if self.rect.x < 100:
            self.speed[0] = -self.speed[0]
        if self.rect.right > width - 100:
            self.speed[0] = -self.speed[0]
    def __init__(self, x, y):
        self.rect = eship3_rect
        self.lives = 1
        self.image = eship3
        self.sights = (self.rect.centerx, self.rect.centery)
        self.speed = [3,0]
        self.rect.x = x
        self.rect.y = y
        


# Starting positions        
player = ship(124,height)
e1 = enemy(124,0)

go = 1
while go:

    HUD = myfont.render(("Stage "+str(counter+1)), 1, (BLACK))
    HUDLives = myfont.render(("Lives:"+str(player.lives)), 1, (BLACK))
    label = myfont.render("Stage "+ str(counter+1)+" Cleared", 1, (WHITE))
    b = random.randint(1,60)

##############################  MOVE ZONE  ######################################
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            go = 0
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_d: #Move Right
                player.speed[0] = 2
            if event.key == pygame.K_a: #Move Left
                player.speed[0] =-2
            if event.key == pygame.K_SPACE: ## Mah Lazer
                if len(laserlist) > 0:
                    pass
                else:
                    laserlist.append(bullet((player.rect.centerx), (player.rect.centery )))
                    
        if event.type == pygame.KEYUP: ## Allows holding down on keys
            if event.key == pygame.K_d: 
                player.speed[0] = 0
            if event.key == pygame.K_a: 
                player.speed[0] = 0
                
########################  FIRING ZONE  ######################################

    if counter == 1: ## Enemy firing 2
        if badguy.lives <= 0:
            pass
        else:
            if b == DarkLaserCap:
                if len(darklaserlist) > 2:
                    pass
                else:
                    darklaserlist.append(darkbullet((badguy.rect.centerx ), (badguy.rect.centery )))

    if counter == 2: ## Enemy firing 3 
        if b == 2:
            if len(darklaserlist) > 3:
                pass
            else:
                darklaserlist.append(darkbullet((e3.rect.centerx ), (e3.rect.centery )))
    
    if counter >= 3: ## Enemy firing 4
        if e4.lives == 0:
            pass
        else:
            if b == 2 or 3:
                if len(darklaserlist) > DarkLaserCap:
                    pass
                else:
                    darklaserlist.append(darkbullet((e4.rect.centerx ), (e4.rect.centery )))
        
#########################  DEATH ZONE  #####################################

    if counter == 0:        
        for laser in laserlist: ## Stage 1 death 
            if e1.lives != 0:
                if e1.rect.colliderect(laser.rect) == True:
                    e1.lives +=-1
                    if len(laserlist) >0:
                        laserlist.pop(0)
                    if e1.lives == 0:
                        stage_clear_toggle=True
                    
    if counter == 1:
        for laser in laserlist: ## stage 2 death 
            if e2.lives >= 0:
                if e2.rect.colliderect(laser.rect) == True:
                    e2.lives +=-1
                    if len(laserlist) > 0:
                        laserlist.pop(0)
            if badguy.lives >= 0:
                if badguy.rect.colliderect(laser.rect) == True:
                    badguy.lives +=-1
                    if len(laserlist) > 0:    
                        laserlist.pop(0)
            if e2.lives <= 0 and badguy.lives <= 0:
                stage_clear_toggle=True


    if counter >= 3:
        for laser in laserlist: ## stage 4 death 
            if e4.lives != 0:
                if e4.rect.colliderect(laser.rect) == True:
                    e4.lives +=-1
                    if len(laserlist) >1:
                        laserlist.pop(0)
            if e5.lives != 0:
                if e5.rect.colliderect(laser.rect) == True:
                    e5.lives +=-1
                    if len(laserlist) >1:
                        laserlist.pop(0)
            if e4.lives == 0 and e5.lives == 0:
                stage_clear_toggle=True
                
    if counter == 2:
        for laser in laserlist: ## stage 3 death 
            if e3.lives != 0:
                if e3.rect.colliderect(laser.rect) == True:
                    e3.lives += -1
                    if len(laserlist) >1:
                        laserlist.pop(0)
                    if e3.lives == 0:
                        stage_clear_toggle=True

    for laser in darklaserlist: ## Player death
        if player.rect.colliderect(laser.rect) == True:
            player.lives += -1
            if len(darklaserlist) > 1:
                darklaserlist.pop(0)
        if player.lives == 0:
            go = 0
                
 #########################  Laser Moving Zone  ##############################
            
    for laser in laserlist: ## Keeps lasers moving
        laser.update()
    for laser in laserlist: ## Displays lasers
        screen.blit(laser.image, laser.rect)

    for darklaser in darklaserlist: ## Keeps  enemy lasers moving
        darklaser.update()
    for laser in darklaserlist: ## Displays enemy lasers
        screen.blit(laser.image, laser.rect)
        
############################  Move Zone  ###############################      
    player.move()
    
    if e1.lives > 0:
        e1.move()
        
    if counter == 1:
        if e2.lives > 0:
            e2.move()
        if badguy.lives > 0:
            badguy.move()
                    
    if counter == 2:
        if e3.lives > 0:
            e3.move()

    if counter >= 3:
        if e4.lives > 0:
            e4.move()
        if e5.lives > 0:
            e5.move()
###########################  Blit Zone  ###################################
            
    screen.fill(BLACK)
    pygame.draw.rect(screen, WHITE, LeftRect, 0)
    pygame.draw.rect(screen, WHITE, RightRect, 0)
    
    screen.blit(HUD,(25, 25))
    screen.blit(HUDLives,(25,45))
    
    for laser in laserlist:
        screen.blit(laser.image, laser.rect)
    for laser in darklaserlist:
        screen.blit(laser.image, laser.rect)
        
    screen.blit(player.image, player.rect)
    
    if e1.lives > 0: 
        screen.blit(e1.image, e1.rect)
    if counter == 1:
        if e2.lives > 0:
            screen.blit(e2.image, e2.rect)
        if badguy.lives > 0:
            screen.blit(badguy.image, badguy.rect)
    if counter == 2:
        if e3.lives > 0:
            screen.blit(e3.image, e3.rect)
    if counter >= 3:
        if e4.lives > 0:
            screen.blit(e4.image, e4.rect)
        if e5.lives > 0:
            screen.blit(e5.image, e5.rect)
            
##########################  Staging Zone  ####################################
            
    if stage_clear_toggle==True: ## End up level clean up
        screen.blit(label,(300, 350))
        stage_clear_count+=1
        if stage_clear_count>stage_clear_MAX:
            stage_clear_count=0
            stage_clear_toggle=False
            if counter == 0: ## Second stage
                e2 = enemyFast(124,0)
                e2.lives = 1
                badguy = enemy(400,0)
                badguy.lives = 1
                DarkLaserCap += 1
            if counter == 1: ##Third stage
                e3 = enemyShooter(240,0)
                e3.lives = 1
            if counter == 2: ## Fourth stage
                e4 = enemyShooter(240,0)
                e4.lives = 2
                e5 = enemyFast(124,0)
                e5.lives = 2
            if counter >= 3: ## All the rest
                e4.lives = LastLives + 1
                e5.lives = LastLives + 1
                e4.speed[0] +=1
                e5.speed[0] +=1
                DarkLaserCap += 1
            counter +=1
                
            #also, initialize the next stage here
#############################################################################
            
    pygame.display.flip()
    time.sleep(maindelay)

pygame.quit()
if player.lives == 0:
    print "Game Over"
else:
    print "You win!!!"

