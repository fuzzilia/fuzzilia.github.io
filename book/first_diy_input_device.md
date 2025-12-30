# C107 左手デバイスって自作できるの!? 補足資料

このページは、サークルfuzziliaがC107にて頒布した漫画の補足資料となります。
2026年1月3日までに公開予定です。

<!--

## LED 点滅通常

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

## LED 点滅時間変更

```c++
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);
  delay(200); // 点灯時間を200ミリ秒に変更
  digitalWrite(LED_BUILTIN, LOW);
  delay(1500); // 消灯時間を1500ミリ秒に変更
}
```

## 時間経過でキー入力

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

## PXX スイッチによるキー入力プログラム

```c++
#include "Keyboard.h"

void setup() {
  Keyboard.begin();
  pinMode(0, INPUT_PULLUP);
  pinMode(1, INPUT_PULLUP);
}

void loop() {
  while (digitalRead(0) == LOW) {
    Keyboard.press('a');
  }
  while (digitalRead(1) == LOW) {
    Keyboard.press('b');
  }
  Keyboard.releaseAll();
}
```

```c++
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

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

shift キー

MAC の場合(Cmd+x)

```c++
    Keyboard.press(KEY_LEFT_GUI);
    Keyboard.press('x');
```

Windows の場合(Ctrl+x)

```c++
    Keyboard.press(KEY_LEFT_CTRL);
    Keyboard.press('x');
```

-->
