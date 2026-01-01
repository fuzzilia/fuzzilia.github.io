# C107 左手デバイスって自作できるの!? 補足資料

このページは、サークルfuzziliaがコミックマーケット107にて頒布した漫画の補足資料となります。

主に漫画中のURLやコードなど目で見て打ち込むのが大変な部分をテキストとして提供し、細かい補足を行います。

それらが必要ないページについては、ここでは取り上げません。ご了承ください。

## P5 Arduino IDE のダウンロード

Arduino IDE は以下のページからダウンロードしてください。

[https://www.arduino.cc/en/software/](https://www.arduino.cc/en/software/)

## P6 Arduino IDE の設定

メニューバーから下記操作で設定画面を開きます。

- Macの場合 : Arduino IDE -> Preferences
- Windowsの場合 : File -> Preferences

設定画面で以下の2つの設定を行います。

- Language を 日本語に
- Additional boards manager URLs に `https://github.com/earlephilhower/arduino-pico/releases/download/global/package_rp2040_index.json` を入力

## P8 Raspberry Pi Pico ボードのインストールとボード選択

### ボードのインストール

1. 左のアイコン一覧から ボードマネージャーアイコンをクリック
   - ![ボードマネージャーアイコン](board_manager_icon.png)
2. 出てきた左カラムの検索テキスト欄に rasp と入力
3. `Raspberry Pi Pico/RP2040/RP2350` のインストールボタンをクリック

ボード一覧に出てくる `Arduino Mbed OS RP2040 Boards` は、P8で説明した公式が用意している Raspberry Pi Pico ボード用のデータです。
こちらは Raspberry Pi Pico W に対応していなかったり、キーボード機能が使えなかったりするので、今回は利用できません。

### ボード選択

1. メニューバーの ツール -> ボード を選択
2. Raspberry Pi Pico/RP2040/RP2350 を選択
3. Raspberry Pi Pico を選択

- たくさん選択肢がありますが、一番上のやつです。

### PCとボードを接続

PCとボードをUSBケーブルで接続します。
漫画のコマ割りの都合上このタイミングで接続していますが、この前から接続していても大丈夫です。

## P9 LED点滅プログラムの書き込み

下記をArduino IDEのテキスト欄に入力し、書き込みボタンをクリックすると書き込みが行われ、Raspberry Pi Pico に最初から配置されている小さいLEDが点滅します。

```c++
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_BUILTIN, HIGH);
  delay(1000);
  digitalWrite(LED_BUILTIN, LOW);
  delay(1000);
}
```

ちなみに、このプログラムはメニューのファイル -> スケッチ例 -> 01.Basics -> Blink と同じものです。
漫画ではコメントの文法の説明をしていない都合上、コメントは消してあります。

文法を知らない方でもなんとなくわかるかもしれませんが、 `//` から始まる行はコメントと呼ばれ、プログラム上意味ないものとして扱われ、エディタ上でも薄く表示されます。主にプログラムを読む人向けの説明を書くための部分です。
以下、漫画と異なり、このページではコメントを使ってプログラムの説明をする場合があります。

## P10 点滅のパターンを変えて再書き込み

P9のプログラムに2回出てくる `delay(1000);` の行の数字の部分をそれぞれ 200, 1500 に書き換えて書き込んでください。点滅のパターンが変わります。

2回目以降の書き込み時は、1回目と異なりポートの選択が必要となります。
これは、最初に書き込んだ時点でボードの挙動が変わったためで、これ以降書き込むためにはこのポート選択が必要となります(書き込みモードにして書き込む場合を除く)。
Arduino IDE が起動している間は前回選択したポートが引き続き選択されているので、書き込むたびにポート選択の操作をする必要はありません。

ポートはArduino IDE の書き込みアイコンの右の方にあるセレクトボックスをクリックすることで選択できます。
PCによっては複数選択肢が出ると思いますが、どれを選べばよいかはUSBケーブルを抜き差しして消えたり現れたりするやつ、という特定の仕方が楽です。

## P11 点滅のパターンを複雑にしてみる

loopの中身はこんな風に行数を増やしてもOKです。

```c++
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);
  delay(200);
  digitalWrite(LED_BUILTIN, LOW);
  delay(500);
  digitalWrite(LED_BUILTIN, HIGH);
  delay(100);
  digitalWrite(LED_BUILTIN, LOW);
  delay(2000);
}
```

ここではloopの中身だけ切り出して掲載していますが、setupも引き続き必要です。

## P12 キーボード入力

以下のコードに書き換えて書き込めば、Raspberry Pi Pico が3秒ごとに a b a b とキーを自動で打ち込むキーボードになります。

```c++
#include "Keyboard.h"

void setup() {
  Keyboard.begin();
}

void loop() {
  Keyboard.press('a');
  Keyboard.releaseAll();
  delay(3000);

  Keyboard.press('b');
  Keyboard.releaseAll();
  delay(3000);
}
```

書き込んだ直後からキーボードとして動くので、テキストエディタにフォーカスを当てていると、文字が勝手に入力されていきます。

ちなみに、漫画に説明を書き忘れましたが、 `#include "Keyboard.h"` の行はキーボード機能を使えるようにする、という意味合いです。

## P13 キーボード入力のプログラムを変えてみる

上記のプログラムも色々変えてみましょう。
LED点滅プログラムの様に待ち時間を変えても良いですし、入力するキーを変えても良いです。

P12のプログラムの内、

```c++
  Keyboard.press('a');
  Keyboard.releaseAll();
```

の部分が A キーを押して、すぐ離す、という意味のコードでした。
この部分を例えば

```c++
  Keyboard.press('@');
  Keyboard.releaseAll();
```

と変えれば、`@` が入力されるようになります。（キーボードの設定によっては別の記号になるかもしれませんが）

Ctrlのような修飾キーの入力は下記のようにします。

```c++
  // Macの場合の切り取りショートカット Cmd + x
  Keyboard.press(KEY_LEFT_GUI);
  Keyboard.press('x');
  Keyboard.releaseAll();

  // Windowsの場合の切り取りショートカット Ctrl + x
  Keyboard.press(KEY_LEFT_CTRL);
  Keyboard.press('x');
  Keyboard.releaseAll();
```

その他のキー入力をどう書くかは、[Arduinoの Keyboard Modifiers and Special Keys ページ](https://docs.arduino.cc/language-reference/en/functions/usb/Keyboard/keyboardModifiers/) を参照してください。

## P15

Raspberry Pi Pico のピン配置図は以下

https://datasheets.raspberrypi.com/pico/Pico-R3-A4-Pinout.pdf

## P19

GPIO1、GPIO2 にスイッチを繋げた場合、下記プログラムでGPIO1に繋いだスイッチを押すとaが入力され、GPIO2に繋いだスイッチを押すとbが入力されます。

```c++
#include "Keyboard.h"

void setup() {
  Keyboard.begin();
  pinMode(1, INPUT_PULLUP);
  pinMode(2, INPUT_PULLUP);
}

void loop() {
  while (digitalRead(1) == LOW) {
    Keyboard.press('a');
  }
  while (digitalRead(2) == LOW) {
    Keyboard.press('b');
  }
  Keyboard.releaseAll();
}
```

このプログラムは、変数を使わずに最低限の機能を実現するため、分かっている人から見ると不自然になっています。

```c++
  while (digitalRead(2) == LOW) {
    Keyboard.press('b');
  }
```

この部分はスイッチが押されている間は `Keyboard.press('b');` を実行し続ける、という意味のプログラムになっていますが、本来 `Keyboard.press('b');` を実行し続ける必要はなく、押された瞬間の最初の1回だけ実行する方が自然です。

また、このプログラムはチャタリング対策を省略していたり（手元で問題が起きなかったので...）２つのスイッチを同時押ししたときの挙動が微妙だったりするので、興味のあるかたは調べて改善してみてください。
