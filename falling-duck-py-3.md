# Falling Duck Python Level 3
### @explicitHints true

## Falling Duck! @showdialog

Enjoy building the Level 3 version of *Falling Duck*!

*   In level 1, we had two sets of log images selected with an
``||logic(noclick):if-else||`` statement.
*   In level 2, we restructured the code to make it easier to add new log images.   
We also added an animation to the duck sprite when it is flying upward.
*   In level 3, we will add multiple levels to the game. As the player scores points,
the game will increase in difficulty by making logs appear more quickly.

Have fun!

![Sample of gameplay](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_demo.gif)

## More parallel lists!

To add multiple levels to the game, we will use two more parallel lists. One list will hold the frequencies at which projectiles appear for each level. The other list will hold the starting score required to reach each level.

1.  In the **Constants** section, create two new constants named **`LEVEL_PROJECTILE_FREQUENCIES`** and **`LEVEL_STARTING_SCORES`**.
1.  Set their types to **`List[number]`**.
1.  Set the values in the *`LEVEL_PROJECTILE_FREQUENCIES`* list to   
**`1500, 1300, 1100, 900, 700`**.
1.  For now, set the values in the *`LEVEL_STARTING_SCORES`* list to some test values, like   
**`0, 3, 6, 9, 12`**.   
We will adjust these values later.

Notice that these two lists form a pair of parallel lists. The first value in each list corresponds to level 0, which starts when the player has a score of zero, and projectiles will appear every 1,500 milliseconds. The second value in each list corresponds to level 1, and so on.

Nothing will change in your game yet, but check the hint to make sure your code looks right.

#### ~ tutorialhint

```python
# Constants
const LEVEL_PROJECTILE_FREQUENCIES: number[] = [ 1500, 1300, 1100, 900, 700, ]
const LEVEL_STARTING_SCORES: number[] = [ 0, 3, 6, 9, 12, ]
```

## Create projectiles based on level

To create projectiles based on the current level, we need to do a little prep work.

1.  Create a new function named **`create_projectiles`**. See the box below if you need help with creating a function.
1.  Move the code that creates the projectiles from the   
``||game(noclick):game.on_update_interval()||`` event handler function into the new function.
1.  Delete the entire   
``||game(noclick):game.on_update_interval||`` event handler (both the function and the event handler call).
1.  In the **Global Variables** section, create two new variables named **`level`** and **`next_projectile_time`**.
1.  Set both new variables to `0` and give them appropriate types.

~hint How do I create a function?

Use the keyword **`def`** to create a named function.

```python
def createProjectiles():
    pass
```

The empty parentheses after the name of the function indicates
that the function does not take parameters.

The body of the function is indented beneath the function's definition.

Functions in Python **must** include **at least one line**. If you don't
want to write the body of your function yet, you can use the **`pass`**
statement to create an "empty" function.

hint~

The `level` variable keeps track of the current level of the game. The `next_projectile_time` variable determines when to create the next set of projectiles.

---

Now, let's create projectiles based on the current level.

1.  In the ``||game(noclick):game.on_update()||`` function, add an   
``||logic:if (true) then||``   
statement. Anywhere inside the event handler function is fine; feel free to add it to the end.
1.  Use code snippets from the ``||game:Game||``, ``||variables:Variables||``, and ``||logic:Logic||`` drawers if needed to create the following condition:   
``||game:time since start (ms)||``
``||logic:>=||``
``||variables:next_projectile_time||``
1.  Within the ``||logic(noclick):if||`` statement, call your new **`create_projectiles`** function.
See the box below if you need help with calling a function.
1.  Finally, we need to update the *`next_projectile_time`* variable each time we create projectiles.    
At the **bottom** of your **`create_projectiles`** function, set the **`next_projectile_time`** variable to:   
``||game:time since start (ms)||``
``||math:+||``
``||variables(arrays):LEVEL_PROJECTILE_FREQUENCIES||``
``||arrays:get value at||``
``||variables(arrays):level||``
1.  Remember that we need to tell a function when we are changing the value
of a global variable. At the **top** of your
``||functions(noclick):create_projectiles||``   
function, indicate that *`next_projectile_time`* is a **global** variable.

~hint How do I call a function?

To call a function in Python, simply enter the name of the function
followed by a set of parentheses. If your function takes parameters,
you would place the parameter values inside of the parentheses.

```python
createProjectiles()
```

hint~

Restart the simulator.

Your game should still function the same as before, but now projectiles are created based on the current level.

We will add code to increase the level in the next step.

Check the hint if you need help.

