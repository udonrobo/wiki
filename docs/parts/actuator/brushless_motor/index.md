# ブラシレスモーター

![alt text](thumbnail.jpg)

## 🌟 概要

ESC と組み合わせて使用します。ESC に PWM で制御信号を送ると回転します。

## 🌟 配線

定格電圧は ESC によって決まります。ESC に対応しているリポバッテリーのセル数が記載されています。写真の場合、2~6 セル です。

ESC とブラシレスモーターの間の配線は一応極性がありますが、気にせず繋げてよいです。(回転方向が変わるだけです)

制御線はサーボモーターのコネクタと互換性があるので、サーボモーターのコネクタに接続します。

=== "全体"

    ![alt text](wire.jpg)

=== "アップ"

    ![alt text](up.jpg)

!!! note

    ESC から 5V が供給されるためサーボ用電源は不要です。

## 🌟 ファームウエア

```cpp title="RaspberryPi Pico での使用例"
#include <Udon.hpp>

static Udon::Servo motor;

void setup()
{
    // モーターの制御ピンを設定
    motor.attach(3);

    // 最大パルス幅を送信
    motor.writeMicroseconds(2000);
    delay(2000);

    // 最小パルス幅を送信
    motor.writeMicroseconds(1000);
    delay(2000);
}

void loop()
{
    motor.writeMicroseconds(1100);  // 1000~2000 の範囲でパルス幅を指定
}
```

!!! warning

    想像よりかなり速く回転します。安全面から最小値から少しずつ値を増やしていくことをおすすめします。

!!! note "電源投入順"

    駆動電源を入れてから回路電源を入れる必要があります。
    
    ESC の電源は駆動電源から取るため、駆動電源のあとに回路電源を入れると setup関数内で送信している初期化信号を ESC が受信できません。

!!! note "値の範囲"

    単方向用の ESC はパルス幅が最小のときに停止します。

    両回転対応の ESC はパルス幅が中間値のとき停止します。(部にある ESC の場合)
