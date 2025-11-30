# Falling Duck Blocks Level 1

## Falling Duck! @showdialog

Enjoy building the Level 1 version of *Falling Duck*!

![Sample of gameplay](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_demo.gif)

## Let's meet our hero!

Our hero character is a duck that flies through the sky and dodges logs. Let's create our hero!

1.  From the ``||sprites:Sprites||`` drawer, drag a   
``||variables(sprite):set mySprite to||``
``||sprites:sprite [ ] of kind Player||``   
block into the   
``||loops(noclick):on start||``   
container already on your workspace.
1.  Select the blank image for your sprite to open the image editor.
1.  Select **Gallery** at the top of the image editor to open the image gallery.
1.  Select one of the duck images for your hero.
1.  In the image editor, select **Done** to set your sprite's image.

~hint What is a sprite?
A sprite is an image that represents a character or object in your game.
Sprites can move around the screen and interact with other sprites.
Sprites have properties like its image, position, velocity, and kind.
hint~

Watch the simulator restart.

Your hero appears in the middle of the screen!

~hint Why is the simulator so small?
If you need to see your project in more detail,
you can make the simulator larger by selecting the
"Launch in fullscreen" (also called the "zoom") button.

Select the zoom button again to shrink the simulator
back into the corner of the screen.

Nearby buttons mute the sound and
stop/hide the simulator.
hint~

Select the hint (light bulb) below if you need help.

```blocks
let mySprite: Sprite = null
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
```

## Let's set the scene

Now, let's give our hero a scene for our hero to fly.

1.  From the ``||scene:Scene||`` drawer, drag a   
``||scene:set background color to [ ]||``   
block into your   
``||loops(noclick):on start||``   
container.
1.  Select the empty color to choose a color for the background.   
We suggest the light blue color to represent the sky.
1.  From the ``||scene:Scene||`` drawer, drag a   
``||scene:start screen confetti effect||``   
block into your   
``||loops(noclick):on start||``   
container.
1.  Change the effect from *confetti* to something atmospheric.   
We suggest **blizzard**.

Watch the simulator restart.

Your hero is now flying through the scene that you built!

Select the hint (light bulb) below if you need help.

```blocks
let mySprite: Sprite = null
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
// @highlight
scene.setBackgroundColor(9)
// @highlight
effects.blizzard.startScreenEffect()
```

## Score and lives

Now, let's set the initial score and lives for our game.

1.  From the ``||info:Info||`` drawer, drag a   
``||info:set score to [0]||``
block into your   
``||loops(noclick):on start||``   
container.
1.  Also from the ``||info:Info||`` drawer, drag a   
``||info:set life to [3]||``
block into your   
``||loops(noclick):on start||``   
container.

Watch the simulator restart.

The screen now shows the player's score and lives at the top of the screen.

Select the hint (light bulb) below if you need help.

```blocks
let mySprite: Sprite = null
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
// @highlight
info.setScore(0)
// @highlight
info.setLife(3)
```

## It's a bird! It's a plane! It's Falling Duck!

Now, let's make our hero fall and fly!

1.  From the ``||sprites:Sprites||`` drawer, add a   
``||sprites:set||``
``||variables(sprites):mySprite||``
``||sprites:x to [0]||``   
block to the **bottom** of your   
``||loops(noclick):on start||``   
container.
1.  In the block that you just added, change the property from *x* to **ay**.
1.  Change the value from *0* to **100**.

Watch the simulator restart.

Your hero now falls with gravity!

Now, let's make our hero fly when we press any button.

1.  From the ``||controller:Controller||`` drawer, drag a   
``||controller:on [A] button pressed||``   
container onto your workspace.
1.  Change the button from *A* to **any**.
1.  Drag a   
``||sprites:set||``
``||variables(sprites):mySprite||``
``||sprites:x to [0]||``   
block into this new container.
1.  In the block that you just added, change the property from *x* to
**vy (velocity y)**.
1.  Change the value from *0* to **-35**.

Watch the simulator restart.

Now, when you press any button, your hero flies up!

Select the hint (light bulb) below if you need help.

