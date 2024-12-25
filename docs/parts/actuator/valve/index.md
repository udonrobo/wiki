# 電磁弁

## 🌟 概要

エアシリンダーなどの空気圧機器を制御するための部品です。電磁石を用いて弁を開閉することで、空気の流れを制御します。

## 🌟 配線

定格電圧は 12V です。極性があるので注意してください。

![alt text](wire.jpg)

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
