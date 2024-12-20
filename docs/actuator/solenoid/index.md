# ソレノイド

## 🌟 概要

電磁石を用いて機械的な動作を行うアクチュエータです。

## 🌟 配線

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
    delay(1000);
    digitalWrite(pin, LOW);
    delay(1000);
}
```
