# Falling Duck JavaScript Level 1
### @explicitHints true

## Falling Duck! @showdialog

Enjoy building the Level 1 version of *Falling Duck*!

![Sample of gameplay](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_demo.gif)

## Let's prepare our code file

First, let's split our code file into multiple sections for better organization.

Using block comments, create the following sections in your code file:

*   Constants
*   Global variables
*   Functions
*   Event handlers
*   Main

A block comment looks something like this:

```typescript
/**
 * This is a block comment.
 */
```

Notice that the autocomplete feature helps you create block comments quickly!

---

Also, for this tutorial, we need to use a new SpriteKind for scoring. At the top of your code file, add the following code to create a new SpriteKind called `AddScore`:

```typescript
namespace SpriteKind {
    export const AddScore = SpriteKind.create()
}
```

Use the hint below (light bulb) to see how your code file should look after completing these steps.

~hint Why use sections in my code file? Does the order matter?

Using sections in your code file helps organize your code into logical parts, making it easier to read and maintain.

The order of these sections has been chosen to reduce the chance of unexpected errors in your code. For example, global variables are declared before they are used in functions or event handlers.

hint~

#### ~ tutorialhint
```typescript
namespace SpriteKind {
    export const AddScore = SpriteKind.create()
}

/**
 * Constants
 */

/**
 * Global variables
 */

/**
 * Functions
 */

/**
 * Event handlers
 */

/**
 * Main
 */
```

## Let's meet our hero!

Our hero character is a duck that flies through the sky and dodges logs. Let's create our hero!

*   In the **Global variables** section, create a variable called **`heroSprite`** of type **`Sprite`**. Set the initial value to **`null`**.

```typescript
/**
 * Global variables
 */
let heroSprite: Sprite = null
```

Now, let's create the sprite for our hero.

1.  In the **Main** section, use the toolbox to drag a code snippet that creates a sprite. Remove the **let** keyword, as you have already declared the variable.
1.  Notice that the artist's palette appears next to the line number. Select it to open the image editor.
1.  You also can replace the ``img`...` `` portion with a built-in duck image.  Feel free to use the **`sprites.duck.duck3`** image.

```typescript
heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
```

~hint What is a sprite?

A *sprite* is an image that represents a character or object in your game.

Sprites can move around the screen and interact with other sprites.

Sprites have properties like its image, position, velocity, and kind.

hint~

Restart the simulator.

Your hero appears in the middle of the screen!

~hint Why is the simulator so small?

If you need to see your project in more detail,
you can make the simulator larger by selecting the
**Launch in fullscreen** (also called the *zoom*) button.

Select the zoom button again to shrink the simulator
back into the corner of the screen.

Nearby buttons mute the sound and
stop/hide the simulator.

hint~

~hint Why is the simulator gray? Why doesn't the simulator automatically restart like in Blocks?

When using a typing language in MakeCode, the simulator does not automatically restart
after every change. This is to help you avoid losing your place while typing.

To restart the simulator, select the "Restart" button (circular arrow) in the simulator screen.

hint~

As always, if you need help, use the hint below to see how your code should look after completing these steps.

#### ~ tutorialhint
```typescript
/**
 * Global variables
 */
let heroSprite: Sprite = null

/**
 * Main
 */
heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
```

## Let's set the scene

Now, let's give our hero a scene for our hero to fly.

1.  In the **Main** section, use a code snippet from the ``||scene:Scene||`` drawer to set the background color.
1.  Select a color for the background. A light blue color works well for a sky.
1.  Next, use a code snippet from the ``||scene:Scene||`` drawer to start a screen effect.
1.  Select an appropriate effect, such as **`blizzard`**.

Restart the simulator.

Your hero is now flying through the scene that you built!

### MakeCode Arcade Palette

The following table shows the colors available in the
standard MakeCode Arcade color palette.

