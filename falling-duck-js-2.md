# Falling Duck JavaScript Level 2
### @explicitHints true

## Falling Duck! @showdialog

Enjoy building the Level 2 version of *Falling Duck*!

*   In level 1, we had two sets of log images selected with an
``||logic(noclick):if-else||`` statement.
*   In level 2, we will restructure the code to make it easier to add new log images.   
We also will add an animation to the duck sprite when it is flying upward.

Have fun!

![Sample of gameplay](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_demo.gif)

## Arrays are parallel?

In level 1, we used an ``||logic(noclick):if-else||`` statement to choose between two sets of log images.

Let's add more variety to the game by adding more log images.

To do this, we will use two arrays to hold the top and bottom log images. Each array will hold the same number of images, and the images at the same index in each array will form a pair.

Because these arrays are used together, they are called *parallel arrays*.

When we create a new pair of log sprites, we will generate one random index to select an image from each array.

Let's get started!

## Create the arrays

First, we will create the arrays to hold the log images. As these arrays will not change, we can define them as constants.

1.  In the **Constants** section, create two new constants named `BOTTOM_IMAGES` and `TOP_IMAGES`.
1.  Set their type to `Image[]`.
1.  Set the arrays to empty arrays for the time being. Give yourself room to add images.

Your initial code should look like this:

```typescript
/**
 * Constants
 */
const BOTTOM_IMAGES: Image[] = [
]
const TOP_IMAGES: Image[] = [
]
```

Now, copy and paste the images (or the stock gallery images) from your
``||game(noclick):on game update every 1500 ms||``
function to your new arrays.

Be sure that the first image in each array forms a pair, and that the second image in each array forms a pair.

```typescript
const BOTTOM_IMAGES: Image[] = [
    sprites.duck.log2,
    sprites.duck.log3,
]
const TOP_IMAGES: Image[] = [
    sprites.duck.log7,
    sprites.duck.log6,
]
```

## Time to use them!

Now, it's time to use our new arrays!

First, we will modify the code that creates the log sprites.

There are a **LOT** of steps here. Follow them carefully!

1.  Find your   
``||game(noclick):on game update every 1500 ms||``   
function.
1.  From either branch of the *if-else* container, move the assignment for **topSprite** and **bottomSprite** out of the *if-else* statement and into the lines that define the variables, replacing the **null** values.
1.  Delete the *if-else* statement that selects the log images.
1.  Above the lines that create **topSprite** and **bottomSprite**, create a variable named **imageIndex**. Set its type to **`number`** and its value to **`randint(0, BOTTOM_IMAGES.length - 1)`**.
1.  In place of the images for **topSprite** and **bottomSprite**, set their images to  
**`TOP_IMAGES[imageIndex]`** and  
**`BOTTOM_IMAGES[imageIndex]`**, respectively.

Compare your code with the hint to make sure you have everything in the right place!

Restart the simulator, and test your game. It should work just as before,
but now we can add more log images easily!

#### ~ tutorialhint

```typescript
game.onUpdateInterval(1500, function () {
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
})
```

## Add more images!

Now, let's add more log images to our arrays!

1.  Return to your two image arrays.
1.  Add more log images to each array, making sure that the images at the same location in each array form a pair.

Restart the simulator, and test your game. It should work just as before,
but now you will see more variety in the log pairs!

How easy was that! No other changes to the code!

#### ~ tutorialhint

```typescript
const TOP_IMAGES: Image[]  = [
    sprites.duck.log8,
    sprites.duck.log7,
    sprites.duck.log6,
    sprites.duck.log5,
]
const BOTTOM_IMAGES: Image[] = [
    sprites.duck.log1,
    sprites.duck.log2,
    sprites.duck.log3,
    sprites.duck.log4,
]
```

## Animate the duck!

Finally, we will add an animation to the duck sprite when it is flying upward.

First, we need a way to keep track of whether the duck is currently animated.
We'll explain why we need this variable later.

