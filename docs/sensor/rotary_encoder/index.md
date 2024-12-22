# ロータリーエンコーダー

## 🌟 概要

回転角度を測定するセンサーです。

## 🌟 配線

=== "全体図"

    ![alt text](<Photo 2024-12-21, 11 36 26.jpg>)

=== "中継用配線"

    ![alt text](IMG_20230216_123333.jpg)

## 🌟 最小構成のコード例

測定角度はカウント値として取得できます。一周分のカウント値はエンコーダーの分解能によって決まります。

=== "Raspberry Pi Pico で制御"

    ```cpp
    #include <Udon.hpp>

    static Udon::EncoderPico encoder{ 12, 13 };  // A, B ピン番号

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

## 🌟 実際のコード例

サブマイコンでエンコーダーのカウント値を計測し、CAN でメッセージを送信する例です。

一つのエンコーダーにつき、一つの CNA ID を持たせて送信します。

```cpp title="サブマイコン側 (Raspberry Pi Pico)"
#include <Udon.hpp>

class CanEncoderWriter
{
    Udon::EncoderPico encoder;
    Udon::CanWriter<Udon::Message::Encoder> canWriter;

public:
    CanEncoderWriter(Udon::EncoderPico&& encoder, Udon::CanWriter<Udon::Message::Encoder>&& canWriter)
        : encoder{ std::move(encoder) }
        , canWriter{ std::move(canWriter) }
    {
    }

    void begin()
    {
        encoder.begin();
    }

    void update()
    {
        const int32_t count = encoder.read();
        canWriter.setMessage({ count });
    }
};

static Udon::CanBusSpi bus;

static CanEncoderWriter encoders[] {
    CanEncoderWriter {
        Udon::EncoderPico{ 12, 13 },
        Udon::CanWriter<Udon::Message::Encoder>{ bus, 0x001 }
    },
    CanEncoderWriter {
        Udon::EncoderPico{ 14, 15 },    
        Udon::CanWriter<Udon::Message::Encoder>{ bus, 0x002 }
    },
    CanEncoderWriter {
        Udon::EncoderPico{ 16, 17 },
        Udon::CanWriter<Udon::Message::Encoder>{ bus, 0x003 }
    },
    CanEncoderWriter {
        Udon::EncoderPico{ 18, 19 },
        Udon::CanWriter<Udon::Message::Encoder>{ bus, 0x004 }
    },
};

static Udon::LoopCycleController loopCtrl{ 1000 };

void setup()
{
    bus.begin();

    for (auto& encoder : encoders)
    {
        encoder.begin();
    }
}

void loop()
{
    bus.update();

    for (auto& encoder : encoders)
    {
        encoder.update();
    }

    loopCtrl.update();
}
```

```cpp title="メインマイコン側 (Teensy4.0)"
#include <Udon.hpp>

class CanEncoderReader
{
    Udon::CanReader<Udon::Message::Encoder> canReader;

public:
    CanEncoderReader(Udon::CanReader<Udon::Message::Encoder>&& canReader)
        : canReader{ std::move(canReader) }
    {
    }

    int32_t getCount() const
    {
        if (const auto message = canReader.getMessage())
        {
            return message->count;
        }

        Serial.println("Encoder node not found");
        return 0;
    }
};

static Udon::CanBusTeensy<CAN1> bus;

static CanEncoderReader encoders[] {
    CanEncoderReader{{ bus, 0x001 }},
    CanEncoderReader{{ bus, 0x002 }},
    CanEncoderReader{{ bus, 0x003 }},
    CanEncoderReader{{ bus, 0x004 }},
};

static Udon::LoopCycleController loopCtrl{ 10000 };

void setup()
{
    Serial.begin(115200);
    bus.begin();
}

void loop()
{
    bus.update();

    const int32_t count1 = encoders[0].getCount();
    const int32_t count2 = encoders[1].getCount();
    const int32_t count3 = encoders[2].getCount();
    const int32_t count4 = encoders[3].getCount();

    Serial.println(count1);
    Serial.println(count2);
    Serial.println(count3);
    Serial.println(count4);

    loopCtrl.update();
}
```
