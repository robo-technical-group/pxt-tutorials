# Falling Duck Python Level 4
### @explicitHints true

## Falling Duck! @showdialog

Enjoy building the Level 4 version of *Falling Duck*!

*   In level 1, we had two sets of log images selected with an
``||logic(noclick):if-else||`` statement.
*   In level 2, we restructured the code to make it easier to add new log images.   
We also added an animation to the duck sprite when it is flying upward.
*   In level 3, we added multiple levels to the game. As the player scores points,
the game will increase in difficulty by making logs appear more quickly.
*   In level 4, we will allow the player to select an initial 
difficulty level. This difficulty level will influence the speed
of the projectiles and the acceleration of the player's fall.

Have fun!

![Sample of gameplay](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_demo.gif)

## Prep work

Before we give the player the ability to select a difficulty,
we need to move some code around.

1.  In the **Functions** section of your code,
create a function called   
``||functions:init_game||``.
1.  Move all of the blocks from the **Main** section of your code
into to your new function.   
**Remember** to **indent** your function's code properly.
1.  Because   
``||functions:init_game||``   
modifies a global variable,
add the following line to the top of your function:   
`global hero_sprite`.
1.  In **Main**, call your new function.
It should be the only line of code in **Main** right now.
1.  Create another function called   
``||functions:update_game||``.
1.  Move all of the blocks from your existing  
``||game(noclick):on_update||``   
event handler function into your new function.
1.  Add a call to your new function to   
``||game(noclick):on_update||``.

Test your game to make sure it still works correctly.

If so, then onward! If not, then use the hint for help.

#### ~ tutorialhint

```python
# Functions
def init_game():
    global hero_sprite
    scene.set_background_color(9)
    effects.blizzard.start_screen_effect()
    info.set_score(0)
    info.set_life(3)
    hero_sprite = sprites.create(sprites.duck.duck3, SpriteKind.player)
    hero_sprite.ay = 100

def update_game():
    global is_animated, level
    if hero_sprite.top < 0 or hero_sprite.bottom > scene.screen_height():
        game.game_over(False)
    if hero_sprite.vy < 0:
        if not is_animated:
            animation.run_image_animation(
                hero_sprite,
                [
                    sprites.duck.duck3,
                    sprites.duck.duck4,
                    sprites.duck.duck5,
                    sprites.duck.duck6,
                    sprites.duck.duck1,
                    sprites.duck.duck2,
                ],
                100, True
            )
            is_animated = True
    else:
        animation.stop_animation(animation.AnimationTypes.ALL, hero_sprite)
        hero_sprite.set_image(sprites.duck.duck3)
        is_animated = False
    if game.runtime() >= next_projectile_time:
        create_projectiles()
    if level < LEVEL_PROJECTILE_FREQUENCIES.length - 1:
        if info.score() >= LEVEL_PROJECTILE_FREQUENCIES[level + 1]:
            music.play(music.melody_playable(music.power_up), music.PlaybackMode.IN_BACKGROUND)
            level = level + 1
            info.change_life_by(1)

# Event handlers
def on_update():
    update_game()
game.on_update(on_update)

# Main
init_game()
```

## Is the game running?

Before we ask the player to select a difficulty, let's create a variable
that indicates whether the game has started. Without this, the game
will try to update before we are ready.

1.  In the **Global variables** section, create a new **bool** variable called   
``||variables:is_game_running||``.   
Set its initial value to   
``||logic:False||``.
1.  In your   
``||game(noclick):on_update||``   
event handler function, call your   
``||functions(noclick):update_game||``   
function **only if** the game is running.
1.  **Remove** the call to   
``||functions(noclick):init_game||``   
from **Main**.

Restart the simulator.

You should see a blank screen with no errors.

Check the hint if you need any help.

#### ~ tutorialhint

```python
# Global variables
is_game_running: bool = False

# Event handlers
def on_update():
    if is_game_running:
        update_game()
game.on_update(on_update)
```

## Menu, please!

Now, let's give the player a choice of difficulty levels.

To do this, we will use the   
``||miniMenu:Mini Menu||``   
extension.

