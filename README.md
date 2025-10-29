# 🧸 ESP8266 OLED Doraemon Display (SSD1306 + FrameBuffer) — MicroPython

## 🧠 Overview
This project demonstrates how to display a **Doraemon bitmap image** on a **128x64 SSD1306 OLED display** using **MicroPython** and the **ESP8266**.  
It uses the **frame buffer (`framebuf`)** module to render monochrome images stored as **byte arrays**.

---

## ⚙️ Hardware Setup

| Component | ESP8266 Pin | Description |
|------------|-------------|--------------|
| OLED SDA   | GPIO4 (D2)  | I2C Data line |
| OLED SCL   | GPIO5 (D1)  | I2C Clock line |
| VCC        | 3.3V        | Power supply |
| GND        | GND         | Common ground |

🪛 **Connections:**
- OLED **VCC → 3.3V**  
- OLED **GND → GND**  
- OLED **SDA → GPIO4 (D2)**  
- OLED **SCL → GPIO5 (D1)**  

---

## 🧩 Code

```python
from machine import Pin, I2C
import ssd1306
import framebuf

# Doraemon 128x64 bitmap stored as bytearray
Doremon = bytearray(
    b'\x00\x00\x00\x00\x00\x00\x00\x0f\xff\xff\xf8\x00\x00\x00\x00\x00\x00...<truncated for brevity>...'
)

# D1 = SCL = GPIO5
# D2 = SDA = GPIO4
i2c = I2C(sda=Pin(4), scl=Pin(5))
display = ssd1306.SSD1306_I2C(128, 64, i2c)

# Create framebuffer and draw to OLED
fb = framebuf.FrameBuffer(Doremon, 128, 64, framebuf.MONO_HLSB)
display.fill(0)
display.blit(fb, 0, 0)
display.show()
