# Falling Duck Python Level 2
### @explicitHints true

## Falling Duck! @showdialog

Enjoy building the Level 2 version of *Falling Duck*!

*   In level 1, we had two sets of log images selected with an
``||logic(noclick):if-else||`` statement.
*   In level 2, we will restructure the code to make it easier to add new log images.   
We also will add an animation to the duck sprite when it is flying upward.

Have fun!

![Sample of gameplay](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_demo.gif)

## Lists are parallel?

In level 1, we used an ``||logic(noclick):if-else||`` statement to choose between two sets of log images.

Let's add more variety to the game by adding more log images.

To do this, we will use two lists to hold the top and bottom log images. Each list will hold the same number of images, and the images at the same index in each list will form a pair.

Because these lists are used together, they are called *parallel lists*.
In other languages, they may be called *parallel arrays* or *parallel vectors*.

When we create a new pair of log sprites, we will generate one random index to select an image from each list.

Let's get started!

## Create the lists

First, we will create the lists to hold the log images. As these lists will not change, we can define them as constants.

1.  In the **Constants** section, create two new constants named `BOTTOM_IMAGES` and `TOP_IMAGES`.
1.  Set their type to `List[Image]`.
1.  Set the lists to empty lists for the time being. Give yourself room to add images.

Your initial code should look like this:

```python
# Constants
BOTTOM_IMAGES: List[Image] = [
]
TOP_IMAGES: List[Image] = [
]
```

Now, copy and paste the images (or the stock gallery images) from your
``||game(noclick):on game update every 1500 ms||``
function to your new lists.

Be sure that the first image in each list forms a pair, and that the second image in each list forms a pair.

```python
BOTTOM_IMAGES: List[Image] = [
    sprites.duck.log2,
    sprites.duck.log3,
]
TOP_IMAGES: List[Image] = [
    sprites.duck.log7,
    sprites.duck.log6,
]
```

## Time to use them!

Now, it's time to use our new lists!

First, we will modify the code that creates the log sprites.

There are a **LOT** of steps here. Follow them carefully!

1.  Find your   
``||game(noclick):on game update every 1500 ms||``   
function.
1.  From either branch of the *if-else* container, move the assignment for **topSprite** and **bottomSprite** out of the *if-else* statement and into the lines that define the variables, replacing the **None** values.
1.  Delete the *if-else* statement that selects the log images.
1.  Above the lines that create **top_sprite** and **bottom_sprite**, create a variable named **image_index**. Set its type to **`number`** and its value to **`randint(0, len(BOTTOM_IMAGES) - 1)`**.
1.  In place of the images for **top_sprite** and **bottom_sprite**, set their images to  
**`TOP_IMAGES[image_index]`** and  
**`BOTTOM_IMAGES[image_index]`**, respectively.

Compare your code with the hint to make sure you have everything in the right place!

Restart the simulator, and test your game. It should work just as before,
but now we can add more log images easily!

#### ~ tutorialhint

```python
def on_update_interval():
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
game.on_update_interval(1500, on_update_interval)
```

## Add more images!

Now, let's add more log images to our lists!

1.  Return to your two image lists.
1.  Add more log images to each list, making sure that the images at the same location in each list form a pair.

Restart the simulator, and test your game. It should work just as before,
but now you will see more variety in the log pairs!

How easy was that! No other changes to the code!

#### ~ tutorialhint

```pythong
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
```

## Animate the duck!

Finally, we will add an animation to the duck sprite when it is flying upward.

First, we need a way to keep track of whether the duck is currently animated.
We'll explain why we need this variable later.

1.  In the global variables section, create a variable named **is_animated**.
1.  Set its type to `bool` and its initial value to `False`.

```python
# Global variables
is_animated: bool = False
```

---

Now, let's determine if the duck is flying upward.

1.  Find your   
``||game(noclick):on game update||``   
function.
1.  Add the ``||logic:if then else||`` statement below to the function.
Anywhere is fine; feel free to put it at the bottom.

```python
if mySprite.vy < 0:
    
else:
    
```

---

Now, let's animate the duck when it is flying upward and not already animated.

*   In the upper branch of the ``||logic(noclick):if||`` statement,
add another ``||logic:if then||`` statement, shown below. Be sure to indent it properly.

```typescript
if not is_animated:  

```

---

Now, let's fill in the new ``||logic(noclick):if||`` statement to run the animation.