1.  In the **Global variables** section, create a new variable called   
``||variables:difficulty||``.   
Set its type to **`number`** and its initial value to **`0`**.
1.  From the   
``||miniMenu:Mini Menu||``   
drawer, add a   
``||miniMenu:create menu sprite menu item1||``   
snippet to **Main**.   
It should be the only line of code in **Main** right now.
1.  Copy and paste from the existing snippet to add   
**two more** menu items.   
Be sure to separate the arguments to **`create_menu`** with commas.
1.  Give your difficulty levels names, such as   
*Easy*, *Intermediate*, and *Difficult*.
1.  From the   
``||miniMenu:Mini Menu||``   
drawer, add a   
``||miniMenu:run code this on button pressed with selection selectedIndex||``   
snippet to **the bottom** of **Main**.
1.  Change the name of the event handler function to something a bit
more descriptive, such as   
``on_difficulty_selected``.   
Remember to change **both** the function name
**and** the event handler registration.
1.  In the event handler that runs when selecting a difficulty level,
close the menu.   
**Hint**: Use a code snippet from the   
``||miniMenu:Mini Menu||``   
drawer.
1.  Then, set your new   
``||variables:difficulty||``   
variable to   
``||variables(noclick):selectedIndex||``,   
which is one of the parameters of the event handler.
1.  Then, call your   
``||functions:init_game||``   
function.
1.  Finally, set   
``||variables:is_game_running||``   
to   
``||logic:True||``.
1.  Because you are changing global variables, add the following line
to the top of your event handler function:
`global difficulty, is_game_running`.

Restart the simulator.

Try selecting a menu option. **You will generate an error**.

---

Oops! Let's fix that error.

1.  Find your existing   
``||controller(noclick):on [any] button pressed||``   
event handler.
1.  Only set the hero sprite's velocity if the game is running.   
**Hint**: Look at the code for your   
``||game(noclick):on game update||``   
event handler for inspiration.

Restart the simulator.

Now, you should be able to select a difficulty,
and then the game should start.

Check the hint if you need help.

#### ~ tutorialhint

```python
# Global variables
difficulty: number = 0

# Event handlers
def on_a_pressed():
    if is_game_running:
        hero_sprite.vy = -35
controller.any_button.on_event(ControllerButtonEvent.PRESSED, on_a_pressed)

def on_update():
    if is_game_running:
        update_game()
game.on_update(on_update)

# Main
my_menu = miniMenu.create_menu(
    miniMenu.create_menu_item("Easy"),
    miniMenu.create_menu_item("Intermediate"),
    miniMenu.create_menu_item("Difficult")
)
def on_difficulty_selected(selection, selectedIndex):
    global difficulty, is_game_running
    my_menu.close()
    difficulty = selectedIndex
    init_game()
    is_game_running = True
my_menu.on_button_pressed(controller.A, on_difficulty_selected)
```

## Ooh! Parallel lists again!

Now, let's create some lists that will hold our settings for the different difficulties.

1.  In the **Constants** section, create two new lists called   
``||arrays:PLAYER_ACCELERATIONS||``   
and   
``||arrays:PROJECTILE_SPEEDS||``.
1.  Set their types to **`List[number]`**.
1.  Set the values of each array to some appropriate values.   
For `PLAYER_ACCELERATIONS`, try *125*, *100*, and *75*.   
For `PROJECTILE_SPEEDS`, try *-30*, *-45*, and *-60*.

Check the hint if you need any help.

#### ~ tutorialhint

```python
# Constants
PLAYER_ACCELERATIONS: List[number] = [ 125, 100, 75, ]
PROJECTILE_SPEEDS: List[number] = [ -30, -45, -60, ]
```

## Use your lists!

Now that we've setup our lists, we need to use them!

1.  Find the line of code in your   
``||functions(noclick):init_game||``   
function that sets the hero sprite's acceleration.
1.  In place of the existing value, set the acceleration to   
**`PLAYER_ACCELERATIONS[difficulty]`**.
1.  Beneath that line, add a line that sets   
``||variables:projectile_`vx||``   
to   
**`PROJECTILE_SPEEDS[difficulty]`**.
1.  Because you are changing a global variable, add   
``||variables:projectile_vx||``   
to your **global** list for this function.

Restart the simulator.

Now, you should be able to choose a difficulty level
and see a change in gravity and in the speed of the logs!

#### ~ tutorialhint

```python
def init_game():
    global hero_sprite, projectile_vx
    scene.set_background_color(9)
    effects.blizzard.start_screen_effect()
    info.set_score(0)
    info.set_life(3)
    hero_sprite = sprites.create(sprites.duck.duck3, SpriteKind.player)
    # @highlight
    hero_sprite.ay = PLAYER_ACCELERATIONS[difficulty]
    # @highlight
    projectile_vx = PROJECTILE_SPEEDS[difficulty]
