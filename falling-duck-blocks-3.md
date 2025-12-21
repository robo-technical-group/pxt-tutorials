# Falling Duck Blocks Level 3

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

## More parallel arrays!

To add multiple levels to the game, we will use two more parallel arrays. One array will hold the frequencies at which projectiles appear for each level. The other array will hold the starting score required to reach each level.

1.  Create two new variables named **`LEVEL_PROJECTILE_FREQUENCIES`** and **`LEVEL_STARTING_SCORES`**.
1.  Create a new function named **`initLevelSettings`** to initialize these arrays.
1.  Call your new function at the **bottom** of your   
``||loops:on start||`` container.
1.  In the **`initLevelSettings`** function, drop a   
``||variables(arrays): set [list] to ||``
``||arrays:array of (0) (1)||``   
block from the ``||arrays:Arrays||`` drawer.
1.  Change the variable name to **`LEVEL_PROJECTILE_FREQUENCIES`**.
1.  Duplicate the block to create the second array for **`LEVEL_STARTING_SCORES`**.
1.  Set the values in the *`LEVEL_PROJECTILE_FREQUENCIES`* array to   
**`1500, 1300, 1100, 900, 700`**.
1.  For now, set the values in the *`LEVEL_STARTING_SCORES`* array to some test values, like   
**`0, 3, 6, 9, 12`**.   
We will adjust these values later.

Notice that these two arrays form a pair of parallel arrays. The first value in each array corresponds to level 0, which starts when the player has a score of zero, and projectiles will appear every 1,500 milliseconds. The second value in each array corresponds to level 1, and so on.

Nothing will change in your game yet, but check the hint to make sure your code looks right.

```blocks
function initLevelSettings () {
    let LEVEL_PROJECTILE_FREQUENCIES = [1500, 1300, 1100, 900, 700]
    let LEVEL_STARTING_SCORES = [0, 3, 6, 9, 12]
}
initLevelSettings()
```

## Create projectiles based on level

To create projectiles based on the current level, we need to do a little prep work.

1.  Create a new function named **`createProjectiles`**.
1.  Move the code that creates the projectiles from the   
``||game(noclick):on game update every (1500) ms||`` container into the new function.
1.  Delete the empty   
``||game(noclick):on game update every (1500) ms||`` container.
1.  Create two new variables named **`level`** and **`nextProjectileTime`**.
1.  At the **bottom** of your   
``||loops(noclick):on start||``   
container, set both new variables to `0`.

The `level` variable keeps track of the current level of the game. The `nextProjectileTime` variable determines when to create the next set of projectiles.

---

Now, let's create projectiles based on the current level.

1.  In the ``||game(noclick):on game update||`` container, add an   
``||logic:if (true) then||``   
block. Anywhere inside the container is fine; feel free to add it to the bottom.
1.  Use blocks from the ``||game:Game||``, ``||variables:Variables||``, and ``||logic:Logic||`` drawers to create the following condition:   
``||logic(noclick):if||``
``||game:time since start (ms)||``
``||logic:>=||``
``||variables:nextProjectileTime||``
``||logic(noclick):then||``
1.  Within the ``||logic(noclick):if||`` block, call your new **`createProjectiles`** function.
1.  Finally, we need to update the *`nextProjectileTime`* variable each time we create projectiles.    
At the **bottom** of your **`createProjectiles`** function, set the **`nextProjectileTime`** variable to:   
``||game:time since start (ms)||``
``||math:+||``
``||variables(arrays):LEVEL_PROJECTILE_FREQUENCIES||``
``||arrays:get value at||``
``||variables(arrays):level||``

Watch the simulator restart.

Your game should still function the same as before, but now projectiles are created based on the current level.

We will add code to increase the level in the next step.

Check the hint if you need help.

```blocks
function createProjectiles () {
    let scoreProjectile = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
    scoreProjectile.setFlag(SpriteFlag.Invisible, true)
    let imageIndex = randint(0, BOTTOM_IMAGES.length - 1)
    let topSprite = sprites.createProjectileFromSide(TOP_IMAGES[imageIndex], projectileVx, 0)
    let bottomSprite = sprites.createProjectileFromSide(BOTTOM_IMAGES[imageIndex], projectileVx, 0)
    topSprite.top = 0
    bottomSprite.bottom = scene.screenHeight()
    // @highlight
    nextProjectileTile = game.runtime() + LEVEL_PROJECTILE_FREQUENCIES[level]
}
game.onUpdate(function () {
    if (game.runtime() >= nextProjectileTile) {
        createProjectiles()
    }
})
let nextProjectileTile = 0
```

## Level up!

Now, let's add code to increase the level as the player scores points.

