# BNO055

## 🌟 概要

BNO055は、9軸のセンサーを搭載したIMU（慣性計測装置）です。

主にロボットの旋回角を取得するために使用されます。

## 🌟 配線

メイン基板上に BNO055 を接続する用の端子があるため、そこに接続します。

-- 画像 --

## 🌟 ファームウエア

`Udon::BNO055` クラスを使用します。ドキュメントにサンプルスケッチが載っています。

<https://github.com/udonrobo/UdonLibrary/blob/master/docs/Driver/BNO055.md>

```cpp
#include <Udon.hpp>

static Udon::BNO055 gyro{ Wire };

void setup()
{
    gyro.begin();
}

void loop()
{
    gyro.update();

    const double yaw = gyro.getYaw();
}
```