```

## Congratulations! @showdialog

You did it! You built *Falling Duck* Level 4!

Continue to the next level to add more features to your game,
or give these enhancements a try on your own:

*   When selecting difficulty, enhance the screen by setting a background color or image.
*   Enhance the difficulty menu by adding a title and custom frame.
*   Add more log images to the arrays to increase variety. Try other obstacles, too!
*   Change the speed and frequency of the projectiles to make the game easier or harder.
*   Add sound effects when the player scores points or loses a life.
*   Change the background color and screen effect to create a different atmosphere.
*   Create your own artwork for the game.
*   Use splash or dialog boxes to add a story to your game.

Share your completed game with friends and family, and have fun playing *Falling Duck*!

Visit the following link for information on sharing your projects:

<https://arcade.makecode.com/share>

```package
blockSettings=github:microsoft/pxt-settings-blocks#v1.0.0
miniMenu=github:riknoll/arcade-mini-menu#v0.0.1
```

```python-template
@namespace
class SpriteKind:
    add_score = SpriteKind.create()

# Constants
BOTTOM_IMAGES: List[Image] = [
    sprites.duck.log2,
    sprites.duck.log3,
    sprites.duck.log4,
    sprites.duck.log1,
]
LEVEL_PROJECTILE_FREQUENCIES: List[number] = [ 1500, 1300, 1100, 900, 700, ]
LEVEL_STARTING_SCORES: List[number] = [ 0, 3, 6, 9, 12, ]
TOP_IMAGES: List[Image] = [
    sprites.duck.log7,
    sprites.duck.log6,
    sprites.duck.log5,
    sprites.duck.log8,
]

# Global variables
hero_sprite: Sprite = None
is_animated: bool = False
level: number = 0
next_projectile_time: number = 0
projectile_vx: number = -45

# Functions
def create_projectiles():
    global next_projectile_time
    score_projectile: Sprite = sprites.create_projectile_from_side(img("""
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
                1 1
            """),
        projectile_vx,
        0)
    score_projectile.top = 0
    score_projectile.set_kind(SpriteKind.add_score)
    image_index: number = randint(0, len(BOTTOM_IMAGES) - 1)
    top_sprite: Sprite = sprites.create_projectile_from_side(TOP_IMAGES[image_index], projectile_vx, 0)
    bottom_sprite: Sprite = sprites.create_projectile_from_side(BOTTOM_IMAGES[image_index], projectile_vx, 0)
    top_sprite.top = 0
    bottom_sprite.bottom = scene.screen_height()
    score_projectile.set_flag(SpriteFlag.INVISIBLE, True)
    next_projectile_time = game.runtime() + LEVEL_PROJECTILE_FREQUENCIES[level]

# Event handlers
def on_a_pressed():
    hero_sprite.vy = -35
controller.any_button.on_event(ControllerButtonEvent.PRESSED, on_a_pressed)

def on_update():
    global is_animated, level
    if hero_sprite.top < 0 or hero_sprite.bottom > scene.screen_height():
        game.game_over(False)
    if hero_sprite.vy < 0:
        if not is_animated:
            animation.run_image_animation(
                hero_sprite,
                [
                    sprites.duck.duck3,
                    sprites.duck.duck4,
                    sprites.duck.duck5,
                    sprites.duck.duck6,
                    sprites.duck.duck1,
                    sprites.duck.duck2,
                ],
                100, True
            )
            is_animated = True
    else:
        animation.stop_animation(animation.AnimationTypes.ALL, hero_sprite)
        hero_sprite.set_image(sprites.duck.duck3)
        is_animated = False
    if game.runtime() >= next_projectile_time:
        create_projectiles()
    if level < LEVEL_PROJECTILE_FREQUENCIES.length - 1:
        if info.score() >= LEVEL_PROJECTILE_FREQUENCIES[level + 1]:
            music.play(music.melody_playable(music.power_up), music.PlaybackMode.IN_BACKGROUND)
            level = level + 1
            info.change_life_by(1)
game.on_update(on_update)

def on_overlap_player_projectile(sprite, otherSprite):
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.change_life_by(-1)
sprites.on_overlap(SpriteKind.player, SpriteKind.projectile, on_overlap_player_projectile)

def on_overlap_player_addscore(sprite, otherSprite):
    sprites.destroy(otherSprite)
    info.change_score_by(1)
sprites.on_overlap(SpriteKind.player, SpriteKind.add_score, on_overlap_player_addscore)

# Main
scene.set_background_color(9)
effects.blizzard.start_screen_effect()
info.set_score(0)
info.set_life(3)
hero_sprite = sprites.create(sprites.duck.duck3, SpriteKind.player)
hero_sprite.ay = 100
```

```ghost
@namespace
class SpriteKind:
    add_score = SpriteKind.create()