```text
+----+--------------+
|  # |    Color     |
+----+--------------+
|  0 | Transparent  |
|  1 | White        |
|  2 | Red          |
|  3 | Pink         |
|  4 | Orange       |
|  5 | Yellow       |
|  6 | Teal         |
|  7 | Green        |
|  8 | Blue         |
|  9 | Light Blue   |
| 10 | Purple       |
| 11 | Light Purple |
| 12 | Dark Purple  |
| 13 | Tan          |
| 14 | Brown        |
| 15 | Black        |
+----+--------------+
```

#### ~ tutorialhint

```typescript
/**
 * Main
 */
heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
// @highlight
scene.setBackgroundColor(9)
// @highlight
effects.blizzard.startScreenEffect()
```

## Score and lives

Now, let's set the initial score and lives for our game.

1.  In the **Main** section, use a code snippet from the ``||info:Info||`` drawer to set the score to **`0`**.
1.  Next, use another code snippet from the ``||info:Info||`` drawer to set the lives to **`3`**.

Restart the simulator.

The screen now shows the player's score and lives at the top of the screen.

#### ~ tutorialhint

```typescript
/**
 * Main
 */
heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
// @highlight
info.setScore(0)
// @highlight
info.setLife(3)
```

## It's a bird! It's a plane! It's Falling Duck!

Now, let's make our hero fall and fly!

*   In the **Main** section, set the vertical acceleration of your hero sprite to **`100`**. The property is called **`ay`**.

~hint How do I do this?

1.  On an empty line in your code window, start typing the name of your variable, `heroSprite`.

1.  As you type, the autocomplete feature will show a list of variables to help you.

1.  Finish typing the variable name, or press **TAB** on your keyboard to have autocomplete finish the typing for you.

1.  After entering the variable name, press the **dot/period key** "." and
autocomplete will show a list of properties for that variable. Start typing, or select from the menu.   
(Use the arrow keys or your mouse to select an item.)   
Press **TAB** again to have autocomplete finish the typing for you.

Want to get the autocomplete menu out of your way? Press **ESC** on your keyboard to dismiss the autocomplete menu.

Still need help? As always, you can use the help (light bulb) at the bottom of the page to see the code that you're creating.

hint~

Restart the simulator.

Your hero now falls with gravity!

---

Now, let's make our hero fly when we press any button.

1.  In the **Event handlers** section, use a code snippet from the ``||controller:Controller||`` drawer to handle the event when **any button** is **pressed**.
1.  Inside the event handler, set the vertical velocity of your hero sprite to **`-35`**. The property is called **`vy`**.

Restart the simulator.

Now, when you press any button, your hero flies up!

~hint How does an event handler work in JavaScript/TypeScript?

An event handler is a function that runs in response to a specific event, such as a button press or a collision between sprites.

When the event occurs, the code inside the event handler function is executed.

In the version that is provided in the code snippet, the event handler uses an anonymous function, which is a function without a name. This is sometimes called an "inline function."

This is different from how an event handler works in Python, where a named function must be used.

hint~

#### ~ tutorialhint

```typescript
/**
 * Event handlers
 */
// @highlight
controller.anyButton.onEvent(ControllerButtonEvent.Pressed, function () {
    // @highlight
    heroSprite.vy = -35
// @highlight
})

/**
 * Main
 */
heroSprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
scene.setBackgroundColor(9)
effects.blizzard.startScreenEffect()
info.setScore(0)
info.setLife(3)
// @highlight
heroSprite.ay = 100
```

## Don't leave the screen!

Now, let's end the game if our hero flies off the screen.

*   In the **Event handlers** section, drag a new code snippet from the ``||game:Game||`` drawer to handle the **on game update** event.

Within the function that handles this event, you will build an **if** statement that checks if the hero sprite has flown off the top or bottom of the screen.

First, create an **if** statement that will hold multiple conditions. This is also called a *compound conditional* statement.

```typescript
game.onUpdate(function() {
    if (

    ) {
        
    }
})
```

For one of the conditions in your compound conditional, check if the top of the hero sprite is less than zero (the top of the screen).

For the second condition, check if the bottom of the hero sprite is greater than the screen height (the bottom of the screen). Use a code snippet from the ``||scene:Scene||`` drawer to get the screen height.