#### ~ tutorialhint

```python
# Global variables
level: number = 0
next_projectile_time: number = 0

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

def on_update():
    ## Existing code is here ##
    if game.runtime() >= next_projectile_time:
        create_projectiles()

game.on_update(on_update)
```

## Level up!

Now, let's add code to increase the level as the player scores points.

1.  In the ``||game(noclick):on game update||`` event handler, add another   
``||logic:if (true) then||``   
statement. Anywhere inside the event handler function is fine; feel free to add it to the bottom.
1.  Use the ``||logic:Logic||``, ``||math:Math||``, ``||variables:Variables||``, and ``||arrays:Arrays||`` drawers to create the following condition:   
``||variables:level||``
``||logic:<||``
``||arrays:length of array||``
``||variables(arrays):LEVEL_STARTING_SCORES||``
``||math:- (1)||``
1.  Inside of this ``||logic(noclick):if||`` statement, add another   
``||logic:if (true) then||``   
statement.
1.  Use the ``||info:Info||``, ``||arrays:Arrays||``, ``||math:Math||``, and ``||variables:Variables||`` drawers to create the following condition:   
``||info:score||``
``||logic:>=||``
``||variables(arrays):LEVEL_STARTING_SCORES||``
``||arrays:get value at||``
``||variables:level||``
``||math:+ (1)||``
1.  Inside of this new ``||logic(noclick):if||`` statement, add a statement that plays a sound effect. You can find one in the ``||music:Music||`` drawer.
1.  Be sure to change the *`PlaybackMode`* to **`IN_BACKGROUND`**.
1.  Below the sound effect block, add a statement that increments *`level`* by 1.
1.  Finally, give the player an extra life when they level up. Use the ``||info:Info||`` drawer to   
``||info:change life by (-1)||``   
Be sure to change the default value to **`1`**.

~hint How do I change the sound effect?

By default, when using the code snippet from the toolbox, the
`music.play()` function plays the **`music.ba_ding`** sound effect.

To see the other options, delete the *`ba_ding`* constant name, and then delete
the period before it. Type the period **(.)** again. When you do so,
the AutoComplete feature will display a menu with available options.
Scroll to the bottom of the list to see the other playable sounds.

hint~

Review the code that we just built.

1.  First, we check whether we have new levels to reach.
1.  If so, we check whether the player's score is high enough to reach the next level.
1.  If the player has enough points, we play a sound effect, increase the level by 1, and give the player an extra life.

Restart the simulator.

Play the game and try to score points. As your score increases, you should hear a sound effect and gain an extra life each time you reach a new level. The logs should also start appearing more quickly.

Check the hint if you need help.

#### ~ tutorialhint

```python
def on_update():
    global is_animated, level
    ## Existing code is here. ##
    if level < LEVEL_PROJECTILE_FREQUENCIES.length - 1:
        if info.score() >= LEVEL_PROJECTILE_FREQUENCIES[level + 1]:
            music.play(music.melody_playable(music.power_up), music.PlaybackMode.IN_BACKGROUND)
            level = level + 1
            info.change_life_by(1)
game.on_update(on_update)
```

## Set final starting scores

Now that everything is working, let's set the final starting scores for each level.

1.  In the **Constants** section, change the values in the
*`LEVEL_STARTING_SCORES`* list to   
**`0, 15, 30, 45, 60`**.

Restart the simulator.

Play the game to see the new values in action!

#### ~ tutorialhint

```python
# Constants
LEVEL_PROJECTILE_FREQUENCIES: List[number] = [ 1500, 1300, 1100, 900, 700, ]
# @highlight
LEVEL_STARTING_SCORES: List[number] = [ 0, 15, 30, 45, 60, ]
```

## Congratulations! @showdialog

You did it! You built *Falling Duck* Level 3!

Continue to the next level to add more features to your game,
or give these enhancements a try on your own:

*   Add more log images to the lists to increase variety. Try other obstacles, too!
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
TOP_IMAGES: List[Image] = [
    sprites.duck.log7,
    sprites.duck.log6,
    sprites.duck.log5,
    sprites.duck.log8,
]

# Global variables
hero_sprite: Sprite = None
is_animated: bool = False
projectile_vx: number = -45

# Functions

# Event handlers
def on_a_pressed():
    hero_sprite.vy = -35
controller.any_button.on_event(ControllerButtonEvent.PRESSED, on_a_pressed)

def on_update():
    global is_animated
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
game.on_update(on_update)

def on_update_interval():
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
game.on_update_interval(1500, on_update_interval)

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
