# ESP32-S3-LED
在 ESP32-S3 上点亮 LED 最常见有两种方式：通过 GPIO 控制普通单色 LED，或者控制开发板自带的板载 WS2812 RGB 灯珠（官方 DevKitC-1 等常见开发板通常在 GPIO 48 或 GPIO 38 上集成了一颗 WS2812 彩灯）。

1. 点亮普通单色 LED（或部分带普通板载灯的模组）

如果你外接了一颗普通 LED（正极串联 220Ω~1kΩ 电阻接 GPIO，负极接 GND），或者使用自带普通 LED 的开发板，可以使用以下代码：
cpp
// 定义连接 LED 的引脚（请根据实际接线修改，如 GPIO 2）
const int LED_PIN = 2;

void setup() {
  // 将引脚设置为输出模式
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH); // 输出高电平，点亮 LED
  delay(1000);                 // 保持 1 秒（1000 毫秒）
  
  digitalWrite(LED_PIN, LOW);  // 输出低电平，熄灭 LED
  delay(1000);                 // 保持 1 秒
}
请谨慎使用此类代码。
提示：如果只需要常亮而不闪烁，只需保留 digitalWrite(LED_PIN, HIGH); 并删除 LOW 和 delay 即可。

2. 点亮板载 WS2812 RGB 彩灯（ESP32-S3-DevKitC-1 常用）

乐鑫官方及大多数第三方 ESP32-S3 开发板没有普通单色 LED，而是集成了一颗可编程 RGB 灯珠（通常接在 GPIO 48，部分版本为 GPIO 38）。直接使用 digitalWrite 无法点亮它，需要使用内置的 neopixelWrite 函数：
cpp
// 常见 ESP32-S3 开发板板载 RGB 灯引脚为 48 或 38
#define RGB_PIN 48 

void setup() {
  // neopixelWrite 无需额外初始化 pinMode
}

void loop() {
  // 参数格式：neopixelWrite(引脚, 红R, 绿G, 蓝B)，亮度范围 0~255
  neopixelWrite(RGB_PIN, 64, 0, 0);   // 亮红灯（建议亮度设为 50~100，避免刺眼）
  delay(1000);

  neopixelWrite(RGB_PIN, 0, 64, 0);   // 亮绿灯
  delay(1000);

  neopixelWrite(RGB_PIN, 0, 0, 64);   // 亮蓝灯
  delay(1000);

  neopixelWrite(RGB_PIN, 0, 0, 0);    // 熄灭
  delay(1000);
}
