# ロータリーエンコーダー

## 🌟 概要

回転角度を測定するセンサーです。

## 🌟 配線

-- 画像 --

## 🌟 ファームウエア

=== "Raspberry Pi Pico で制御"

    ```cpp
    #include <Udon.hpp>

    static Udon::Encoder encoder{ 0, 1 };  // A, B ピン番号

    void setup()
    {
        encoder.begin();
    }

    void loop()
    {
        const int32_t count = encoder.read();
    }
    ```

    !!! warning

        A, B ピンは隣り合っている必要があります。

    !!! note

        PIO を使って計測します。よって最大 8 個までのエンコーダーを接続できます。

=== "Teensy で制御"

    ```cpp
    #include <Encoder.h>

    static Encoder encoder{ 0, 1 };  // A, B ピン番号

    void setup()
    {
        Serial.begin(115200);
        encoder.begin();
    }

    void loop()
    {
        const int32_t count = encoder.read();

        Serial.println(count);
        
        delay(10);
    }
    ```