1.  Inside the   
``||logic(noclick):if not||``
``||variables(noclick):is_animated||``
``||logic(noclick):then||``
branch, drop an   
``||animation:animate||``
``||variables(animation):mySprite||``
``||animation:frames [frames] interval (ms) (500) loop (loop)||``
snippet from the ``||animation:Animations||`` drawer.
1.  Change the *`None`* placeholder sprite to **`hero_sprite`** (or whatever you named your duck sprite).
1.  Change the *interval* to **`500`** and *loop* to **`True`**.
1.  Add another statement that sets the **`is_animated`** variable to **`True`**.

We'll need to create the animation asset before adding it to the code.

1.  Open the **Assets Editor**.
1.  In the *Assets Editor*, switch to the **Gallery** tab.
1.  Find the **`duckRight`** animation and **Duplicate** it.
1.  Return to the **Code Editor** by selecting the **Python** tab.

Now, in place of the empty *frames* array, add the animation asset you just created using the code below.

```python
animation.run_image_animation(
    hero_sprite,
    assets.animation("""
    duckRight
    """),
    100, True
)
```

Restart the simulator, and test your game.

When you press a button to make the duck fly upward,
you should see the flying duck animation!

~hint Why do we need the is_animated variable?

Without the ``||variables(noclick):is_animated||`` variable,
the animation would restart every time the player pressed a button.

hint~

---

Now, we need to stop the animation when the duck is no longer flying upward.

1.  In the bottom branch of the   
``||logic(noclick):if||``
``||variables(noclick):my_sprite||``
``||sprites(noclick):vy (velocity y)||``
``||logic(noclick):< 0 then||``   
statement, drop a   
``||animation:stop (all) animation on||``
``||variables(animation):mySprite||``   
snippet from the ``||animation:Animations||`` drawer.
1.  Then, add a   
``||sprites:set||``
``||variables(sprites):mySprite||``
``||sprites:image to [img]||``   
snippet.
1.  Set the sprite variables in both statements to **`hero_sprite`** (or whatever you named your duck sprite).
1.  Set the image to the original duck image that you used when creating the duck sprite.
1.  Finally, add another statement that sets the **`is_animated`** variable to **`False`**.
1.  Because we are changing the value of a global variable, we need to tell Python about it. At the start of the   
``||game(noclick):on game update||``   
function, add line below.

```python
global is_animated
```

Restart the simulator, and test your game.
When the duck is no longer flying upward, the animation should stop.

### ~ tutorialhint

```python
def on_update():
    global is_animated
    if hero_sprite.top < 0 or hero_sprite.bottom > scene.screen_height():
        game.game_over(False)
    if hero_sprite.vy < 0:
        if not is_animated:
            animation.run_image_animation(
                hero_sprite,
                assets.animation("""
                duckRight
                """),
                100, True
            )
            is_animated = True
    else:
        animation.stop_animation(animation.AnimationTypes.ALL, hero_sprite)
        hero_sprite.set_image(sprites.duck.duck3)
        is_animated = False
game.on_update(on_update)
```

## Congratulations! @showdialog

You did it! You built *Falling Duck* Level 2!

Continue to the next level to add more features to your game,
or give these enhancements a try on your own:

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

# Global variables
hero_sprite: Sprite = None
projectile_vx: number = -45

# Functions

# Event handlers
def on_a_pressed():
    hero_sprite.vy = -35
controller.any_button.on_event(ControllerButtonEvent.PRESSED, on_a_pressed)

def on_update():
    if hero_sprite.top < 0 or hero_sprite.bottom > scene.screen_height():
        game.game_over(False)
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
    top_sprite: Sprite = None
    bottom_sprite: Sprite = None
    if Math.percent_chance(50):
        top_sprite = sprites.create_projectile_from_side(sprites.duck.log6, projectile_vx, 0)
        bottom_sprite = sprites.create_projectile_from_side(sprites.duck.log3, projectile_vx, 0)
    else:
        top_sprite = sprites.create_projectile_from_side(sprites.duck.log7, projectile_vx, 0)
        bottom_sprite = sprites.create_projectile_from_side(sprites.duck.log2, projectile_vx, 0)
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
                assets.animation("""
                duckRight
                """),
                100, True
            )
            is_animated = True
    else:
        animation.stop_animation(animation.AnimationTypes.ALL, hero_sprite)
        hero_sprite.set_image(sprites.duck.duck3)
        is_animated = False
game.on_update(on_update)

def on_update_interval():
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