```blocks
// @highlight
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    mySprite.vy = -35
})
let mySprite: Sprite = null
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
info.setScore(0)
info.setLife(3)
// @highlight
mySprite.ay = 100
```

## Don't leave the screen!

Now, let's end the game if our hero flies off the screen.

1.  From the ``||game:Game||`` drawer, drag an   
``||game:on game update||``   
container onto your workspace.
1.  From the ``||logic:Logic||`` drawer, drag an   
``||logic:if [true] then||``   
block into this new container.
1.  First, you'll need an ``||logic:or||`` block.   
From the ``||logic:Logic||`` drawer, drag an   
``||logic:[ ] or [ ]||``   
block in place of the **true** placeholder.
1.  In one of the placeholders of the **or** block,
drop a   
``||logic:[0] < [0]||``   
block.
1.  Replace the first *zero* with a   
``||variables(sprites):mySprite||``
``||sprites:x||``   
block.
1.  Change the property from *x* to **top**.

This checks if our hero has flown off the top of the screen.

1.  In the other placeholder of the **or** block,
drop another   
``||logic:[0] < [0]||``   
block.
1.  Change the operator to **>** (greater than).
1.  Replace the first *zero* with a   
``||variables(sprites):mySprite||``
``||sprites:x||``   
block.
1.  Change the property from *x* to **bottom**.
1.  Replace the second *zero* with a   
``||scene:screen height||``   
block.

This checks if our hero has fallen through the bottom of the screen.

If either of these conditions is true, we want to end the game.

1.  From the ``||game:Game||`` drawer, drag a   
``||game:game over [WIN]||``   
block into the **then** section of the **if** block.
1.  Change *WIN* to **LOSE**.

Watch the simulator restart.

Now, if our hero flies off the screen, the game ends!

Select the hint (light bulb) below if you need help.

```blocks
game.onUpdate(function () {
    //@ hide
    let mySprite: Sprite = null
    if (mySprite.top < 0 || mySprite.bottom > scene.screenHeight()) {
        game.gameOver(false)
    }
})
```


## Keeping score

Now, let's have a way for our hero to earn points!

Remember that logs will fly into the screen from the right side of the screen.
If our hero dodges the logs, our hero earns points.

We are going to set the speed of multiple projectiles, so let's create a variable.

1.  From the ``||variables:Variables||`` drawer, select **Make a Variable**.
1.  Name the variable **projectileVx**.
1.  From the ``||variables:Variables||`` drawer, drag a   
``||variables:set [ ] to [0]||``   
block into your   
``||loops(noclick):on start||``   
container.
1.  Make sure the variable is set to **projectileVx**.
1.  Change the value from *0* to **-45**.

Now, let's create the scoring projectile that the player will touch to earn points.

1.  From the ``||game:Game||`` drawer, drag a   
``||game:on game update every [500] ms||``   
container onto your workspace.
1.  Change the time from *500* to **1500** milliseconds.   
**Note**: Since *1500* is not one of the options in the dropdown menu, you will need to type it in manually.
1.  Into this new container, from the ``||sprites:Sprites||`` drawer, add a   
``||variables(sprites):set projectile to||``
``||sprites:projectile [ ] from side with vx [50] vy [50]||``   
block.
1.  We are going to have multiple projectiles, so change the variable name from
*projectile* to **scoreProjectile**.
1.  Select the blank image for your projectile to open the image editor.
1.  In the image editor, draw a tall thin rectangle to represent the scoring projectile. Make the image **2** pixels wide and **120** pixels tall so that it fills the screen. Be sure to fill the rectangle with a color (any color besides transparent). Select **Done** to set your projectile's image.
1.  In place of the *50* for the **vx** (velocity x) value, drop a   
``||variables:projectileVx||``   
block from the ``||variables:Variables||`` drawer.
1.  Change the **vy** (velocity y) value from *50* to **0**.
1.  From the ``||sprites:Sprites||`` drawer, add a   
``||sprites:set||``
``||variables(sprites):mySprite||``
``||sprites:kind to [Player]||``
1.  Change the variable from *mySprite* to **scoreProjectile**.
1.  Change the kind from *Player* to **AddScore**.
1.  From the ``||sprites:Sprites||`` drawer, add a   
``||sprites:set||``
``||variables(sprites):mySprite||``
``||sprites:x to [0]||``   
block to the **bottom** of your new container.
1.  Change the variable from *mySprite* to **scoreProjectile**.
1.  Change the property from *x* to **top**.

