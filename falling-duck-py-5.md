# Falling Duck Python Level 5
### @explicitHints true

## Falling Duck! @showdialog

Enjoy building the Level 5 version of *Falling Duck*!

*   In level 1, we had two sets of log images selected with an
``||logic(noclick):if-else||`` statement.
*   In level 2, we restructured the code to make it easier to add new log images.   
We also added an animation to the duck sprite when it is flying upward.
*   In level 3, we added multiple levels to the game. As the player scores points,
the game will increase in difficulty by making logs appear more quickly.
*   In level 4, we allowed the player to select an initial 
difficulty level. This difficulty level influences the speed
of the projectiles and the acceleration of the player's fall.
*   In level 5, we will customize the high scores routine.

Have fun!

![Sample of gameplay](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_demo.gif)

## Game over?

It doesn't seem fair that players competing at the Difficult level compete
for the same high score that a player earned at the Easy level.

So, let's store high scores for each difficulty level!

We need to do some work to get ready for this.

1.  Create a function called   
``||functions:end_game||``.
1.  For now, just to make sure our code is working as we expect, add a   
``||game:splash||``   
statement to your new function.
1.  Set the splash message to something like   
`"Game over!"`
1.  In your   
``||functions(noclick):update_game||`` function,
replace the existing   
``||game(noclick):game over||``   
line with a call to your new function.

---

We also need to call this function when we run out of lives.

1.  From the ``||info:Info||`` drawer,
drag an   
``||info:on life zero||``   
container into the **Event handlers** section of your code.
1.  In this new event handler function, add a call to your   
``||functions(noclick):end_game||``   
function.

Restart the simulator.

When you lose the game, instead of the standard "Game Over" dialog,
you should see your splash message instead.

Review the code in the hint if you need any help.

#### ~ tutorialhint

```python
def end_game():
    game.splash("Game over!")
def update_game():
    global is_animated, level
    if hero_sprite.top < 0 or hero_sprite.bottom > scene.screen_height():
        # @highlight
        end_game()
def on_life_zero():
    end_game()
info.on_life_zero(on_life_zero)
```

## Customizing the end game

Now, let's customize how the **Game Over** process runs.

Add the following code to your ``||functions(noclick):end_game||`` function.

You can find all of the code snippets in the ``||game:Game||`` drawer.

*   Set the scoring type to   
**`game.ScoringType.NONE`**.   
We will be controlling how the high score is saved ourselves.
*   Set game over **effect**s for both the winning and the losing conditions.
*   Set game over **sound**s for both the winning and the losing conditions.
*   Set the game over **message** to `"NEW HIGH SCORE!"` for a **WIN**.

Check the hint if you need any help.

#### ~ tutorialhint

```python
def end_game():
    game.splash("Game over!")
    game.set_game_over_scoring_type(game.ScoringType.NONE)
    game.set_game_over_effect(True, effects.confetti)
    game.set_game_over_effect(False, effects.melt)
    game.set_game_over_playable(True, music.melodyPlayable(music.powerUp), False)
    game.set_game_over_playable(False, music.melodyPlayable(music.wawawawaa), False)
    game.set_game_over_message(True, "NEW HIGH SCORE!")
```

## High score?

Now, let's save and check the high scores for each difficulty.

Check the hint if you need any help.

1.  First, **delete** the existing ``||game(noclick):splash||`` block,
since we are implementing the real logic now.
1.  At the **bottom** of your   
``||functions(noclick):end_game||``   
function, create a new (local, not global) variable called **`setting_name`**.
1.  Set the type to **`string`** and the value of   
to the string   
**`"HighScore" + difficulty`**.
1.  At the **bottom** of your   
``||functions(noclick):end_game||``   
function, add an   
``||logic:if (True) then else||``   
statement.
1.  Use snippets from the   
``||blockSettings:settings||`` and   
``||info:Info||``   
drawers to build the following conditional:   
``||logic(noclick):if||``
``||blockSettings:setting with name||``
``||variables:settingName||``
``||blockSettings:exists||``
``||logic:and||``
``||info:score||``
``||logic: < ||``
``||blockSettings:read setting||``
``||variables:settingName||``
``||blockSettings:as number||``
---

The conditional tests if the setting exists and, if it does,
whether the current score is lower than the high score
for this difficulty.

If this is the case, then the player has not beaten the high score
for this difficulty.

When this happens, let's give the player a custom message and then
end the game as a loss.

Add the following code to the upper (**`true`**) branch of the   
``||logic(noclick):if||``   
statement:

*   Add a snippet from the   
``||game:Game||``   
drawer that sets the game over **message** to the string   
**`"GAME OVER High Score: "`**   
``||text:+||``
``||blockSettings:read setting||``
``||variables:settingName||``
``||blockSettings:as number||``   
for a **LOSE**.
*  Add a snippet from the   
``||game:Game||``   
drawer that ends the game as **`LOSE`**.

---

If the conditional is not true, then either the settings does not exist
or the player has beaten the previous high score.

When this happens, let's store the current score as the high score
for this difficulty.

Add the following code to the lower (**`false`**) branch of the
``||logic(noclick):if||``   
statement:

*   Add a snippet from the   
``||blockSettings:settings||``   
drawer that stores the current   
``||info:score||``   
as a number using the value of the   
``||variables:settingName||``   
variable as the name of the setting.
*  Add a snippet from the   
``||game:Game||``   
drawer that ends the game as a **`WIN`**.

Restart the simulator.

Play the game and try to beat your high scores for each difficulty setting!

Check the hint if you need any help.

#### ~ tutorialhint

```python
def end_game():
    game.set_game_over_scoring_type(game.ScoringType.NONE)
    game.set_game_over_effect(True, effects.confetti)
    game.set_game_over_effect(False, effects.melt)
    game.set_game_over_playable(True, music.melodyPlayable(music.powerUp), False)
    game.set_game_over_playable(False, music.melodyPlayable(music.wawawawaa), False)
    game.set_game_over_message(True, "NEW HIGH SCORE!")
    setting_name: string = "HighScore" + difficulty
    if blockSettings.exists(setting_name) and info.score() < blockSettings.read_number(setting_name):
        game.set_game_over_message(True, "GAME OVER High Score: " + blockSettings.read_number(setting_name))
        game.game_over(False)
    else:
        blockSettings.write_number(setting_name, info.score())
        game.game_over(True)
```

## Congratulations! @showdialog

You did it! You built *Falling Duck* Level 5!

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

def end_game():
    game.set_game_over_scoring_type(game.ScoringType.NONE)
    game.set_game_over_effect(True, effects.confetti)
    game.set_game_over_effect(False, effects.melt)
    game.set_game_over_playable(True, music.melodyPlayable(music.powerUp), False)
    game.set_game_over_playable(False, music.melodyPlayable(music.wawawawaa), False)
    game.set_game_over_message(True, "NEW HIGH SCORE!")
    setting_name: string = "HighScore" + difficulty
    if blockSettings.exists(setting_name) and info.score() < blockSettings.read_number(setting_name):
        game.set_game_over_message(True, "GAME OVER High Score: " + blockSettings.read_number(setting_name))
        game.game_over(False)
    else:
        blockSettings.write_number(setting_name, info.score())
        game.game_over(True)
        
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
        end_game()
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

info.on_life_zero(end_game)

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
