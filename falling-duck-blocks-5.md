# Falling Duck Blocks Level 5

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
block to your new function.
1.  Set the splash message to something like   
`"Game over!"`
1.  In your   
``||functions(noclick):updateGame||`` function,
replace the existing   
``||game(noclick):game over||``   
block with a call to your new function.

---

We also need to call this function when we run out of lives.

1.  From the ``||info:Info||`` drawer,
drag an   
``||info:on life zero||``   
container onto your workspace.
1.  In this new container, add a call to your   
``||functions(noclick):endGame||``   
function.

Wait for the simulator to restart.

When you lose the game, instead of the standard "Game Over" dialog,
you should see your splash message instead.

Review the code in the hint if you need any help.

```blocks
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
let mySprite: Sprite = null
```

## Customizing the end game

Now, let's customize how the **Game Over** process runs.

Add the following blocks to your ``||functions(noclick):endGame||`` function.

You can find all of them in the ``||game:Game||`` drawer.

*   Set the scoring type to **`None`**. We will be controlling how the high score is saved ourselves.
*   Set game over **effect**s for both the winning and the losing conditions.
*   Set game over **sound**s for both the winning and the losing conditions.
*   Set the game over **message** to `"NEW HIGH SCORE!"` for a **WIN**.

Check the hint if you need any help.

```blocks
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
1.  Using the ``||variables:Variables||`` drawer,
create a new variable called **`settingName`**.
1.  At the **bottom** of your   
``||functions(noclick):endGame||``   
function, set the value of   
``||variables:settingName||``   
to the string   
**`"HighScore"`**   
``||text:join||``   
with the value of the   
``||variables:difficulty||``   
variable.
1.  At the **bottom** of your   
``||functions(noclick):endGame||``   
function, add an   
``||logic:if (true) then else||``   
container.
1.  Use an   
``||logic:and||``   
block with blocks from the   
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

---

The conditional tests if the setting exists and, if it does,
whether the current score is lower than the high score
for this difficulty.

If this is the case, then the player has not beaten the high score
for this difficulty.

When this happens, let's give the player a custom message and then
end the game as a loss.

Add the following blocks to the upper (**`true`**) branch of the   
``||logic(noclick):if||``   
container:

*   Add a block from the   
``||game:Game||``   
drawer that sets the game over **message** to the string   
**`"GAME OVER High Score:"`**   
``||text:join||``
``||blockSettings:read setting||``
``||variables:settingName||``
``||blockSettings:as number||``   
for a **LOSE**.
*  Add a block from the   
``||game:Game||``   
drawer that ends the game as **`LOSE`**.

---

If the conditional is not true, then either the settings does not exist
or the player has beaten the previous high score.

When this happens, let's store the current score as the high score
for this difficulty.

Add the following blocks to the lower (**`false`**) branch of the
``||logic(noclick):if||``   
container:

*   Add a block from the   
``||blockSettings:settings||``   
drawer that stores the current   
``||info:score||``   
as a number using the value of the   
``||variables:settingName||``   
variable as the name of the setting.
*  Add a block from the   
``||game:Game||``   
drawer that ends the game as a **`WIN`**.

Wait for the simulator to restart.

Play the game and try to beat your high scores for each difficulty setting!

Check the hint if you need any help.

```blocks
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
let settingName: string = ""
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
        endGame()
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
info.onLifeZero(function () {
    endGame()
})
```