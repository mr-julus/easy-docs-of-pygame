# Basic Functionalities of Pygame
```python
import pygame
import random
import time

## INIT
pygame.init()

## SETTINGS
screen = pygame.display.set_mode((1280, 720))
pygame.display.set_caption("Pygame Window")
font = pygame.font.SysFont('Consolas', 40)
clock = pygame.time.Clock()

## SET MOUSE VISIBLE
pygame.mouse.set_visible(False)                                             ### true or false

## GET MOUSE VISIBLE
pygame.mouse.get_visible()

## SET COLORS
RED = (255, 0, 0)
ORANGE = (255, 165, 0)
YELLOW = (255, 255, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
PURPLE = (128, 0, 128)
PINK = (255, 192, 203)
GRAY = (128, 128, 128)
BLACK = (0, 0, 0)
WHITE = (255, 255, 255) 
BROWN = (165, 42, 42) 

## SET A PICTURE AS A BACKGROUND
background = pygame.image.load("assets/background.png")

## SET A SOUND
sound = pygame.mixer.Sound('jump.wav')

## CREATE A TEXT
text = font.render('Score: 100', True, (0, 0, 0))

## CREATE A BUTTON
class CreateButton:
    def __init__(self, x, y, largeur, hauteur, couleur, text):
        self.x = x
        self.y = y
        self.largeur = largeur
        self.hauteur = hauteur
        self.couleur = couleur
        self.text = font.render(text, True, (255, 255, 255))

button = CreateButton(640, 360, 100, 50, (255, 255, 255), "A Button")
button_rect = pygame.Rect(button.x, button.y, button.largeur, button.hauteur)

## CREATE A SPRITE
class CreateSprite(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.image.load("assets/sprite.png")
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, 1280 - self.rect.width)
        self.rect.y = random.randint(0, 720 - self.rect.height)
        self.speed_x = random.choice([-5, 5])
        self.speed_y = random.choice([-5, 5])

    ## MOVE THE SPRITE
    def update(self):
        self.rect.x += self.speed_x
        self.rect.y += self.speed_y

        ## BOUNCE ON THE EDGES OF THE SCREEN
        if self.rect.left < 0 or self.rect.right > 1280:
            self.speed_x = -self.speed_x
        if self.rect.top < 0 or self.rect.bottom > 720:
            self.speed_y = -self.speed_y 

        ## CHECK COLLISION WITH OTHER SPRITES
        for other_sprite in all_sprites:
            if other_sprite != self and pygame.sprite.collide_rect(self, other_sprite):
                self.handle_collision(other_sprite)

    ## HANDLE COLLISION
    def handle_collision(self, other_sprite):
        self.speed_x, other_sprite.speed_x = other_sprite.speed_x, self.speed_x
        self.speed_y, other_sprite.speed_y = other_sprite.speed_y, self.speed_y

all_sprites = pygame.sprite.Group()

## CREATE A TIMER
start = time.time()

running = True
while running:
    
    ## DRAW THE BACKGROUND, THE TEXT AND THE BUTTON ON THE SCREEN
    screen.fill("purple")
    screen.blit(background, (0, 0))

    screen.blit(text, (10, 10))

    pygame.draw.rect(screen, button.couleur, button_rect)
    screen.blit(button.text, (button.x + 10, button.y + 10))

    ## DRAW A RECTANGLE
    pygame.draw.rect(screen, RED, (100, 100, 200, 100))                     ### x, y, largeur, hauteur

    ## DRAW A CIRCLE
    pygame.draw.circle(screen, BLUE, (400, 300), 50)                        ### position, rayon

    ## DRAW ALL SPRITES
    all_sprites.draw(screen)

    ## UPDATE THE SPRITES
    all_sprites.update()

    ## CHECK THE EVENTS
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        ## IF KEY IS DOWN
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                if time.time() - start >= 3000:
                    print("The key 'SPACE' is down after 3 seconds")
                else:
                    print("The key 'SPACE' is down!")
            
            ## ADD AND DELETE A SPRITE
            elif event.key == pygame.K_a:
                sprite = CreateSprite()
                all_sprites.add(sprite)
            elif event.key == pygame.K_d:
                if all_sprites:
                    sprite = all_sprites.sprites()[0]
                    all_sprites.remove(sprite)

            ## REMOVE ALL SPRITES
            elif event.key == pygame.K_s:
                all_sprites.empty()

            ## PLAY A SOUND
            elif event.key == pygame.K_ESCAPE:
                sound.play()

        ## IF MOUSE CLICK
        if event.type == pygame.MOUSEBUTTONDOWN:
            ## MOUSE POSITION
            mouse_pos = pygame.mouse.get_pos()
            print(mouse_pos)

            ## IF MOUSE CLICK A BUTTON
            if button_rect.collidepoint(mouse_pos):
                print("Button at position " + str(mouse_pos) + " has been clicked!")

    ## IF A KEY IS PRESSED
    keys = pygame.key.get_pressed()
    if keys[pygame.K_w]:
        print("The key 'W' has been pressed!")

    ## UPDATE SCREEN AND SET A FPS
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
```
> avaible online : ```https://mr-julus.is-a.dev/pygame/pygame_functionalities.py```
