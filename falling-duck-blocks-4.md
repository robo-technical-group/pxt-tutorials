# Falling Duck Blocks Level 4

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

1.  Create a function called   
``||functions:initGame||``.
1.  Move all of the blocks from your   
``||loops(noclick):on start||``    
container into to your new function.
1.  Add a call to your function to   
``||loops(noclick):on start||``.
1.  Create another function called   
``||functions:updateGame||``.
1.  Move all of the blocks from your   
``||game(noclick):on game update||``   
container into your new function.
1.  Add a call to your function to   
``||game(noclick):on game update||``.

Test your game to make sure it still works correctly.

If so, then onward!

## Is the game running?

Before we ask the player to select a difficulty, let's create a variable
that indicates whether the game has started. Without this, the game
will try to update before we are ready.

1.  Create a new variable called   
``||variables:isGameRunning||``.
1.  In your   
``||loops(noclick):on start||``
container, set the value of this variable to   
``||logic:false||``.
1.  In your   
``||game(noclick):on game update||``   
container, call your   
``||functions(noclick):updateGame||``   
function **only if** the game is running.
1.  **Remove** the call to   
``||functions(noclick):initGame||``   
from your   
``||loops(noclick):on start||``   
container.

Wait for the simulator to restart.

You should see a blank screen with no errors.

Check the hint if you need any help.

```blocks
function updateGame() {

}
let isGameRunning: boolean = false
isGameRunning = false
game.onUpdate(function () {
    if (isGameRunning) {
        updateGame()
    }
})
```

## Menu, please!

Now, let's give the player a choice of difficulty levels.

To do this, we will use the   
``||miniMenu:Mini Menu||``   
extension.

1.  From the   
``||miniMenu:Mini Menu||``   
toolbox, add a   
``||variables(miniMenu):set [myMenu] to||``
``||miniMenu:create menu sprite menu item ("abc")||``   
block to **the bottom ** of your   
``||loops(noclick):on start||``   
container.
1.  Select the **expand (+)**   
button at the bottom of the new block to add   
**two more** menu items.
1.  Give your difficulty levels names, such as   
*Easy*, *Intermediate*, and *Difficult*.
1.  From the   
``||miniMenu:Mini Menu||``   
toolbox, add a   
``||variables(miniMenu):[myMenu]||``
``||miniMenu:on [A] pressed with||``
``||variables(miniMenu):selection selectedIndex||``   
container to **the bottom ** of your   
``||loops(noclick):on start||``   
container.
1.  Using the   
``||variables:Variables||``   
drawer, create a new variable called   
``||variables:difficulty||``.
1.  In the event handler that runs when selecting a difficulty level,
close the menu.
1.  Then, set your new   
``||variables:difficulty||``   
variable to   
``||variables(noclick):selectedIndex||``   
by dragging a copy from the top of the event handler.
1.  Then, call your   
``||functions:initGame||``   
function.
1.  Finally, set   
``||variables:isGameRunning||``   
to   
``||logic:true||``.

Wait for the simulator to restart.

Try selecting a menu option. **You will generate an error**.

---

Oops! Let's fix that error.

1.  Find your existing   
``||controller(noclick):on [any] button pressed||``   
container.
1.  Only set the hero sprite's velocity if the game is running.

Wait for the simulator to restart.

Now, you should be able to select a difficulty,
and then the game should start.

Check the hint if you need help.

```blocks
function initGame() {

}
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    if (isGameRunning) {
        mySprite.vy = -35
    }
})
let isGameRunning: boolean = false
let mySprite: Sprite = null
isGameRunning = false
let difficultyMenu = miniMenu.createMenu(
miniMenu.createMenuItem("Easy"),
miniMenu.createMenuItem("Intermediate"),
miniMenu.createMenuItem("Difficult")
)
difficultyMenu.onButtonPressed(controller.A, function (selection, selectedIndex) {
    difficultyMenu.close()
    difficulty = selectedIndex
    initGame()
    isGameRunning = true
})
```

## Ooh! Parallel arrays again!

Now, let's create some arrays that will hold our settings for the different difficulties.

1.  Create a new function called   
``||functions:initDifficultySettings||``.
1.  In this new function, create two new arrays called   
``||arrays:PLAYER_ACCELERATIONS||``   
and   
``||arrays:PROJECTILE_SPEEDS||``.
1.  Give each array three slots to correspond to the three difficulty levels.
1.  Set the values of each array to some appropriate values.   
For `PLAYER_ACCELERATIONS`, try *125*, *100*, and *75*.   
For `PROJECTILE_SPEEDS`, try *-30*, *-45*, and *-60*.
1.  Call this new function **at the beginning** of your   
``||functions(noclick):initGame||``   
function.

Check the hint if you need any help.

