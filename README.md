```python
import pygame
import random
import sys

# Começar os trabalhos com Pygame
pygame.init()

# Paleta de cores à nossa disposição
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# Dimensões da janela do jogo
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption('Jogo de Moto')

# Controlar o ritmo do jogo
clock = pygame.time.Clock()

# Criando a classe da moto
class Moto:
def __init__(self):
self.image = pygame.image.load('moto.png') # Insira o arquivo da moto aqui
self.x = WIDTH // 2
self.y = HEIGHT - 100
self.width = 50
self.height = 100
self.speed = 5

def move(self, dx):
self.x += dx
if self.x < 0:
self.x = 0
elif self.x > WIDTH - self.width:
self.x = WIDTH - self.width

def draw(self, screen):
screen.blit(self.image, (self.x, self.y))

# Classe para os obstáculos na pista
class Obstaculo:
def __init__(self):
self.width = 50
self.height = 50
self.x = random.randint(0, WIDTH - self.width)
self.y = -50
self.speed = random.randint(3, 7)

def move(self):
self.y += self.speed

def draw(self, screen):
pygame.draw.rect(screen, RED, (self.x, self.y, self.width, self.height))

def off_screen(self):
return self.y > HEIGHT

# A espinha dorsal do jogo
def jogo():
moto = Moto()
obstaculos = []
score = 0
font = pygame.font.SysFont(None, 40)
game_over = False

while not game_over:
screen.fill(WHITE)
for event in pygame.event.get():
if event.type == pygame.QUIT:
pygame.quit()
sys.exit()

# Comandos da moto
keys = pygame.key.get_pressed()
if keys[pygame.K_LEFT]:
moto.move(-moto.speed)
if keys[pygame.K_RIGHT]:
moto.move(moto.speed)

# Criar novos obstáculos
if random.randint(1, 50) == 1:
obstaculos.append(Obstaculo())

# Movimentar e exibir os obstáculos
for obstaculo in obstaculos[:]:
obstaculo.move()
obstaculo.draw(screen)

if obstaculo.off_screen():
obstaculos.remove(obstaculo)
score += 1

# Detectar colisões
if (obstaculo.x < moto.x + moto.width and
obstaculo.x + obstaculo.width > moto.x and
obstaculo.y < moto.y + moto.height and
obstaculo.y + obstaculo.height > moto.y):
game_over = True

# Desenhando a moto
moto.draw(screen)

# Mostrar a pontuação
score_text = font.render(f"Pontuação: {score}", True, BLACK)
screen.blit(score_text, (10, 10))

# Atualizar o display
pygame.display.update()

# Definir taxa de quadros
clock.tick(60)

# Invocar a função principal
if __name__ == "__main__":
jogo()
```
