# Falling Duck JavaScript Level 3
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

## More parallel arrays!

To add multiple levels to the game, we will use two more parallel arrays. One array will hold the frequencies at which projectiles appear for each level. The other array will hold the starting score required to reach each level.

1.  In the **Constants** section, create two new constants named **`LEVEL_PROJECTILE_FREQUENCIES`** and **`LEVEL_STARTING_SCORES`**.
1.  Set their types to **`number[]`**.
1.  Set the values in the *`LEVEL_PROJECTILE_FREQUENCIES`* array to   
**`1500, 1300, 1100, 900, 700`**.
1.  For now, set the values in the *`LEVEL_STARTING_SCORES`* array to some test values, like   
**`0, 3, 6, 9, 12`**.   
We will adjust these values later.

Notice that these two arrays form a pair of parallel arrays. The first value in each array corresponds to level 0, which starts when the player has a score of zero, and projectiles will appear every 1,500 milliseconds. The second value in each array corresponds to level 1, and so on.

Nothing will change in your game yet, but check the hint to make sure your code looks right.

#### ~ tutorialhint

```typescript
/**
 * Constants
 */
const LEVEL_PROJECTILE_FREQUENCIES: number[] = [ 1500, 1300, 1100, 900, 700, ]
const LEVEL_STARTING_SCORES: number[] = [ 0, 3, 6, 9, 12, ]
```

## Create projectiles based on level

To create projectiles based on the current level, we need to do a little prep work.

1.  Create a new function named **`createProjectiles`**. See the box below if you need help with creating a function.
1.  Move the code that creates the projectiles from the   
``||game(noclick):game.onUpdateInterval()||`` event handler function into the new function.
1.  Delete the entire   
``||game(noclick):game.onUpdateInterval||`` event handler.
1.  In the **Global Variables** section, create two new variables named **`level`** and **`nextProjectileTime`**.
1.  Set both new variables to `0` and give them appropriate types.

~hint How do I create a function?

Use the keyword **`function`** to create a named function.

```typescript
function createProjectiles(): void {

}
```

The empty parentheses after the name of the function indicates
that the function does not take parameters.

The curly braces mark the beginning and end of the function's code.
In this case, the function does not contain any code --
It is an empty function.

It's always a good idea to indicate the return type of a function.
In this case, our function will not return a value, so we state
that our function has a **`void`** return.

hint~

The `level` variable keeps track of the current level of the game. The `nextProjectileTime` variable determines when to create the next set of projectiles.

---

Now, let's create projectiles based on the current level.

1.  In the ``||game(noclick):game.onUpdate()||`` event handler, add an   
``||logic:if (true) then||``   
statement. Anywhere inside the event handler function is fine; feel free to add it to the end.
1.  Use code snippets from the ``||game:Game||``, ``||variables:Variables||``, and ``||logic:Logic||`` drawers if needed to create the following condition:   
``||game:time since start (ms)||``
``||logic:>=||``
``||variables:nextProjectileTime||``
1.  Within the ``||logic(noclick):if||`` statement, call your new **`createProjectiles`** function.
See the box below if you need help with calling a function.
1.  Finally, we need to update the *`nextProjectileTime`* variable each time we create projectiles.    
At the **bottom** of your **`createProjectiles`** function, set the **`nextProjectileTime`** variable to:   
``||game:time since start (ms)||``
``||math:+||``
``||variables(arrays):LEVEL_PROJECTILE_FREQUENCIES||``
``||arrays:get value at||``
``||variables(arrays):level||``

~hint How do I call a function?

To call a function in JavaScript, simply enter the name of the function
followed by a set of parentheses. If your function takes parameters,
you would place the parameter values inside of the parentheses.

```typescript
createProjectiles()
```

hint~

Restart the simulator.

Your game should still function the same as before, but now projectiles are created based on the current level.

We will add code to increase the level in the next step.

Check the hint if you need help.

#### ~ tutorialhint

```typescript
/**
 * Global variables
 */
let level: number = 0
let nextProjectileTime: number = 0

/**
 * Functions
 */
function createProjectiles(): void {
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

game.onUpdate(function () {
    /** Existing code is here. **/
    if (game.runtime() >= nextProjectileTile) {
        createProjectiles()
    }
})
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
1.  Be sure to change the *`PlaybackMode`* to **`InBackground`**.
1.  Below the sound effect block, add a statement that increments *`level`* by 1.
1.  Finally, give the player an extra life when they level up. Use the ``||info:Info||`` drawer to   
``||info:change life by (-1)||``   
Be sure to change the default value to **`1`**.

~hint How do I change the sound effect?

By default, when using the code snippet from the toolbox, the
`music.play()` function plays the **`music.baDing`** sound effect.

To see the other options, delete the *`baDing`* constant name, and then delete
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

```typescript
game.onUpdate(function () {
    /** Existing code is here. **/
    if (level < LEVEL_STARTING_SCORES.length - 1) {
        if (info.score() >= LEVEL_STARTING_SCORES[level + 1]) {
            music.play(music.melodyPlayable(music.powerUp), music.PlaybackMode.InBackground)
            level += 1
            info.changeLifeBy(1)
        }
    }
})
```

## Set final starting scores

Now that everything is working, let's set the final starting scores for each level.

1.  In the **Constants** section, change the values in the
*`LEVEL_STARTING_SCORES`* array to   
**`0, 15, 30, 45, 60`**.

Restart the simulator.

Play the game to see the new values in action!

#### ~ tutorialhint

```typescript
/**
 * Constants
 */
const LEVEL_PROJECTILE_FREQUENCIES: number[] = [ 1500, 1300, 1100, 900, 700, ]
// @highlight
const LEVEL_STARTING_SCORES: number[] = [ 0, 15, 30, 45, 60, ]
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
let projectileVx = -45

/**
 * Functions
 */

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
})
game.onUpdateInterval(1500, function () {
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
