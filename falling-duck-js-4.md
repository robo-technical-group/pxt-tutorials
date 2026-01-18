# Falling Duck JavaScript Level 4
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
``||functions:initGame||``.
1.  Move all of the blocks from the **Main** section of your code into to your new function.
1.  In **Main**, call your new function.
It should be the only line of code in **Main** right now.
1.  Create another function called   
``||functions:updateGame||``.
1.  Move all of the blocks from your   
``||game(noclick):on game update||``   
event handler into your new function.
1.  Add a call to your function to   
``||game(noclick):on game update||``.

Test your game to make sure it still works correctly.

If so, then onward! If not, then use the hint for help.

#### ~ tutorialhint

```typescript
/**
 * Functions
 */
function initGame() {
    scene.setBackgroundColor(9)
    effects.blizzard.startScreenEffect()
    info.setScore(0)
    info.setLife(3)
    heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
    heroSprite.ay = 100
}

function updateGame() {
    if (heroSprite.top < 0 || heroSprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    if (heroSprite.vy < 0) {
        if (!isAnimated) {
            animation.runImageAnimation(
                heroSprite,
                [
                    sprites.duck.duck3,
                    sprites.duck.duck4,
                    sprites.duck.duck5,
                    sprites.duck.duck6,
                    sprites.duck.duck1,
                    sprites.duck.duck2,
                ],
                100, true
            )
            isAnimated = true
        }
    } else {
        animation.stopAnimation(animation.AnimationTypes.All, heroSprite)
        heroSprite.setImage(sprites.duck.duck3)
        isAnimated = false
    }
    if (game.runtime() >= nextProjectileTime) {
        createProjectiles()
    }
    if (level < LEVEL_PROJECTILE_FREQUENCIES.length - 1) {
        if (info.score() >= LEVEL_STARTING_SCORES[level + 1]) {
            music.play(music.melodyPlayable(music.powerUp), music.PlaybackMode.InBackground)
            level += 1
            info.changeLifeBy(1)
        }
    }
}

/**
 * Event handlers
 */
game.onUpdate(function () {
    updateGame()
})

/**
 * Main
 */
initGame()
```

## Is the game running?

Before we ask the player to select a difficulty, let's create a variable
that indicates whether the game has started. Without this, the game
will try to update before we are ready.

1.  In the **Global variables** section, create a new **Boolean** variable called   
``||variables:isGameRunning||``.   
Set its initial value to   
``||logic:false||``.
1.  In your   
``||game(noclick):on game update||``   
event handler function, call your   
``||functions(noclick):updateGame||``   
function **only if** the game is running.
1.  **Remove** the call to   
``||functions(noclick):initGame||``   
from **Main**.

Restart the simulator.

You should see a blank screen with no errors.

Check the hint if you need any help.

#### ~ tutorialhint

```typescript
/**
 * Global variables
 */
let isGameRunning: boolean = false

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
Be sure to separate the arguments to **`createMenu`** with commas.
1.  Give your difficulty levels names, such as   
*Easy*, *Intermediate*, and *Difficult*.
1.  From the   
``||miniMenu:Mini Menu||``   
drawer, add a   
``||miniMenu:run code this on button pressed with selection selectedIndex||``   
snippet to **the bottom** of **Main**.
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
``||functions:initGame||``   
function.
1.  Finally, set   
``||variables:isGameRunning||``   
to   
``||logic:true||``.

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

```typescript
/**
 * Global variables
 */
let difficulty: number = 0

/**
 * Event handlers
 */
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    if (isGameRunning) {
        heroSprite.vy = -35
    }
})
game.onUpdate(function () {
    if (isGameRunning) {
        updateGame()
    }
})

/**
 * Main
 */
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Easy"),
    miniMenu.createMenuItem("Intermediate"),
    miniMenu.createMenuItem("Difficult")
)
myMenu.onButtonPressed(controller.A, function(selection: string, selectedIndex: number) {
    myMenu.close()
    difficulty = selectedIndex
    initGame()
    isGameRunning = true
})
```

## Ooh! Parallel arrays again!

Now, let's create some arrays that will hold our settings for the different difficulties.

1.  In the **Constants** section, create two new arrays called   
``||arrays:PLAYER_ACCELERATIONS||``   
and   
``||arrays:PROJECTILE_SPEEDS||``.
1.  Set their types to **`number[]`**.
1.  Set the values of each array to some appropriate values.   
For `PLAYER_ACCELERATIONS`, try *125*, *100*, and *75*.   
For `PROJECTILE_SPEEDS`, try *-30*, *-45*, and *-60*.

Check the hint if you need any help.

#### ~ tutorialhint

```typescript
/**
 * Constants
 */