1.  In the ``||game(noclick):on game update||`` container, add another   
``||logic:if (true) then||``   
block. Anywhere inside the container is fine; feel free to add it to the bottom.
1.  Use blocks from the ``||logic:Logic||``, ``||math:Math||``, ``||variables:Variables||``, and ``||arrays:Arrays||`` drawers to create the following condition:   
``||logic(noclick):if||``
``||variables:level||``
``||logic:<||``
``||arrays:length of array||``
``||variables(arrays):LEVEL_STARTING_SCORES||``
``||math:- (1)||``
1.  Inside of this ``||logic(noclick):if||`` block, add another   
``||logic:if (true) then||``   
block.
1.  Use blocks from the ``||info:Info||``, ``||arrays:Arrays||``, ``||math:Math||``, and ``||variables:Variables||`` drawers to create the following condition:   
``||logic(noclick):if||``
``||info:score||``
``||logic:>=||``
``||variables(arrays):LEVEL_STARTING_SCORES||``
``||arrays:get value at||``
``||math:+ (1)||``
1.  Inside of this new ``||logic(noclick):if||`` container, add a block that plays a sound effect. You can find one in the ``||music:Music||`` drawer.
1.  Below the sound effect block, add a block from the ``||variables:Variables||`` drawer to   
``||variables:change (level) by (1)||``
1.  Finally, give the player an extra life when they level up. Use a block from the ``||info:Info||`` drawer to   
``||info:change life by (1)||``   
Be sure to change the default value to **`1`**.

Review the code that we just built.

1.  First, we check whether we have new levels to reach.
1.  If so, we check whether the player's score is high enough to reach the next level.
1.  If the player has enough points, we play a sound effect, increase the level by 1, and give the player an extra life.

Wait for the simulator to restart.

Play the game and try to score points. As your score increases, you should hear a sound effect and gain an extra life each time you reach a new level. The logs should also start appearing more quickly.

Check the hint if you need help.

```blocks
game.onUpdate(function () {
    if (level < LEVEL_STARTING_SCORES.length - 1) {
        if (info.score() >= LEVEL_STARTING_SCORES[level + 1]) {
            music.play(music.melodyPlayable(music.powerUp), music.PlaybackMode.InBackground)
            level += 1
            info.changeLifeBy(1)
        }
    }
})
let level = 0
let LEVEL_STARTING_SCORES: number[] = []
```

## Set final starting scores

Now that everything is working, let's set the final starting scores for each level.

1.  In your **`initLevelSettings`** function, change the values in the
*`LEVEL_STARTING_SCORES`* array to   
**`0, 15, 30, 45, 60`**.

Wait for the simulator to restart.

Play the game to see the new values in action!

```blocks
function initLevelSettings () {
    let LEVEL_PROJECTILE_FREQUENCIES = [1500, 1300, 1100, 900, 700]
    // @highlight
    let LEVEL_STARTING_SCORES = [0, 15, 30, 45, 60]
}
```

## Congratulations! @showdialog

You did it! You built *Falling Duck* Level 3!

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
    TOP_IMAGES  =[
        sprites.duck.log8,
        sprites.duck.log7,
        sprites.duck.log6,
        sprites.duck.log5,
    ]
}
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    mySprite.vy = -35
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.changeLifeBy(-1)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.AddScore, function (sprite, otherSprite) {
    sprites.destroy(otherSprite)
    info.changeScoreBy(1)
})
function initBottomImages () {
    BOTTOM_IMAGES = [
        sprites.duck.log1,
        sprites.duck.log2,
        sprites.duck.log3,
        sprites.duck.log4,
    ]
}
let imageIndex = 0
let bottomSprite: Sprite = null
let topSprite: Sprite = null
let scoreProjectile: Sprite = null
let projectileVx = 0
let BOTTOM_IMAGES: Image[] = []
let TOP_IMAGES: Image[] = []
let mySprite: Sprite = null
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
info.setScore(0)
info.setLife(3)
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
mySprite.ay = 100
projectileVx = -45
initBottomImages()
initTopImages()
let isAnimated: boolean = false
game.onUpdate(function () {
    if (mySprite.top < 0 || mySprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    if (mySprite.vy < 0) {
        if (!(isAnimated)) {
            animation.runImageAnimation(
            mySprite,
            sprites.duck.duckRight,
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
})
game.onUpdateInterval(1500, function () {
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
})
```

```ghost
function initTopImages () {
    TOP_IMAGES  =[
        sprites.duck.log8,
        sprites.duck.log7,
        sprites.duck.log6,
        sprites.duck.log5,
    ]
}
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    mySprite.vy = -35
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.changeLifeBy(-1)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.AddScore, function (sprite, otherSprite) {
    sprites.destroy(otherSprite)
    info.changeScoreBy(1)
})
function initBottomImages () {
    BOTTOM_IMAGES = [
        sprites.duck.log1,
        sprites.duck.log2,
        sprites.duck.log3,
        sprites.duck.log4,
    ]
}
function initLevelSettings () {
    LEVEL_PROJECTILE_FREQUENCIES = [1500, 1300, 1100, 900, 700]
    LEVEL_STARTING_SCORES = [0, 15, 30, 45, 60]
}
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
let imageIndex = 0
let bottomSprite: Sprite = null
let topSprite: Sprite = null
let scoreProjectile: Sprite = null
let projectileVx = 0
let BOTTOM_IMAGES: Image[] = []
let TOP_IMAGES: Image[] = []
let LEVEL_STARTING_SCORES: number[] = []
let LEVEL_PROJECTILE_FREQUENCIES: number[] = []
let mySprite: Sprite = null
let nextProjectileTime: number = 0
let level: number = 0
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
info.setScore(0)
info.setLife(3)
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
mySprite.ay = 100
projectileVx = -45
initBottomImages()
initTopImages()
let isAnimated: boolean = false
initLevelSettings()
game.onUpdate(function () {
    if (mySprite.top < 0 || mySprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    if (mySprite.vy < 0) {
        if (!(isAnimated)) {
            animation.runImageAnimation(
            mySprite,
            sprites.duck.duckRight,
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