~hint How do I change the variable name?
Select the variable name in the block.
A menu appears where you can select **Rename variable...**.
hint~

~hint What is a millisecond?
A millisecond is one thousandth of a second.
So, 1500 milliseconds is one and a half seconds.
hint~

~hint Where will the projectile appear?
Because the projectile's x velocity is negative, it will fly to the left.
Because the projectile's y velocity is zero, it will not move up or down.
The Arcade engine automatically creates the projectile just off the right side of the screen so that it flies across the screen.
Feel free to try other velocity values to see how they affect the projectile's movement and starting position.
hint~

~hint How do I change the size of an image in the image editor?
In the bottom left corner of the image editor, change the values for the width and height of the image.
hint~

Wait for the simulator to restart.

Now, scoring projectiles fly in from the right side of the screen every 1 1/2 seconds!

Finally, let's make it so that when our hero touches a scoring projectile, the player earns a point.

1.  From the ``||sprites:Sprites||`` drawer, drag a   
``||sprites:on sprite of kind [Player] overlaps sprite of kind [Player]||``   
container onto your workspace.
1.  Change the second kind from *Player* to **AddScore**.
1.  From the ``||sprites:Sprites||`` drawer, add a   
``||sprites:destroy||``
``||variables(sprites):mySprite||``   
to this new container.
1.  From the header (top) of this event handler, drag an   
``||variables(noclick):otherSprite||``   
block into the   
``||sprites(noclick):destroy||``
block to destroy the scoring projectile.
1.  From the ``||info:Info||`` drawer, add a   
``||info:change score by [1]||``   
block into this new container.

Watch the simulator restart.

Now, when our hero touches a scoring projectile, the player earns a point!

Select the hint (light bulb) below if you need help.

```blocks
sprites.onOverlap(SpriteKind.Player, SpriteKind.AddScore, function (sprite, otherSprite) {
    sprites.destroy(otherSprite)
    info.changeScoreBy(1)
})
game.onUpdateInterval(1500, function () {
    scoreProjectile = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
})
let scoreProjectile: Sprite = null
let projectileVx: number = 0
projectileVx = -45
```

## Avoid the logs!

Now, let's add the logs that our hero must avoid. In this version of the game, we will create a simplified set of logs.

1.  Using the **Make a Variable** option from the ``||variables:Variables||`` drawer, create two new variables named **topSprite** and **bottomSprite**.
1.  From the ``||logic:Logic||`` drawer, add a   
``||logic:if [true] then [ ] else [ ] ||``   
block to the **bottom** of the   
``||game:on game update every [1500] ms||``   
container already on your workspace.
1.  In place of the *true* placeholder, drop a   
``||math:[0] % chance||``   
block from the ``||math:Math||`` drawer.
1.  Change the *0* to **50** to create a 50% chance for each branch of your
``||logic(noclick):if||`` statement.
1.  In the first branch of your ``||logic(noclick):if||`` block,
add **two**   
``||variables(sprites):set [projectile] to||``
``||sprites:projectile [ ] from side with vx [50] vy [50]||``   
blocks from the ``||sprites:Sprites||`` drawer.
1.  Change the variable from *projectile* to **topSprite** in the first block and to **bottomSprite** in the second block.
1.  In the **topSprite** block, select one of the log images from the Gallery. We suggest using the three-segment log from the second set of log images. (The image is named **log7**.)
1.  In the **bottomSprite** block, select another log image from the Gallery. We suggest using the two-segment log from the first set of log images. (The image is named **log2**.)
1.  In place of the *50* for the **vx** (velocity x) value in both blocks, drop a   
``||variables:projectileVx||``   
block from the ``||variables:Variables||`` drawer.
1.  Change the **vy** (velocity y) value from *50* to **0** in both blocks.
1.  Duplicate these two blocks and drop them into the bottom branch of your   
``||logic(noclick):if||`` block. Change the images to different log images.

