# My Tutorial

```validation.global
# BlocksExistValidator
```
# Flash-a-rama
## こ1のチュートリアルでは、2台のmicro:bitを使って、無線通信による簡単なゲームを作る方法を学びます。micro:bitには「radio（無線）」機能があり、ボタンを押すと相手のmicro:bitにメッセージを送ることができます。@showdialog

## Step 2

micro:bit の通信機能を使った対戦ゲームをつくりましょう。「ずっと」ブロックは使わないので左側のメニューの上までもってきてゴミ箱に捨ててください。


## Step 3

はじめに、スプライトを配置しましょう！
スプライトは、LED画面上に表示される点（ピクセル）で表されるゲームのキャラクターです。
このゲームでは、自分「player」と弾丸「bullet」、敵「enemy」の３つのキャラクタを使うことにします。
まず、スプライトを入れる変数を定義します。変数から、「変数を追加する ... 」をクリックして 「player」「bullet」「enemy」の３つの変数を作ってください。

```blocks
// @validate-exists

let player = 0
let bullet = 0
let enemy = 0
```


## Step 4

micro:bitのLED画面は5×5のグリッドで、スプライトの位置は (x, y) で表されます（例：左上が (0, 0)、右下が (4, 4)）です。
最初だけブロックに、スプライトを一番下の中央に配置してください。

```blocks
//@validate-exists

player = game.createSprite(2, 4)
```

## Step 5

次は、micro:bit 本体を右に傾けたら player を右に動くようにしましょう。
「入力」にある「ゆさぶられたとき」 ``input.onGesture(Gesture.Shake, function () {})`` ブロックを配置して「ゆさぶられた」の部分を「右に傾けた」にかえます。
そして、「ゲーム」にある「スプライトのxを１だけ増やす」ブロックを配置して、スプライトを「player」に変更します。

```blocks
//@validate-exists
let player: game.LedSprite = null
input.onGesture(Gesture.TiltRight, function () {
    player.change(LedSpriteProperty.X, 1)
})
```

## Step 6

同じように、micro:bit本体を左に傾けたら player を左に動くようにしましょう。

```blocks
//@validate-exists
let player: game.LedSprite = null
input.onGesture(Gesture.TiltLeft, function () {
    player.change(LedSpriteProperty.X, -1)
})
```

## ここまでできたら、プログラムをダウンロードして、micro:bit を左右に傾けたときにスプライトが左右に動くことを確認しましょう。@showdialog

## micro:bit同士が通信するには、同じグループ番号を設定する必要があります。無線グループを設定して通信できるように準備しましょう。@showdialog

## Step 9
```validation.local
# BlocksExistValidator
* Enabled: false
```

「無線」にある「無線のグループを設定」を「最初だけ」にセットして、無線グループの番号を入力します。
数字が全角だと、エラーになるので注意してください。


```blocks
radio.setGroup(1)
```
## Step 10 

次に、Aボタンが押されたときに、攻撃をするようにしましょう。
ボタンAが押された時を配置してください。そして、変数 bullet を定義して、新しいスプライトを player と同じ場所に配置しましょう。

```blocks
//@validate-exists
let player: game.LedSprite = null
input.onButtonPressed(Button.A, function () {
    bullet = game.createSprite(player.get(LedSpriteProperty.X), player.get(LedSpriteProperty.X))
})
```

## Step 11

bullet を前にすすめます。「ループ」にある「くりかえし」ブロックをつかって、bullet の 500ミリ秒まち、y座標を１減らすを、４回繰り返します。

```blocks
let player: game.LedSprite = null
let bullet: game.LedSprite = null
input.onButtonPressed(Button.A, function () {
    bullet = game.createSprite(player.get(LedSpriteProperty.X), player.get(LedSpriteProperty.X))
    for (let index = 0; index < 4; index++) {
        basic.pause(500)
        bullet.delete()
        bullet.change(LedSpriteProperty.Y, -1)
    }
})
```
## Step 12

bulett が上端まできたら、bullet を消して、bullet のx座標を、「無線で数値を送信」ブロックを使って対戦相手に送ります。

```blocks
let player: game.LedSprite = null
let bullet: game.LedSprite = null
input.onButtonPressed(Button.A, function () {
    bullet = game.createSprite(player.get(LedSpriteProperty.X), player.get(LedSpriteProperty.X))
    for (let index = 0; index < 4; index++) {
        basic.pause(500)
        bullet.change(LedSpriteProperty.Y, -1)
    }
    radio.sendNumber(bullet.get(LedSpriteProperty.X))
})
```

## Step 13

次は、対戦相手のプログラムです。無線で受信した時（receivedNumber）を使って、数値を受け取ったときに、受け取った数値をx座標にして、スプライトenemyを画面の最上端に表示させます。

```blocks
let player: game.LedSprite = null
let enemy: game.LedSprite = null
radio.onReceivedNumber(function (receivedNumber) {
    enemy = game.createSprite(receivedNumber, 0)
})
```

## Step 14
500ミリ秒まち、y座標を１増やすを、４回繰り返します。

```blocks
let bullet: game.LedSprite = null
let enemy: game.LedSprite = null
radio.onReceivedNumber(function (receivedNumber) {
    bullet = game.createSprite(receivedNumber, 0)
    for (let index = 0; index < 4; index++) {
        basic.pause(500)
        enemy.change(LedSpriteProperty.Y, 1)
    }
})
```

## Step 15
「論理」にある「もし〜なら」ブロックを使って、player が enemy に触ったら、アイコンを表示して、スプライト enemy を消します。 

```blocks
let player: game.LedSprite = null
let enemy: game.LedSprite = null
radio.onReceivedNumber(function (receivedNumber) {
    enemy = game.createSprite(receivedNumber, 0)
    for (let index = 0; index < 4; index++) {
        basic.pause(500)
        enemy.change(LedSpriteProperty.Y, 1)
    }
    if (player.isTouching(enemy)) {
        basic.showIcon(IconNames.Asleep)
    }
    enemy.delete()
})
```

## Step 16
 you win というメッセージを送ります。

```blocks
let player: game.LedSprite = null
let enemy: game.LedSprite = null
radio.onReceivedNumber(function (receivedNumber) {
    enemy = game.createSprite(receivedNumber, 0)
    for (let index = 0; index < 4; index++) {
        basic.pause(500)
        enemy.change(LedSpriteProperty.Y, 1)
    }
    if (player.isTouching(enemy)) {
        basic.showIcon(IconNames.Asleep)
    }
    enemy.delete()
    radio.sendString("You win!")
})
```

## Step 17
 最後に、「無線で受信したとき（resievedString）」を使って、メッセージを受け取ったら表示するようにしましょう。

```blocks
radio.onReceivedString(function (receivedString) {
    basic.showString(receivedString)
})
```

## これで終わりです。ダウンロードして対戦してみましょう。@showdialog