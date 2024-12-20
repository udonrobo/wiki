# DC モーター

## 🌟 概要

直流電源を供給することで回転します。モータードライバを介して制御します。

## 🌟 配線

マブチ 385 モータの場合の配線図です。モーターによって定格電圧が異なるので、適切な電圧を供給してください。この場合は 24V です。

=== "小電流用"

    ![alt text](<Photo 2024-12-19, 18 25 03.jpg>)

=== "大電流用"

    ![alt text](<Photo 2024-12-19, 18 38 58.jpg>)

=== "制御線が 2 線のタイプ"

    DIR 線と PWM 線の 2 線のタイプです。あまり使わないので配線図は割愛します。

    ![alt text](<Photo 2024-12-19, 18 41 29.jpg>)

    ![alt text](<Photo 2024-12-19, 18 41 36.jpg>)

## 🌟 ファームウエア / 制御線 3 線

ピン番号は基板上に記載されています。

=== "最小構成"

    ```cpp
    const int pinA = 0;
    const int pinB = 2;
    const int pinP = 1;

    void setup()
    {
        pinMode(pinA, OUTPUT);
        pinMode(pinB, OUTPUT);
    }

    void loop()
    {
        digitalWrite(pinA, HIGH);
        digitalWrite(pinB, LOW);
        analogWrite(pinP, 100);  // 0~255 の範囲でPWM出力
    }
    ```

=== "Udon::Motor3 クラスを使う"

    ```cpp
    #include <Udon.hpp>

    static Udon::Motor3 motor{ 0, 2, 1 };

    void setup()
    {
        motor.begin();
    }

    void loop()
    {
        motor.move(100);
    }
    ```

## 🌟 ファームウエア / 制御線 2 線

=== "最小構成"

    ```cpp
    const int pinA = 0;
    const int pinP = 1;

    void setup()
    {
        pinMode(pinA, OUTPUT);
    }

    void loop()
    {
        digitalWrite(pinA, HIGH);
        analogWrite(pinP, 100);  // 0~255 の範囲でPWM出力
    }
    ```

=== "Udon::Motor2 クラスを使う"

    ```cpp
    #include <Udon.hpp>

    static Udon::Motor2 motor{ 0, 1 };

    void setup()
    {
        motor.begin();
    }

    void loop()
    {
        motor.move(100);
    }
    ```
