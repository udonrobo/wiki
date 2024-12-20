# オムニを動かそう

## モータークラスを使って車輪を動かす

動作確認として単にモーターを動かすだけのプログラムを作成しましょう。

=== "Main.ino"

    ```cpp
    #include "Motor.hpp"

    Motor motor0{ 19, 17, 18 };    // RF
    Motor motor1{ 16, 15, 14 };    // RB
    Motor motor2{ 28, 26, 27 };    // LB
    Motor motor3{ 22, 20, 21 };    // LF

    void setup()
    {
        motor0.begin();
        motor1.begin();
        motor2.begin();
        motor3.begin();
    }

    void loop()
    {
        motor0.move(100);
        motor1.move(100);
        motor2.move(100);
        motor3.move(100);
    }
    ```

=== "Motor.hpp"

    ```cpp
    #pragma once

    class Motor
    {
        int pinA;
        int pinB;
        int pinP;

    public:

        Motor(int pinA, int pinB, int pinP)
            : pinA(pinA)
            , pinB(pinB)
            , pinP(pinP)
        {
        }

        void begin()
        {
            pinMode(pinA, OUTPUT);
            pinMode(pinB, OUTPUT);
        }

        void move(int power)
        {
            digitalWrite(pinA, (power >= 0) ? HIGH : LOW);
            digitalWrite(pinB, (power <= 0) ? HIGH : LOW);
            analogWrite(pinP, abs(power));
        }
    };
    ```

## コントローラーからデータを受信

コントローラーに載っている LoRa モジュールからデータを受信してみましょう。

`Udon::E220PadPS5` クラスを使うと簡単にコントローラーのデータを取得できます。

```cpp
#include <Udon.hpp>

Udon::E220PadPS5 pad({
    .serial  = Serial1,
    .m0      = 13,
    .m1      = 12,
    .aux     = 2,
});

void setup()
{
    const int channel = 3;
    pad.begin(channel);
}

void loop()
{
    pad.update();

    // バツボタンが押された瞬間
    if (pad.getCross().click)
    {
        Serial.println("Cross button is clicked.");
    }

    // 三角ボタンが押されている間
    if (pad.getTriangle().press)
    {
        Serial.println("Triangle button is pressed.");
    }

    delay(10);
}
```

スティックの値は `Udon::Vec2` 型で取得できます。

```cpp
void loop()
{
    pad.update();

    Udon::Vec2 leftStick = pad.getLeftStick();

    Serial.println(leftStick.x);
}
```
