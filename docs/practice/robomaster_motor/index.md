# ロボマスモーターを回そう



```cpp
#include <Udon.hpp>

Udon::CanBusTeensy<CAN2> bus;

Udon::RoboMasterC620 motor{ bus, 1 };  // モーターID: 1

void setup()
{
    bus.begin();
}

void loop()
{
    bus.update();

    motor.setCurrent(1000);
}
```
