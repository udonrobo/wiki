# スイッチ

## 🌟 概要

押されたか押されていないかを検知するセンサーです。

## 🌟 配線

基板上のスイッチの端子に接続します。極性はありません。

![alt text](<Photo 2024-12-21, 11 41 00.jpg>)

## 🌟 最小構成のコード例

digitalRead() 関数を使ってスイッチの状態を読み取ります。押されていないときに入力電圧が不安定にならないようにプルアップします。

押すとグランドに接続されるので、押されたときに LOW になります。

```cpp
const int pin = 26;  // スイッチのピン番号

void setup()
{
    Serial.begin(115200);
    
    pinMode(pin, INPUT_PULLUP);
}

void loop()
{
    const bool isPressed = digitalRead(pin) == LOW;

    Serial.println(isPressed);

    delay(10);
}
```

## 🌟 実際のコード例

サブマイコンでスイッチを状態を計測し、CAN でメッセージを送信する例です。

一つのスイッチにつき、一つの CNA ID を持たせて送信します。

```cpp title="サブマイコン側 (Raspberry Pi Pico)"
#include <Udon.hpp>

class Switch
{
    uint8_t pin;

public:
    Switch(uint8_t pin)
        : pin{ pin }
    {}

    void begin()
    {
        pinMode(pin, INPUT_PULLUP);
    }

    bool isPressed() const
    {
        return digitalRead(pin) == LOW;
    }
};

class CanSwitchWriter
{
    Switch sw;
    Udon::CanWriter<Udon::Message::Switch> canWriter;

public:
    CanSwitchWriter(Switch&& sw, Udon::CanWriter<Udon::Message::Switch>&& canWriter)
        : sw{ std::move(sw) }
        , canWriter{ std::move(canWriter) }
    {
    }

    void begin()
    {
        sw.begin();
    }

    void update()
    {
        const bool isPressed = sw.isPressed();
        canWriter.setMessage({ isPressed });
    }
};

static Udon::CanBusSpi bus;

static CanSwitchWriter switches[] {
    CanSwitchWriter {
        Switch{ 2 },
        Udon::CanWriter<Udon::Message::Switch>{ bus, 0x001 }
    },
    CanSwitchWriter {
        Switch{ 4 },    
        Udon::CanWriter<Udon::Message::Switch>{ bus, 0x002 }
    },
    CanSwitchWriter {
        Switch{ 6 },
        Udon::CanWriter<Udon::Message::Switch>{ bus, 0x003 }
    },
    CanSwitchWriter {
        Switch{ 8 },
        Udon::CanWriter<Udon::Message::Switch>{ bus, 0x004 }
    },
};

static Udon::LoopCycleController loopCtrl{ 1000 };

void setup()
{
    bus.begin();

    for (auto& sw : switches)
    {
        sw.begin();
    }
}

void loop()
{
    bus.update();

    for (auto& sw : switches)
    {
        sw.update();
    }

    loopCtrl.update();
}
```

```cpp title="メインマイコン側 (Teensy4.0)"
#include <Udon.hpp>

class CanSwitchReader
{
    Udon::CanReader<Udon::Message::Switch> canReader;

public:
    CanSwitchReader(Udon::CanReader<Udon::Message::Switch>&& canReader)
        : canReader{ std::move(canReader) }
    {
    }
    
    Udon::Optional<bool> isPressed() const
    {
        return canReader
            .getMessage()
            .transform([](const auto& message) { return message.press; });
    }
};

static Udon::CanBusTeensy<CAN1> bus;

static CanSwitchReader switches[] {
    CanSwitchReader{{ bus, 0x001 }},
    CanSwitchReader{{ bus, 0x002 }},
    CanSwitchReader{{ bus, 0x003 }},
    CanSwitchReader{{ bus, 0x004 }},
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

    for (const auto& sw : switches)
    {
        if (const auto isPressed = sw.isPressed())
        {
            Serial.println(*isPressed);
        }
        else
        {
            Serial.println("Switch node not found");
        }
    }

    loopCtrl.update();
}
```