const PLAYER_ACCELERATIONS: number[] = [ 125, 100, 75, ]
const PROJECTILE_SPEEDS: number[] = [ -30, -45, -60, ]
```

## Use your arrays!

Now that we've setup our arrays, we need to use them!

1.  Find the line of code in your   
``||functions(noclick):initGame||``   
function that sets the hero sprite's acceleration.
1.  In place of the existing value, set the acceleration to   
**`PLAYER_ACCELERATIONS[difficulty]`**.
1.  Beneath that line, add a line that sets   
``||variables:projectileVx||``   
to   
**`PROJECTILE_SPEEDS[difficulty]`**.

Restart the simulator.

Now, you should be able to choose a difficulty level
and see a change in gravity and in the speed of the logs!

#### ~ tutorialhint

```typescript
function initGame () {
    scene.setBackgroundColor(9)
    effects.blizzard.startScreenEffect()
    info.setScore(0)
    info.setLife(3)
    heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
    // @highlight
    mySprite.ay = PLAYER_ACCELERATIONS[difficulty]
    // @highlight
    projectileVx = PROJECTILE_SPEEDS[difficulty]
}
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

```template
namespace SpriteKind {
    export const AddScore = SpriteKind.create()
}

/**
 * Constants
 */
const BOTTOM_IMAGES: Image[] = [
    sprites.duck.log3,
    sprites.duck.log2,
    sprites.duck.log4,
    sprites.duck.log1,
]
const LEVEL_PROJECTILE_FREQUENCIES: number[] = [1500, 1300, 1100, 900, 700,]
const LEVEL_STARTING_SCORES: number[] = [0, 15, 30, 45, 60,]
const TOP_IMAGES: Image[] = [
    sprites.duck.log6,
    sprites.duck.log7,
    sprites.duck.log5,
    sprites.duck.log8,
]

/**
 * Global variables
 */
let heroSprite: Sprite = null
let isAnimated: boolean = false
let level: number = 0
let nextProjectileTime: number = 0
let projectileVx = -45

/**
 * Functions
 */
function createProjectiles() {
    let scoreProjectile: Sprite = sprites.createProjectileFromSide(img`
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
    let imageIndex: number = randint(0, BOTTOM_IMAGES.length - 1)
    let topSprite: Sprite =
        sprites.createProjectileFromSide(TOP_IMAGES[imageIndex], projectileVx, 0)
    let bottomSprite: Sprite =
        sprites.createProjectileFromSide(BOTTOM_IMAGES[imageIndex], projectileVx, 0)
    topSprite.top = 0
    bottomSprite.bottom = scene.screenHeight()
    nextProjectileTime = game.runtime() + LEVEL_PROJECTILE_FREQUENCIES[level]
}

