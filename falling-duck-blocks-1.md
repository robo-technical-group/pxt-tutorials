# Falilng Duck Blocks Level 1

## Falling Duck! @showdialog

Enjoy building *Falling Duck*!

![Sample of gameplay](https://robo-technical-group.github.io/pxt-tutorials/assets/images/falling_duck_demo.gif)

## Let's meet our hero!

Our hero character is a duck. Let's create our hero!

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
A sprite....
hint~

Watch the simulator restart.

Your hero appears in the middle of the screen!

~hint Why is the simulator so small?
If you need to see your project in more detail,
you can make the simulator larger by selecting the
zoom button.

Select the zoom button again to shrink the simulator
back into the corner of the screen.

Nearby buttons mute the sound and
stop the simulator.
hint~

Select the hint (light bulb) below if you need help.

```blocks
let mySprite: Sprite = null
mySprite = sprites.create(sprites.duck.duck3, SpriteKind.Player)
```

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
    if (Math.percentChance(50)) {
        topSprite = sprites.createProjectileFromSide(sprite.duck.log7, projectileVx, 0)
        bottomSprite = sprites.createProjectileFromSide(sprites.duck.log2, projectileVx, 0)
    } else {
        topSprite = sprites.createProjectileFromSide(sprites.duck.log6, projectileVx, 0)
        bottomSprite = sprites.createProjectileFromSide(sprites.duck.log3, projectileVx, 0)
    }
    topSprite.top = 0
    bottomSprite.bottom = scene.screenHeight()
})

```