1.  In the global variables section, create a variable named **isAnimated**.
1.  Set its type to `boolean` and its initial value to `false`.

```typescript
/**
 * Global variables
 */
let isAnimated: boolean = false
```

---

Now, let's determine if the duck is flying upward.

1.  Find your   
``||game(noclick):on game update||``   
function.
1.  Add the ``||logic:if then else||`` statement below to the function.
Anywhere is fine; feel free to put it at the bottom.

```typescript
if (mySprite.vy < 0) {
    
} else {
    
}
```

---

Now, let's animate the duck when it is flying upward and not already animated.

*   In the upper branch of the ``||logic(noclick):if||`` statement,
add another ``||logic:if then||`` statement, shown below.

```typescript
if (!isAnimated) {
    
}
```

---

Now, let's fill in the new ``||logic(noclick):if||`` statement to run the animation.

1.  Inside the   
``||logic(noclick):if not||``
``||variables(noclick):isAnimated||``
``||logic(noclick):then||``
branch, drop an   
``||animation:animate||``
``||variables(animation):mySprite||``
``||animation:frames [frames] interval (ms) (500) loop (loop)||``
snippet from the ``||animation:Animations||`` drawer.
1.  Change the *`null`* placeholder sprite to **`heroSprite`** (or whatever you named your duck sprite).
1.  Change the empty *frames* array to the flying duck animation, which is called **`sprites.duck.duckRight`** in the stock gallery.
1.  Change the *interval* to **`500`** and *loop* to **`true`**.
1.  Add another statement that sets the **`isAnimated`** variable to **`true`**.

Restart the simulator, and test your game.

When you press a button to make the duck fly upward,
you should see the flying duck animation!

~hint Why do we need the isAnimated variable?

Without the ``||variables(noclick):isAnimated||`` variable,
the animation would restart every time the player pressed a button.

hint~

---

Now, we need to stop the animation when the duck is no longer flying upward.

1.  In the bottom branch of the   
``||logic(noclick):if||``
``||variables(noclick):mySprite||``
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
1.  Set the sprite variables in both statements to **`heroSprite`** (or whatever you named your duck sprite).
1.  Set the image to the original duck image that you used when creating the duck sprite.
1.  Finally, add another statement that sets the **`isAnimated`** variable to **`false`**.

Restart the simulator, and test your game.
When the duck is no longer flying upward, the animation should stop.

### ~ tutorialhint

```typescript
game.onUpdate(function () {
    if (heroSprite.top < 0 || heroSprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    if (!isAnimated){
        animation.runImageAnimation(
            heroSprite,
            sprites.duck.duckRight,
            100, true
        )
        isAnimated = true
    } else {
        animation.stopAnimation(animation.AnimationTypes.All, heroSprite)
        heroSprite.setImage(sprites.duck.duck3)
        isAnimated = false
    }
})
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

```template
namespace SpriteKind {
    export const AddScore = SpriteKind.create()
}

/**
 * Constants
 */

/**
 * Global variables
 */
let heroSprite: Sprite = null
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
    let topSprite: Sprite = null
    let bottomSprite: Sprite = null
    if (Math.percentChance(50)) {
        topSprite = sprites.createProjectileFromSide(sprites.duck.log6, projectileVx, 0)
        bottomSprite = sprites.createProjectileFromSide(sprites.duck.log3, projectileVx, 0)
    } else {
        topSprite = sprites.createProjectileFromSide(sprites.duck.log7, projectileVx, 0)
        bottomSprite = sprites.createProjectileFromSide(sprites.duck.log2, projectileVx, 0)
    }
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
    if (!isAnimated){
        animation.runImageAnimation(
            heroSprite,
            sprites.duck.duckRight,
            100, true
        )
        isAnimated = true
    } else {
        animation.stopAnimation(animation.AnimationTypes.All, heroSprite)
        heroSprite.setImage(sprites.duck.duck3)
        isAnimated = false
    }
})
game.onUpdateInterval(1500, function () {
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