Combine these two conditions using an **or** operator, since you want the game to end if either condition is true. The **or** operator in TypeScript is represented by two vertical bars: **`||`**.

---

If either of these conditions is true, we want to end the game.

*   Inside the *body* of the **if** statement, drag a code snippet from the ``||game:Game||`` drawer to end the game. Send **`false`** as the argument to indicate that the player has lost.

The *body* of an **if** statement is surrounded by curly braces: **`{ }`**.

Restart the simulator.

Now, if our hero flies off the screen, the game ends!

#### ~ tutorialhint

```typescript
/**
 * Event handlers
 */
game.onUpdate(function () {
    if (
        heroSprite.top < 0 ||
        heroSprite.bottom > scene.screenHeight()
    ) {
        game.gameOver(false)
    }
})

```

## Keeping score

Now, let's have a way for our hero to earn points!

Remember that logs will fly into the screen from the right side of the screen.
If our hero dodges the logs, the player earns points.

We are going to set the speed of multiple projectiles, so let's create a variable.

*   In the **Global variables** section, create a variable called **`projectileVx`** of type **`number`**. Set the initial value to **`-45`**.

```typescript
/**
 * Global variables
 */
let projectileVx: number = -45
```

Now, let's create the scoring projectile that the player will touch to earn points.

1.  In the **Event handlers** section, from the ``||game:Game||`` drawer, add a code snippet that runs code on an interval of time.
1.  Change the time from *500* to **1500** milliseconds.
1.  Into this new event handler, from the ``||sprites:Sprites||`` drawer, add a code snippet that creates a projectile from the side.
1.  We are going to have multiple projectiles, so change the variable name from *projectile* to **`scoreProjectile`**.
1.  Select the artist palette next to the line number to open the image editor.
1.  In the image editor, draw a tall thin rectangle to represent the scoring projectile. Make the image **2** pixels wide and **120** pixels tall so that it fills the screen. Be sure to fill the rectangle with a color (any color besides transparent). Select **Done** to set your projectile's image.
1.  Change the other arguments in the function to set the **vx** (velocity x) value to the **`projectileVx`** variable and the **vy** (velocity y) value to **`0`**.
1.  Use a code snippet from the ``||sprites:Sprites||`` drawer to set the kind of the **`scoreProjectile`** to **`AddScore`**.
1.  Set the **top** property of the **`scoreProjectile`** to **`0`** so that it starts at the top of the screen.

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

~hint How do I know which function argument is which?

When you hover your mouse over a function, a tooltip appears that describes each argument in order.

This is especially helpful for functions that take multiple arguments.

Give it a try with the createProjectileFromSide function!

hint~

Restart the simulator.

Now, scoring projectiles fly in from the right side of the screen every 1 1/2 seconds!

---

Finally, let's make it so that when our hero touches a scoring projectile, the player earns a point.

1.  In the **Event handlers** section, from the ``||sprites:Sprites||`` drawer, add a new event handler for when two sprites overlap.
1.  Change the second kind from *Player* to **AddScore**.
1.  From the ``||sprites:Sprites||`` drawer, add a code snippet to destroy a sprite.
1.  Change the argument to destroy the **`otherSprite`** (the scoring projectile).
1.  From the ``||info:Info||`` drawer, add a code snippet to change the score by **`1`**.

Restart the simulator.

Now, when our hero touches a scoring projectile, the player earns a point!

#### ~ tutorialhint

```typescript
game.onUpdateInterval(1500, function () {
    let scoreProjectile: Sprite = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)
})
sprites.onOverlap(SpriteKind.Player, SpriteKind.AddScore, function (sprite, otherSprite) {
    sprites.destroy(otherSprite)
    info.changeScoreBy(1)
})
```

## Avoid the logs!

Now, let's add the logs that our hero must avoid. In this version of the game, we will create a simplified set of logs.

