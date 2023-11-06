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

### map

needs to be below all of the other element
in YSOrtedCameraGroup:
add a surface and a rectangle for the floor => not a sprite
in custom draw => draw it
define a surface and blit it

floor_offset_pos = self.floor_rect.topleft - self.offset
self.display_surface.blit(self.floor_surf, floor_offset_pos)

### Tile class

needs to be more flexible
It needs to be able to accept graphics with various sizes
And it should be able to not accept a graphic at all

On larger object, we want to offset them
=> twice the size of a normal element, subtract TILESIZE from it
self.rect = self.image.get_rect(topleft= (pos[0], pos[1] - TILESIZE))

### Proper player status

current player status
12 differnt states
4x idle
4x walk
4x attack
=> player animation

1. import player graphics
2. get attack / magic input

pygame does not have a timer function by itself

### State management

player.status = "up" | "left_attack" | "right_idle"...
pick all the surface in the matching folder ("up" | "left_attack" | "right_idle") and play them repeatedly
player.direction.input => player.status

### animations

the player has the same image
=> loop over a list of different images

player attack
=> create weapon strike next to the player
=> destroy it after the attack stops
At first, only weapon is displayed and doesn't do anything

attack to right:
player faces right, spawn weapon sprite at the rigt side of the player

weapon has to be available inside of the level to interact with enemies, grass
problem: attack input inside of the player
solution: def create_attack in level, and pass it to Player()

we need to know what weapon is selected

### UI

2 different elements:

1. data: health, energy, xp, etc...
2. displaying the data (using rects, basically)

converting stat to pixel
ratio = current / max_amount (100 / 100 = 1) | (50 / 100 = 0.5)
current_width = bg_rect.width \* ratio (200 x 1 = 200) | (200 x 0.5 = 100)
