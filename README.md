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
