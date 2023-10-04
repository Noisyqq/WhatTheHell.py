# WhatTheHell.py
#ให้พี่ปลื้มดูเล่นๆให้พี่ปลื้มรู้ว่าทำแล้วนะครับ:>
import pygame
pygame.init()

WIDTH,HEIGHT = 400,700
WIN = pygame.display.set_mode((WIDTH,HEIGHT))

pygame.display.set_caption("sim")

FPS = 20

g = 0.98
WHITE = (255,255,255)
BLACK = (0,0,0)

gravity = 0.8
bounce_factor = 0.8

#Fiction = 0
#H = int(input("Height : "))
#LR = input("Left or Right : ")
#POWER = int(input("Power : "))

class Floor:
    COLOR = WHITE

    def __init__(self, x, y, width, height):
        self.x = self.original_x = x
        self.y = self.original_y = y
        self.width = width
        self.height = height

    def draw(self, win):
        pygame.draw.rect(
            WIN, self.COLOR, (self.x, self.y, self.width, self.height))

run = True

class Ball:
  MAX_VEL = 3
  COLOR = WHITE

  def __init__(self,x,y,radius):
    self.x = self.original_x = x
    self.y = self.original_y = y
    self.radius = radius
    self.x_vel = self.MAX_VEL
    self.y_vel = 0

  def draw(self,win):
    pygame.draw.circle(win,self.COLOR,(self.x,self.y),self.radius)

  def update(self):
    self.y_vel += g
    self.y += self.y_vel
    if self.y - self.radius < 0:
      self.y = self.radius
      self.y_vel = self.y_vel * bounce_factor * -1
    elif self.y + self.radius > 600:
      self.y = 600 - self.radius
      self.y_vel = self.y_vel * bounce_factor * -1
      
  def reset(self):
    self.x = self.original_x
    self.y = self.original_y
    self.y_vel = 0


def handle_collision(ball,win):
  if ball.y + ball.radius >= HEIGHT:
        ball.y_vel *= -1
  elif ball.y - ball.radius <= 0:
        ball.y_vel *= -1

  if ball.x_vel < 0:
     if ball.y >= floor.y and ball.y <= floor.y + floor.height:
       if ball.x - ball.radius <= floor.x + floor.width:
                ball.x_vel *= -1

  else:
        if ball.y >= right_paddle.y and ball.y <= right_paddle.y + right_paddle.height:
            if ball.x + ball.radius >= right_paddle.x:
                ball.x_vel *= -1

                middle_y = floor.y + floor.height / 2
                difference_in_y = middle_y - ball.y
                reduction_factor = (floor.height / 2) / ball.MAX_VEL
                y_vel = difference_in_y / reduction_factor
                ball.y_vel = -1 * y_vel


ball = Ball(WIDTH//2,0,20)     
clock = pygame.time.Clock()
floor = Floor(0,500,400,200)

while run:
  clock.tick(FPS)
  WIN.fill(BLACK)
  ball.draw(WIN)
  floor.draw(WIN)
  pygame.draw.rect(WIN,(255,255,255),(0,600,400,100))

  for event in pygame.event.get():
    if event.type == pygame.QUIT:
      run = False
      break

  ball.update()
  pygame.display.update()

pygame.quit()
