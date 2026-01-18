# Falling Duck JavaScript Level 5
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
``||functions:endGame||``.
1.  For now, just to make sure our code is working as we expect, add a   
``||game:splash||``   
statement to your new function.
1.  Set the splash message to something like   
`"Game over!"`
1.  In your   
``||functions(noclick):updateGame||`` function,
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
``||functions(noclick):endGame||``   
function.

Restart the simulator.

When you lose the game, instead of the standard "Game Over" dialog,
you should see your splash message instead.

Review the code in the hint if you need any help.

#### ~ tutorialhint

```typescript
function endGame () {
    game.splash("Game over!")
}
function updateGame() {
    if (mySprite.top < 0 || mySprite.bottom > scene.screenHeight()) {
        // @highlight
        endGame()
    }
}
info.onLifeZero(function () {
    endGame()
})
```

## Customizing the end game

Now, let's customize how the **Game Over** process runs.

Add the following code to your ``||functions(noclick):endGame||`` function.

You can find all of the code snippets in the ``||game:Game||`` drawer.

*   Set the scoring type to   
**`game.ScoringType.None`**.   
We will be controlling how the high score is saved ourselves.
*   Set game over **effect**s for both the winning and the losing conditions.
*   Set game over **sound**s for both the winning and the losing conditions.
*   Set the game over **message** to `"NEW HIGH SCORE!"` for a **WIN**.

Check the hint if you need any help.

#### ~ tutorialhint

```typescript
function endGame () {
    game.splash("Game over!")
    game.setGameOverScoringType(game.ScoringType.None)
    game.setGameOverEffect(true, effects.confetti)
    game.setGameOverEffect(false, effects.melt)
    game.setGameOverPlayable(true, music.melodyPlayable(music.powerUp), false)
    game.setGameOverPlayable(false, music.melodyPlayable(music.wawawawaa), false)
    game.setGameOverMessage(true, "NEW HIGH SCORE!")
}
```

## High score?

Now, let's save and check the high scores for each difficulty.

Check the hint if you need any help.

1.  First, **delete** the existing ``||game(noclick):splash||`` block,
since we are implementing the real logic now.
1.  At the **bottom** of your   
``||functions(noclick):endGame||``   
function, create a new (local, not global) variable called **`settingName`**.
1.  Set the type to **`string`** and the value of   
to the string   
**`"HighScore" + difficulty`**.
1.  At the **bottom** of your   
``||functions(noclick):endGame||``   
function, add an   
``||logic:if (true) then else||``   
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
``||logic(noclick):then||``   
**Remember** that the **and** operator in Typescript is written as   
**`&&`**
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

```typescript
function endGame () {
    game.setGameOverScoringType(game.ScoringType.None)
    game.setGameOverEffect(true, effects.confetti)
    game.setGameOverEffect(false, effects.melt)
    game.setGameOverPlayable(true, music.melodyPlayable(music.powerUp), false)
    game.setGameOverPlayable(false, music.melodyPlayable(music.wawawawaa), false)
    game.setGameOverMessage(true, "NEW HIGH SCORE!")
    settingName = "HighScore" + difficulty
    if (blockSettings.exists(settingName) && info.score() < blockSettings.readNumber(settingName)) {
        game.setGameOverMessage(false, "GAME OVER High Score: " + blockSettings.readNumber(settingName))
        game.gameOver(false)
    } else {
        blockSettings.writeNumber(settingName, info.score())
        game.gameOver(true)
    }
}
```

## Congratulations! @showdialog

You did it! You have built all five levels of *Falling Duck*!

Give these enhancements a try on your own:

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

function endGame() {
    game.setGameOverScoringType(game.ScoringType.None)
    game.setGameOverEffect(true, effects.confetti)
    game.setGameOverEffect(false, effects.melt)
    game.setGameOverPlayable(true, music.melodyPlayable(music.powerUp), false)
    game.setGameOverPlayable(false, music.melodyPlayable(music.wawawawaa), false)
    game.setGameOverMessage(true, "NEW HIGH SCORE!")
    let settingName: string = "HighScore" + difficulty
    if (blockSettings.exists(settingName) && info.score() < blockSettings.readNumber(settingName)) {
        game.setGameOverMessage(false, "GAME OVER High Score: " + blockSettings.readNumber(settingName))
        game.gameOver(false)
    } else {
        blockSettings.writeNumber(settingName, info.score())
        game.gameOver(true)
    }
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
        endGame()
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
info.onLifeZero(endGame)
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
