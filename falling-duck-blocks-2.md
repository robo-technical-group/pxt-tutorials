# Falling Duck Blocks Level 2

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

## Create the functions

First, we will create two functions to initialize the arrays.

1.  In the ``||functions:Functions||`` toolbox,
select the **Make a Function...** button.
1.  Create two functions named **initTopImages** and **initBottomImages**.
1.  In your workspace, place the two functions side by side. This will make it easier to see how the elements in the arrays relate to each other.

![Array functions side by side](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_array_functions.png)

## Create the arrays

Next, we will create the arrays to hold the log images.

1.  In the ``||variables:Variables||`` drawer, select the **Make a Variable...** button.
1.  Create two variables named **TOP_IMAGES** and **BOTTOM_IMAGES**.
1.  From the ``||arrays:Arrays||`` toolbox, drag a   
``||variables(arrays):set list to||``
``||arrays:array of [0] [1] (-) (+)||``   
block onto your workspace near the two functions that you created in the previous step.
1.  From the ``||images:Images||`` drawer, drop an empty image into each slot of the array block.
1.  Duplicate the array block.
1.  Place one array block in each of the functions that you created earlier.
1.  In the ``||functions(noclick):initBottomImages||`` function, set the array's variable name to **BOTTOM_IMAGES**.
1.  In the ``||functions(noclick):initTopImages||`` function, set the array's variable name to **TOP_IMAGES**.

Now, either copy and paste the images from your
``||game(noclick):on game update every 1500 ms||``
block to your new arrays, or (if you are using images from the gallery),
select the blank images in the arrays and choose the images from the gallery.

Be sure that the first image in each array forms a pair, and that the second image in each array forms a pair.

```blocks
function initBottomImages () {
    let BOTTOM_IMAGES = [
        sprites.duck.log2,
        sprites.duck.log3,
    ]
}
function initTopImages () {
    let TOP_IMAGES  =[
        sprites.duck.log7,
        sprites.duck.log6,
    ]
}
```

## Time to use them!

Now, it's time to use our new arrays!

First, we need to call the functions to initialize the arrays.

*   From the ``||functions:Functions||`` drawer,
add calls to the two new functions to the **bottom** of your   
``||loops(noclick):on start||``   
container.

---

Next, we will modify the code that creates the log sprites.

There are a **LOT** of steps here. Follow them carefully!

1.  In your workspace, find your   
``||game(noclick):on game update every 1500 ms||``   
container.
1.  From either branch of the if-else container, drag the two blocks out of the container.
Keeps them off to the side for now. You will reuse them later.
1.  Delete the *if-else* statement that selects the log images.
1.  In the ``||variables:Variables||`` drawer, select the **Make a Variable...** button.
1.  Create a variable named **imageIndex**.
1.  In the   
``||game(noclick):on game update every 1500 ms||``   
container, add a  
``||variables:set imageIndex to [0]||``   
block where the if-else statement was.   
(**Note**: Place it **before** the blocks that set the **topSprite** and **bottomSprite** locations.)
1.  From the ``||math:Math||`` drawer, drag a   
``||math:pick random [0] to [10]||``   
block in place of the *`0`* placeholder.
1. From the ``||math:Math||`` drawer, drop a   
``||math:(0) - (0)||``   
block in place of the *`10`* placeholder.
1.  In the first placeholder of the subtraction block, drop a   
``||arrays:length of array||``
``||variables(arrays):list||``   
block from the ``||arrays:Arrays||`` drawer.
1.  Change the *`list`* placeholder to **`BOTTOM_IMAGES`**.
1.  In the second placeholder of the subtraction block,
change the *`0`* to **`1`**.   
The block should now say   
**`set imageIndex to pick random 0 to length of array BOTTOM_IMAGES - 1`**
1.  Immediately below the block that sets the **imageIndex** variable,
drop the blocks that create the **topSprite** and **bottomSprite**
that you dragged out earlier.
1.  In place of the image in each of those blocks, drop a   
``||variables(arrays):list||``
``||arrays:get value at [0]||``   
block from the ``||arrays:Arrays||`` drawer.
1.  In place of the *`0`* placeholder in each of those blocks,
drop an
``||variables:imageIndex||``   
block from the ``||variables:Variables||`` drawer.
1.  For the **topSprite** block, change the variable name from *list* to **TOP_IMAGES**.
1.  For the **bottomSprite** block, change the variable name from *list* to **BOTTOM_IMAGES**.

Compare your code with the hint to make sure you have everything in the right place!

Wait for the simulator to restart, and test your game. It should work just as before,
but now we can add more log images easily!

```blocks
// @hide
function initTopImages() {
}
// @hide
function initBottomImages() {
}
game.onUpdateInterval(1500, function () {
    let scoreProjectile = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
    scoreProjectile.setFlag(SpriteFlag.Invisible, true)
    let imageIndex = randint(0, BOTTOM_IMAGES.length - 1)
    let topSprite = sprites.createProjectileFromSide(TOP_IMAGES[imageIndex], projectileVx, 0)
    let bottomSprite = sprites.createProjectileFromSide(BOTTOM_IMAGES[imageIndex], projectileVx, 0)
    topSprite.top = 0
    bottomSprite.bottom = scene.screenHeight()
})
let TOP_IMAGES: Image[] = []
let BOTTOM_IMAGES: Image[] = []
initTopImages()
initBottomImages()
```

## Add more images!