```blocks
function initDifficultySettings () {
    PLAYER_ACCELERATIONS = [125, 100, 75]
    PROJECTILE_SPEEDS = [-30, -45, -60]
}
function initGame() {
    initDifficultySettings()
}
let PLAYER_ACCELERATIONS: number[] = []
let PROJECTILE_SPEEDS: number[] = []
```

## Use your arrays!

Now that we've setup our arrays, we need to use them!

1.  Find the block in your   
``||functions(noclick):initGame||``   
function that sets the hero sprite's acceleration.
1.  In place of the existing value, drop a   
``||variables(arrays):(list)||``
``||arrays:get value at (0)||``   
block from the   
``||arrays:Arrays||``   
drawer.
1.  Change the array to   
``||variables(noclick):PLAYER_ACCELERATIONS||``.
1.  Set the index to your   
``||variables:difficulty||``   
variable.
1.  Similarly, change the block that sets your   
``||variables(noclick):projectileVx||``   
variable to   
``||variables(arrays):(PROJECTILE_SPEEDS)||``   
``||arrays:get value at||``   
``||variables:difficulty||``.

Wait for the simulator to restart.

Now, you should be able to choose a difficulty level
and see a change in gravity and in the speed of the logs!


```blocks
// @hide
function initDifficultySettings () {
    
}
function initGame () {
    initDifficultySettings()
    level = 0
    scene.setBackgroundColor(9)
    effects.blizzard.startScreenEffect()
    info.setScore(0)
    info.setLife(3)
    mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
    // @highlight
    mySprite.ay = PLAYER_ACCELERATIONS[difficulty]
    // @highlight
    projectileVx = PROJECTILE_SPEEDS[difficulty]
}
let level: number = 0
let mySprite: Sprite = null
let projectileVx: number = 0
```

## Congratulations! @showdialog

You did it! You built *Falling Duck* Level 4!

Continue to the next level to add more features to your game,
or give these enhancements a try on your own:

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

```customts
namespace SpriteKind {
    //% isKind
    export const AddScore = SpriteKind.create()
}
```

```template
function initTopImages () {
    TOP_IMAGES = [
    sprites.duck.log8,
    sprites.duck.log7,
    sprites.duck.log6,
    sprites.duck.log5
    ]
}
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    mySprite.vy = -35
})
function initLevelSettings () {
    LEVEL_PROJECTILE_FREQUENCIES = [
    1500,
    1300,
    1100,
    900,
    700
    ]
    LEVEL_STARTING_SCORES = [
    0,
    15,
    30,
    45,
    60
    ]
}
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.changeLifeBy(-1)
})
function initBottomImages () {
    BOTTOM_IMAGES = [
    sprites.duck.log1,
    sprites.duck.log2,
    sprites.duck.log3,
    sprites.duck.log4
    ]
}
sprites.onOverlap(SpriteKind.Player, SpriteKind.AddScore, function (sprite, otherSprite) {
    sprites.destroy(otherSprite)
    info.changeScoreBy(1)
})
function createProjectiles () {
    scoreProjectile = sprites.createProjectileFromSide(img`
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        f f 
        `, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
    scoreProjectile.setFlag(SpriteFlag.Invisible, true)
    imageIndex = randint(0, BOTTOM_IMAGES.length - 1)
    topSprite = sprites.createProjectileFromSide(TOP_IMAGES[imageIndex], projectileVx, 0)
    bottomSprite = sprites.createProjectileFromSide(BOTTOM_IMAGES[imageIndex], projectileVx, 0)
    topSprite.top = 0
    bottomSprite.bottom = scene.screenHeight()
    nextProjectileTime = game.runtime() + LEVEL_PROJECTILE_FREQUENCIES[level]
}
let isAnimated = false
let nextProjectileTime = 0
let bottomSprite: Sprite = null
let topSprite: Sprite = null
let imageIndex = 0
let scoreProjectile: Sprite = null
let BOTTOM_IMAGES: Image[] = []
let LEVEL_STARTING_SCORES: number[] = []
let LEVEL_PROJECTILE_FREQUENCIES: number[] = []
let TOP_IMAGES: Image[] = []
let projectileVx = 0
let mySprite: Sprite = null
let level = 0
level = 0
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
info.setScore(0)
info.setLife(3)
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
mySprite.ay = 100
projectileVx = -45
initBottomImages()
initTopImages()
initLevelSettings()
game.onUpdate(function () {
    if (mySprite.top < 0 || mySprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    if (mySprite.vy < 0) {
        if (!(isAnimated)) {
            animation.runImageAnimation(
            mySprite,
            [
            sprites.duck.duck3,
            sprites.duck.duck4,
            sprites.duck.duck5,
            sprites.duck.duck6,
            sprites.duck.duck1,
            sprites.duck.duck2
            ],
            100,
            true
            )
            isAnimated = true
        }
    } else {
        animation.stopAnimation(animation.AnimationTypes.All, mySprite)
        mySprite.setImage(sprites.duck.duck3)
        isAnimated = false
    }
    if (game.runtime() >= nextProjectileTime) {
        createProjectiles()
    }
    if (level < LEVEL_STARTING_SCORES.length - 1) {
        if (info.score() >= LEVEL_STARTING_SCORES[level + 1]) {
            music.play(music.melodyPlayable(music.powerUp), music.PlaybackMode.InBackground)
            level += 1
            info.changeLifeBy(1)
        }
    }
})
```