These blocks will randomly select one of the two sets of logs to create. Now, we just need to place the sprites in the correct location.

1.  From the ``||sprites:Sprites||`` drawer, add **two**   
``||sprites:set||``
``||variables(sprites):mySprite||``
``||sprites:x to [0]||``   
blocks to the **bottom** of your      
``||game:on game update every [1500] ms||``   
container, **beneath** the ``||logic(noclick):if||`` block.
1.  Change these two blocks so that the **top** of the **topSprite** is set to **0** and the **bottom** of the **bottomSprite** is set to the screen height. The ``||scene:screen height||`` block can be found in the ``||scene:Scene||`` drawer.

Watch the simulator restart.

Now, logs fly in from the right side of the screen every 1 1/2 seconds!

We don't need to see the scoring projectile, so let's make it invisible.

1.  From the ``||sprites:Sprites||`` drawer, add a   
``||sprites:set||``
``||variables(sprites):mySprite||``
``||sprites:[auto destroy] [OFF]||``   
block to the **bottom** of your   
``||game:on game update every [1500] ms||``   
container.
1.  Change the variable from *mySprite* to **scoreProjectile**.
1.  Change the property from *auto destroy* to **invisible**.
1.  Change the value from *OFF* to **ON**.

Watch the simulator restart.

Now, the scoring projectile is invisible!

Select the hint (light bulb) below if you need help.

```blocks
game.onUpdateInterval(1500, function () {
    scoreProjectile = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
    if (Math.percentChance(50)) {
        topSprite = sprites.createProjectileFromSide(sprites.duck.log7, projectileVx, 0)
        bottomSprite = sprites.createProjectileFromSide(sprites.duck.log2, projectileVx, 0)
    } else {
        topSprite = sprites.createProjectileFromSide(sprites.duck.log6, projectileVx, 0)
        bottomSprite = sprites.createProjectileFromSide(sprites.duck.log3, projectileVx, 0)
    }
    topSprite.top = 0
    bottomSprite.bottom = scene.screenHeight()
    scoreProjectile.setFlag(SpriteFlag.Invisible, true)
})
let scoreProjectile: Sprite = null
let topSprite: Sprite = null
let bottomSprite: Sprite = null
let projectileVx: number = 0
```

## Losing lives

Finally, let's make it so that if our hero hits a log, the player loses a life.

1.  From the ``||sprites:Sprites||`` drawer, drop a   
``||sprites:on sprite of kind [Player] overlaps sprite of kind [Player]||``   
container onto your workspace.
1.  Change the second kind from *Player* to **Projectile**.
1.  From the ``||sprites:Sprites||`` drawer, add a   
``||sprites:destroy||``
``||variables(sprites):mySprite (+)||``   
to this new container.
1.  From the header (top) of this event handler, drag an   
``||variables(noclick):otherSprite||``   
block into the   
``||sprites(noclick):destroy||``   
block to destroy the log.
1.  Select the expand **(+)** button to open more options on the   
``||sprites(noclick):destroy||`` block.
1.  Select an effect for the destruction animation. We suggest **disintegrate**.
1.  From the ``||info:Info||`` drawer, add a   
``||info:change life by [-1]||``   
block into this new container.

Watch the simulator restart.

Now, if our hero hits a log, the player loses a life!

Select the hint (light bulb) below if you need help.

```blocks
sprites.onOverlap(SpriteKind.Player, SpriteKind.Projectile, function (sprite, otherSprite) {
    sprites.destroy(otherSprite, effects.disintegrate, 500)
    info.changeLifeBy(-1)
})
```

## Congratulations! @showdialog

You did it! You built *Falling Duck* Level 1!

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

```customts
namespace SpriteKind {
    //% isKind
    export const AddScore = SpriteKind.create()
}
```

```ghost
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