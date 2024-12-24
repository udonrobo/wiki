# ブラシレスモーター

## 🌟 概要

ESC と組み合わせて使用することで、制御できます。PWM で制御信号を送ることで、指定した速度でモーターを回転させることができます。

## 🌟 配線

部で使用しているモーターの定格電圧は ~24V です。

-- 画像 --

## 🌟 ファームウエア

細かく制御するために `writeMicroseconds` 関数を使用します。この関数は、指定したマイクロ秒でモーターを制御します。

```cpp title="RaspberryPi Pico での使用例"
#include <Udon.hpp>

static Udon::Servo motor;

void setup()
{
    // モーターの制御ピンを設定
    motor.attach(3);

    // キャリブレーション
    motor.writeMicroseconds(2000);
    delay(100);
    motor.writeMicroseconds(1000);
    delay(3000);
}

void loop()
{
    int power = 100;
    motor.writeMicroseconds(map(power, 0, 255, 1470, 2000));
}
```

!!! note "注意"

    単方向用の ESC はデューティー比が最小のときに停止します。

    両回転対応の ESC はデューティー比が中間値のとき停止します。
