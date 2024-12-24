# シリアル LED

デイジーチェーン接続できるシリアル LED です。そのため制御線 1 本で複数の LED を制御できます。

## 🌟 配線

定格電圧は 5V です。

-- 画像 --

## 🌟 ファームウエア

Adafruit の NeoPixel ライブラリを使用します。

```cpp title="RaspberryPi Pico での使用例"
#include <Adafruit_NeoPixel.h>

const int pin  = 0;    // 制御線のピン番号
const int nLed = 20;   // LED の数

static Adafruit_NeoPixel leds{ nLed, pin };

void setup()
{
    leds.begin();
    leds.setBrightness(100);  // 最大出力は明るすぎるので調整
}

void loop()
{
    for (int i = 0; i < nLed; ++i)
    {
        leds.setPixelColor(i, 0xaaaaaa);
    }

    leds.show();

    delay(10);
}
```