```ghost
function initTopImages () {
    TOP_IMAGES = [
    sprites.duck.log8,
    sprites.duck.log7,
    sprites.duck.log6,
    sprites.duck.log5
    ]
}
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    if (isGameRunning) {
        mySprite.vy = -35
    }
})
function initLevelSettings () {
    LEVEL_PROJECTILE_FREQUENCIES = [
    1500,
    1300,
    1100,
    900,
    700
    ]
    LEVEL_STARTING_SCORES = [
    0,
    15,
    30,
    45,
    60
    ]
}
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.changeLifeBy(-1)
})
function initBottomImages () {
    BOTTOM_IMAGES = [
    sprites.duck.log1,
    sprites.duck.log2,
    sprites.duck.log3,
    sprites.duck.log4
    ]
}
function initGame () {
    level = 0
    scene.setBackgroundColor(9)
    effects.blizzard.startScreenEffect()
    info.setScore(0)
    info.setLife(3)
    mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
    mySprite.ay = 100
    projectileVx = -45
    initBottomImages()
    initTopImages()
    initLevelSettings()
}
sprites.onOverlap(SpriteKind.Player, SpriteKind.AddScore, function (sprite, otherSprite) {
    sprites.destroy(otherSprite)
    info.changeScoreBy(1)
})
function createProjectiles () {
    scoreProjectile = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
    scoreProjectile.setFlag(SpriteFlag.Invisible, true)
    imageIndex = randint(0, BOTTOM_IMAGES.length - 1)
    topSprite = sprites.createProjectileFromSide(TOP_IMAGES[imageIndex], projectileVx, 0)
    bottomSprite = sprites.createProjectileFromSide(BOTTOM_IMAGES[imageIndex], projectileVx, 0)
    topSprite.top = 0
    bottomSprite.bottom = scene.screenHeight()
    nextProjectileTime = game.runtime() + LEVEL_PROJECTILE_FREQUENCIES[level]
}
function updateGame() {
    if (mySprite.top < 0 || mySprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    if (mySprite.vy < 0) {
        if (!(isAnimated)) {
            animation.runImageAnimation(
            mySprite,
            [
            sprites.duck.duck3,
            sprites.duck.duck4,
            sprites.duck.duck5,
            sprites.duck.duck6,
            sprites.duck.duck1,
            sprites.duck.duck2
            ],
            100,
            true
            )
            isAnimated = true
        }
    } else {
        animation.stopAnimation(animation.AnimationTypes.All, mySprite)
        mySprite.setImage(sprites.duck.duck3)
        isAnimated = false
    }
    if (game.runtime() >= nextProjectileTime) {
        createProjectiles()
    }
    if (level < LEVEL_STARTING_SCORES.length - 1) {
        if (info.score() >= LEVEL_STARTING_SCORES[level + 1]) {
            music.play(music.melodyPlayable(music.powerUp), music.PlaybackMode.InBackground)
            level += 1
            info.changeLifeBy(1)
        }
    }
}
function initDifficultySettings () {
    PLAYER_ACCELERATIONS = [125, 100, 75]
    PROJECTILE_SPEEDS = [-30, -45, -60]
}
let isAnimated = false
let nextProjectileTime = 0
let bottomSprite: Sprite = null
let topSprite: Sprite = null
let imageIndex = 0
let scoreProjectile: Sprite = null
let BOTTOM_IMAGES: Image[] = []
let LEVEL_STARTING_SCORES: number[] = []
let LEVEL_PROJECTILE_FREQUENCIES: number[] = []
let TOP_IMAGES: Image[] = []
let projectileVx = 0
let mySprite: Sprite = null
let level = 0
let isGameRunning: boolean = false
let PLAYER_ACCELERATIONS: number[] = []
let PROJECTILE_SPEEDS: number[] = []
isGameRunning = false
let difficultyMenu = miniMenu.createMenu(
miniMenu.createMenuItem("Easy"),
miniMenu.createMenuItem("Intermediate"),
miniMenu.createMenuItem("Difficult")
)
difficultyMenu.onButtonPressed(controller.A, function (selection, selectedIndex) {
    difficultyMenu.close()
    difficulty = selectedIndex
    initGame()
    isGameRunning = true
})
game.onUpdate(function () {
    if (isGameRunning) {
        updateGame()
    }
})
```