1.  At the bottom of your ``||game(noclick):game.onUpdateInterval(1500)||`` event handler, create two new variables of type **`Sprite`** called **`topSprite`** and **`bottomSprite`**. Set their initial values to **`null`**.
1.  At the bottom of your ``||game(noclick):game.onUpdateInterval(1500)||`` event handler, add an   
``||logic:if else||``   
code snippet from the ``||logic:Logic||`` drawer.
1.  In place of the *true* placeholder, drop a   
``||math:[percentage] % chance||``   
code snippet from the ``||math:Math||`` drawer.
1.  Change the *percentage* value to **50** to create a 50% chance for each branch of your ``||logic(noclick):if||`` statement.
1.  In the *true* branch of your ``||logic(noclick):if||`` block,
add **two** code snippets from the ``||sprites:Sprites||`` drawer to create two projectiles from the side.
1.  Remove the **`let`** keywords, as you have already declared the variables.
1.  Change the variable names from *projectile* to **topSprite** in the first statement and to **bottomSprite** in the second statement.
1.  For the **topSprite** image, select one of the log images from the Gallery. We suggest using the three-segment log from the second set of log images. (The image is named **`sprites.duck.log7`** if you wish to use a built-in name.)
1.  For the **bottomSprite** image, select another log image from the Gallery. We suggest using the two-segment log from the first set of log images. (The image is named **`sprites.duck.log2`** if you wish to use a built-in name.)
1.  For both projectiles, in place of the *`50`* placeholders, set the *vx* value to **`projectileVx`** and the *vy* value to **`0`**.
1.  Copy these two statements and paste them into the *false* branch of your   
``||logic(noclick):if||`` block. Change the images to different log images. (We suggest using **`sprites.duck.log6`** for the top log and **`sprites.duck.log3`** for the bottom log.)


This code will randomly select one of the two sets of logs to create. Now, we just need to place the sprites in the correct location.

*   Still inside the ``||game(noclick):game.onUpdateInterval(1500)||`` event handler and beneath the ``||logic(noclick):if||`` block, set the **top** of the **topSprite** to **0** and the **bottom** of the **bottomSprite** to the screen height. The ``||scene:screen height||`` code snippet can be found in the ``||scene:Scene||`` drawer.

Restart the simulator.

Now, logs fly in from the right side of the screen every 1 1/2 seconds!

---

We don't need to see the scoring projectile, so let's make it invisible.

1.  To the **bottom** of your ``||game(noclick):game.onUpdateInterval(1500)||`` event handler, add a code snippet from the ``||sprites:Sprites||`` drawer to set a flag of a sprite.
1.  Change the variable from *`mySprite`* to **`scoreProjectile`**.
1.  Change the property from *`AutoDestroy`* to **`Invisible`**.
1.  Change the value from *`false`* to **`true`**.

Restart the simulator.

Now, the scoring projectile is invisible!

#### ~ tutorialhint

```typescript
game.onUpdateInterval(1500, function () {
    let scoreProjectile: Sprite = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
    scoreProjectile.top = 0
    scoreProjectile.setKind(SpriteKind.AddScore)

    // @highlight
    // New code starts here!
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
    scoreProjectile.setFlag(SpriteFlag.Invisible, true)
})
```

## Losing lives

Finally, let's make it so that if our hero hits a log, the player loses a life.

1.  In the **Event handlers** section, add a new event handler from the ``||sprites:Sprites||`` drawer for when two sprites overlap.
1.  Change the second kind from *Player* to **Projectile**.
1.  In the body of this new event handler, from the ``||sprites:Sprites||`` drawer, add a code snippet that destroys a sprite.
1.  Change the argument to destroy the **`otherSprite`** (the log).
1.  In the argument list for the destroy function, type a *comma* **`,`** to add a new argument for the effect. Select an effect for the destruction animation. We suggest **`effects.disintegrate`**.
1.  Add another argument for the duration of the effect. Set the duration to **`500`** milliseconds.
1.  Still within this event handler, from the ``||info:Info||`` drawer, add a code snippet to change the life by **`-1`**.

Restart the simulator.

Now, if our hero hits a log, the player loses a life!

#### ~ tutorialhint

```typescript
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

```ghost
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
    let scoreProjectile: Sprite = sprites.createProjectileFromSide(img`.`, projectileVx, 0)
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