/**
 * Event handlers
 */
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    heroSprite.vy = -35
})
game.onUpdate(function () {
    if (heroSprite.top < 0 || heroSprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    if (heroSprite.vy < 0) {
        if (!isAnimated) {
            animation.runImageAnimation(
                heroSprite,
                [
                    sprites.duck.duck3,
                    sprites.duck.duck4,
                    sprites.duck.duck5,
                    sprites.duck.duck6,
                    sprites.duck.duck1,
                    sprites.duck.duck2,
                ],
                100, true
            )
            isAnimated = true
        }
    } else {
        animation.stopAnimation(animation.AnimationTypes.All, heroSprite)
        heroSprite.setImage(sprites.duck.duck3)
        isAnimated = false
    }
    if (game.runtime() >= nextProjectileTime) {
        createProjectiles()
    }
    if (level < LEVEL_PROJECTILE_FREQUENCIES.length - 1) {
        if (info.score() >= LEVEL_STARTING_SCORES[level + 1]) {
            music.play(music.melodyPlayable(music.powerUp), music.PlaybackMode.InBackground)
            level += 1
            info.changeLifeBy(1)
        }
    }
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.changeLifeBy(-1)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.AddScore, function (sprite, otherSprite) {
    sprites.destroy(otherSprite)
    info.changeScoreBy(1)
})

/**
 * Main
 */
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
info.setScore(0)
info.setLife(3)
heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
heroSprite.ay = 100
```

```ghost
namespace SpriteKind {
    export const AddScore = SpriteKind.create()
}

/**
 * Constants
 */
const BOTTOM_IMAGES: Image[] = [
    sprites.duck.log3,
    sprites.duck.log2,
    sprites.duck.log4,
    sprites.duck.log1,
]
const LEVEL_PROJECTILE_FREQUENCIES: number[] = [ 1500, 1300, 1100, 900, 700, ]
const LEVEL_STARTING_SCORES: number[] = [ 0, 15, 30, 45, 60, ]
const PLAYER_ACCELERATIONS: number[] = [ 125, 100, 75, ]
const PROJECTILE_SPEEDS: number[] = [ -30, -45, -60, ]
const TOP_IMAGES: Image[] = [
    sprites.duck.log6,
    sprites.duck.log7,
    sprites.duck.log5,
    sprites.duck.log8,
]

/**
 * Global variables
 */
let difficulty: number = 0
let heroSprite: Sprite = null
let isAnimated: boolean = false
let isGameRunning: boolean = false
let level: number = 0
let nextProjectileTime: number = 0
let projectileVx = -45

/**
 * Functions
 */
function createProjectiles() {
    let scoreProjectile: Sprite = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
    scoreProjectile.setFlag(SpriteFlag.Invisible, true)
    let imageIndex: number = randint(0, BOTTOM_IMAGES.length - 1)
    let topSprite: Sprite =
        sprites.createProjectileFromSide(TOP_IMAGES[imageIndex], projectileVx, 0)
    let bottomSprite: Sprite =
        sprites.createProjectileFromSide(BOTTOM_IMAGES[imageIndex], projectileVx, 0)
    topSprite.top = 0
    bottomSprite.bottom = scene.screenHeight()
    nextProjectileTime = game.runtime() + LEVEL_PROJECTILE_FREQUENCIES[level]
}

function initGame() {
    scene.setBackgroundColor(9)
    effects.blizzard.startScreenEffect()
    info.setScore(0)
    info.setLife(3)
    heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
    heroSprite.ay = PLAYER_ACCELERATIONS[difficulty]
    projectileVx = PROJECTILE_SPEEDS[difficulty]
}

function updateGame() {
    if (heroSprite.top < 0 || heroSprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    if (heroSprite.vy < 0) {
        if (!isAnimated) {
            animation.runImageAnimation(
                heroSprite,
                [
                    sprites.duck.duck3,
                    sprites.duck.duck4,
                    sprites.duck.duck5,
                    sprites.duck.duck6,
                    sprites.duck.duck1,
                    sprites.duck.duck2,
                ],
                100, true
            )
            isAnimated = true
        }
    } else {
        animation.stopAnimation(animation.AnimationTypes.All, heroSprite)
        heroSprite.setImage(sprites.duck.duck3)
        isAnimated = false
    }
    if (game.runtime() >= nextProjectileTime) {
        createProjectiles()
    }
    if (level < LEVEL_PROJECTILE_FREQUENCIES.length - 1) {
        if (info.score() >= LEVEL_STARTING_SCORES[level + 1]) {
            music.play(music.melodyPlayable(music.powerUp), music.PlaybackMode.InBackground)
            level += 1
            info.changeLifeBy(1)
        }
    }
}

/**
 * Event handlers
 */
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    if (isGameRunning) {
        heroSprite.vy = -35
    }
})
game.onUpdate(function () {
    if (isGameRunning) {
        updateGame()
    }
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.changeLifeBy(-1)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.AddScore, function (sprite, otherSprite) {
    sprites.destroy(otherSprite)
    info.changeScoreBy(1)
})

/**
 * Main
 */
let myMenu = miniMenu.createMenu(
    miniMenu.createMenuItem("Easy"),
    miniMenu.createMenuItem("Intermediate"),
    miniMenu.createMenuItem("Difficult")
)
myMenu.onButtonPressed(controller.A, function(selection: string, selectedIndex: number) {
    myMenu.close()
    difficulty = selectedIndex
    initGame()
    isGameRunning = true
})
```