# Constants
BOTTOM_IMAGES: List[Image] = [
    sprites.duck.log2,
    sprites.duck.log3,
    sprites.duck.log4,
    sprites.duck.log1,
]
LEVEL_PROJECTILE_FREQUENCIES: List[number] = [ 1500, 1300, 1100, 900, 700, ]
LEVEL_STARTING_SCORES: List[number] = [ 0, 3, 6, 9, 12, ]
PLAYER_ACCELERATIONS: List[number] = [ 125, 100, 75, ]
PROJECTILE_SPEEDS: List[number] = [ -30, -45, -60, ]
TOP_IMAGES: List[Image] = [
    sprites.duck.log7,
    sprites.duck.log6,
    sprites.duck.log5,
    sprites.duck.log8,
]

# Global variables
difficulty: number = 0
hero_sprite: Sprite = None
is_animated: bool = False
is_game_running: bool = False
level: number = 0
next_projectile_time: number = 0
projectile_vx: number = -45

# Functions
def create_projectiles():
    global next_projectile_time
    score_projectile: Sprite = sprites.create_projectile_from_side(img("""."""),
        projectile_vx,
        0)
    score_projectile.top = 0
    score_projectile.set_kind(SpriteKind.add_score)
    image_index: number = randint(0, len(BOTTOM_IMAGES) - 1)
    top_sprite: Sprite = sprites.create_projectile_from_side(TOP_IMAGES[image_index], projectile_vx, 0)
    bottom_sprite: Sprite = sprites.create_projectile_from_side(BOTTOM_IMAGES[image_index], projectile_vx, 0)
    top_sprite.top = 0
    bottom_sprite.bottom = scene.screen_height()
    score_projectile.set_flag(SpriteFlag.INVISIBLE, True)
    next_projectile_time = game.runtime() + LEVEL_PROJECTILE_FREQUENCIES[level]

def init_game():
    global hero_sprite, projectile_vx
    scene.set_background_color(9)
    effects.blizzard.start_screen_effect()
    info.set_score(0)
    info.set_life(3)
    hero_sprite = sprites.create(sprites.duck.duck3, SpriteKind.player)
    hero_sprite.ay = PLAYER_ACCELERATIONS[difficulty]
    projectile_vx = PROJECTILE_SPEEDS[difficulty]

def update_game():
    global is_animated, level
    if hero_sprite.top < 0 or hero_sprite.bottom > scene.screen_height():
        game.game_over(False)
    if hero_sprite.vy < 0:
        if not is_animated:
            animation.run_image_animation(
                hero_sprite,
                [
                    sprites.duck.duck3,
                    sprites.duck.duck4,
                    sprites.duck.duck5,
                    sprites.duck.duck6,
                    sprites.duck.duck1,
                    sprites.duck.duck2,
                ],
                100, True
            )
            is_animated = True
    else:
        animation.stop_animation(animation.AnimationTypes.ALL, hero_sprite)
        hero_sprite.set_image(sprites.duck.duck3)
        is_animated = False
    if game.runtime() >= next_projectile_time:
        create_projectiles()
    if level < LEVEL_PROJECTILE_FREQUENCIES.length - 1:
        if info.score() >= LEVEL_PROJECTILE_FREQUENCIES[level + 1]:
            music.play(music.melody_playable(music.power_up), music.PlaybackMode.IN_BACKGROUND)
            level = level + 1
            info.change_life_by(1)

# Event handlers
def on_a_pressed():
    if is_game_running:
        hero_sprite.vy = -35
controller.any_button.on_event(ControllerButtonEvent.PRESSED, on_a_pressed)

def on_update():
    if is_game_running:
        update_game()
game.on_update(on_update)

def on_overlap_player_projectile(sprite, otherSprite):
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.change_life_by(-1)
sprites.on_overlap(SpriteKind.player, SpriteKind.projectile, on_overlap_player_projectile)

def on_overlap_player_addscore(sprite, otherSprite):
    sprites.destroy(otherSprite)
    info.change_score_by(1)
sprites.on_overlap(SpriteKind.player, SpriteKind.add_score, on_overlap_player_addscore)

# Main
my_menu = miniMenu.create_menu(
    miniMenu.create_menu_item("Easy"),
    miniMenu.create_menu_item("Intermediate"),
    miniMenu.create_menu_item("Difficult")
)
def on_difficulty_selected(selection, selectedIndex):
    global difficulty, is_game_running
    my_menu.close()
    difficulty = selectedIndex
    init_game()
    is_game_running = True
my_menu.on_button_pressed(controller.A, on_difficulty_selected)
```