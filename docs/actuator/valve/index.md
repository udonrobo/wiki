# 電磁弁

## 🌟 概要

エアシリンダーなどの空気圧機器を制御するための部品です。電磁石を用いて弁を開閉することで、空気の流れを制御します。

## 🌟 配線

定格電圧は 12V なので DCDC コンバーターで降圧し供給します。

-- 画像 --

## 🌟 ファームウエア

```cpp
const int pin = 2;

void setup()
{
    pinMode(pin, OUTPUT);
}

void loop()
{
    digitalWrite(pin, HIGH);
    delay(100);
    digitalWrite(pin, LOW);
    delay(1000);
}
```

!!! warning

    長時間電流を流すと発熱するため、1 秒以上連続で ON にしないようにしてください。