Now, let's add more log images to our arrays!

1.  Return to your two array initialization functions.
1.  Select the **expand (+)** button in each array block to give each array four slots.
1.  Add more log images to each array, making sure that the images at the same location in each array form a pair.

Wait for the simulator to restart, and test your game. It should work just as before,
but now you will see more variety in the log pairs!

How easy was that! No other changes to the code!

```blocks
function initTopImages () {
    let TOP_IMAGES  = [
        sprites.duck.log8,
        sprites.duck.log7,
        sprites.duck.log6,
        sprites.duck.log5,
    ]
}
function initBottomImages () {
    let BOTTOM_IMAGES = [
        sprites.duck.log1,
        sprites.duck.log2,
        sprites.duck.log3,
        sprites.duck.log4,
    ]
}
```

## Animate the duck!

Finally, we will add an animation to the duck sprite when it is flying upward.

First, we need a way to keep track of whether the duck is currently animated.
We'll explain why we need this variable later.

1.  In the ``||variables:Variables||`` drawer, select the **Make a Variable...** button.
1.  Create a variable named **isAnimated**.
1.  From the ``||variables:Variables||`` drawer, drag a   
``||variables:set isAnimated to [0]||``   
block to the **bottom** of your   
``||loops(noclick):on start||``   
container.
1.  From the ``||logic:Logic||`` drawer, drop a   
``||logic:true||``   
block in place of the *`0`* placeholder.

---

Now, let's determine if the duck is flying upward.

1.  In your workspace, find your   
``||game(noclick):on game update||``   
container.
1.  From the ``||logic:Logic||`` drawer, drag an   
``||logic:if [true] then else||``   
block into the   
``||game(noclick):on game update||``   
container. Anywhere is fine; feel free to put it at the bottom.
1.  Build the following conditional statement:   
``||logic(noclick):if||``
``||variables(sprites):mySprite||``
``||sprites:vy (velocity y)||``
``||logic:< 0 then||``

---

Now, let's animate the duck when it is flying upward and not already animated.

1.  In the upper branch of the ``||logic(noclick):if||`` block,
drop another   
``||logic:if [true] then||``   
block.
1.  From the ``||logic:Logic||`` drawer, drop a   
``||logic:not [true]||``   
block in place of the *`true`* placeholder.
1.  From the ``||variables:Variables||`` drawer, drop a   
``||variables:isAnimated||``   
block in place of the *`true`* placeholder.
1.  Inside the   
``||logic(noclick):if not||``
``||variables(noclick):isAnimated||``
``||logic(noclick):then||``   
container, drop an   
``||animation:animate||``
``||variables(animation):mySprite||``
``||animation:frames [ ] interval (ms) (500) loop (OFF)||``   
block from the ``||animation:Animations||`` drawer.
1.  After the animation block, drop a   
``||variables:set isAnimated to [0]||``   
block from the ``||variables:Variables||`` drawer.
1.  From the ``||logic:Logic||`` drawer, drop a   
``||logic:true||``   
block in place of the *`0`* placeholder.
1.  Returning to the animation block,
set the *interval* to **`100`** and *loop* to **`ON`**.
1.  Now, select the empty animation to open the animation editor.
1.  For now, switch to the **Gallery** tab and select the flying duck animation.
1.  Select the **Done** button to close the animation editor.

Wait for the simulator to restart, and test your game.

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
container, drop a   
``||animation:stop (all) animation on||``
``||variables(animation):mySprite||``   
block from the ``||animation:Animations||`` drawer.
1.  Then, add a   
``||sprites:set||``
``||variables(sprites):mySprite||``
``||sprites:image to [ ]||``   
block.
1.  Set the image to the original duck image that you used when creating the duck sprite.
1.  Finally, add a   
``||variables:set isAnimated to [0]||``   
block from the ``||variables:Variables||`` drawer.
1.  From the ``||logic:Logic||`` drawer, drop a   
``||logic:false||``   
block in place of the *`0`* placeholder.

Wait for the simulator to restart, and test your game.
When the duck is no longer flying upward, the animation should stop.

```blocks
game.onUpdate(function () {
    if (mySprite.top < 0 || mySprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
    // @highlight
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
let mySprite: Sprite = null
let isAnimated: boolean = false
```

## Congratulations! @showdialog

You did it! You built *Falling Duck* Level 2!

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
let imageIndex = 0
let bottomSprite: Sprite = null
let topSprite: Sprite = null
let scoreProjectile: Sprite = null
let projectileVx = 0
let mySprite: Sprite = null
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
info.setScore(0)
info.setLife(3)
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
mySprite.ay = 100
projectileVx = -45
game.onUpdate(function () {
    if (mySprite.top < 0 || mySprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
})
game.onUpdateInterval(1500, function () {
    scoreProjectile = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
    scoreProjectile.setFlag(SpriteFlag.Invisible, true)
    if (Math.percentChance(50)) {
        topSprite = sprites.createProjectileFromSide(sprites.duck.log7, projectileVx, 0)
        bottomSprite = sprites.createProjectileFromSide(sprites.duck.log2, projectileVx, 0)
    } else {
        topSprite = sprites.createProjectileFromSide(sprites.duck.log6, projectileVx, 0)
        bottomSprite = sprites.createProjectileFromSide(sprites.duck.log3, projectileVx, 0)
    }
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
initBotomImages()
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
    scoreProjectile = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
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