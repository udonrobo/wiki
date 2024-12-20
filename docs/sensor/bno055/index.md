# BNO055

## 🌟 概要

BNO055 は、9 軸のセンサーを搭載した IMU（慣性計測装置）です。

主にロボットの旋回角を取得するために使用されます。

## 🌟 配線

メイン基板上に BNO055 を接続する用の端子があるため、そこに接続します。

-- 画像 --

## 🌟 ファームウエア

`Udon::BNO055` クラスを使用します。ドキュメントにサンプルスケッチが載っています。

<https://github.com/udonrobo/UdonLibrary/blob/master/docs/Driver/BNO055.md>

```cpp title="Teensy4.0で旋回角を取得する例"
#include <Udon.hpp>

static Udon::BNO055 gyro{ Wire };

void setup()
{
    Serial.begin(115200);
    gyro.begin();
}

void loop()
{
    gyro.update();

    const double yaw = gyro.getYaw();

    Serial.println(yaw);

    delay(10);
}

```
