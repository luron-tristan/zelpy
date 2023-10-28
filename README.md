# zelpy

Zelda style game with Pygame

## Level class

contains all sprites (player, enemies, map...)
also deals with their interactions
needs to be able to manage hundreds of sprites effectively
=> via groups
sprites can be in multiple groups

two groups:
visible_sprites
group for sprites that will be drawn (only group that draw sprites)

obstacle_sprites
group for sprites that the player can collide with

get the surface from anywhere in the code
pygame.display.get_surface()

direction: pygame.math.Vector2()

### collisions:

- pygame can tell if a collision happened
- but cannot tell which side collided

### camera

both camera and overlap can be achieved by customizing a group
group main design:

- store and draw sprites
- call update method

custom group:

- can add methods or changes to how groups work
  (custom draw method for example)

group.draw => calls blit method

camera:
we draw the image in the rect of the sprite
=> we can use a vector2 to offset the rect and thus blit the image somewhere else
=> for sprite in sprites():
surface.blit(sprite.image, sprite.rect _+ offset_)
(offset comes from the player)

overlap:

- each sprite gets a hitbox for the collision (instead of the image)
  we want the hitbox a bit smaller in y than the image => create depth
- the drawing function needs to know the order of the sprites

self.hitbox = self.rect.inflate(0, -10)
=> reduces by 5 px on each side of y axis
move hitbox instead of rect
put center of rect where hitbox is going to be => rect follows hitbox

order sprites by y position: highest y => on top
