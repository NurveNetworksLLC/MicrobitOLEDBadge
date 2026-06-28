<h1>Micro:bit Electronic Badge / Gaming Adapter</h1>
<p align="center">
  <strong>Advanced 128x64 OLED Display & Dual Serialized 5-Way Joysticks for BBC Micro:bit (v1 & v2)</strong>
</p>

<p align="center">
  <img src="YOUR_IMAGE_FOLDER_URL/badge_hero_shot.jpg" alt="Micro:bit Electronic Badge Front View showing Pong Demo" width="65%">
</p>

<p>Welcome to the official repository for the <strong>Micro:bit Electronic Badge</strong>, engineered by <strong>Nurve Networks LLC</strong>[cite: 1]. This hardware expansion board transforms your BBC micro:bit into a fully self-contained portable gaming console, interactive wearable badge, or data-monitoring dashboard[cite: 1]. By leveraging high-efficiency parallel-to-serial shift registers, the badge provides an expansive 10-button input control map while preserving critical micro:bit GPIO resources for your own prototyping needs[cite: 1].</p>

<div style="background-color: #f4f6f8; border-left: 5px solid #007791; padding: 15px; margin: 20px 0; border-radius: 4px;">
  <strong>🛒 Get the Hardware:</strong> Ready to build your own wearable projects or retro games? You can purchase the production-ready Micro:bit Electronic Badge directly on Amazon here: <a href="YOUR_AMAZON_LISTING_LINK" target="_blank" style="color: #007791; font-weight: bold; text-decoration: underline;">Buy on Amazon</a>
</div>

<hr>

<h2>📸 Hardware Architecture at a Glance</h2>
<p align="center">
  <img src="YOUR_IMAGE_FOLDER_URL/PCB_Annotated_Layout.jpg" alt="Micro:bit Electronic Badge PCB Layout Annotation" width="60%">
</p>

<ul>
  <li><strong>Processor Compatibility:</strong> Plugs directly into any standard BBC micro:bit v1 or v2 via the top 40-pin card-edge interface (J1)[cite: 1].</li>
  <li><strong>High-Contrast Display:</strong> Integrated 128x64 pixel monochrome bitmapped OLED breakout module powered by the industry-standard SSD1306/1315 controller[cite: 1].</li>
  <li><strong>Dual 5-Way Tactical Controls:</strong> Two serialized joysticks (SW1 and SW2) located on either side of the display providing independent Up, Down, Left, Right, and center Click/Fire buttons (10 inputs total)[cite: 1].</li>
  <li><strong>Infinite Expansion:</strong> Dual 2x8 0.1-inch pitch breakout headers (H2 and H3) export all micro:bit GPIOs alongside dedicated system power and ground rails[cite: 1].</li>
  <li><strong>Daisy-Chaining Support:</strong> Features a duplicate micro:bit "clone" edge connector at the base of the PCB, allowing you to pass all system lines through to secondary expansion accessories or breadboard breakout adapters[cite: 1].</li>
</ul>

<hr>

<h2>📐 Technical Specifications</h2>

<table width="100%">
  <thead>
    <tr style="background-color: #f4f6f8;">
      <th align="left">Specification Parameter</th>
      <th align="left">Details / Operational Limits</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Operating Voltage</strong></td>
      <td>3.3V DC (Derived directly from the micro:bit host supply bus)[cite: 1].</td>
    </tr>
    <tr>
      <td><strong>Current Consumption</strong></td>
      <td>1 mA to 25 mA dynamic load (dependent on active OLED pixel ratio); &lt;1 µA when OLED is placed into deep sleep mode[cite: 1].</td>
    </tr>
    <tr>
      <td><strong>Display Communication Protocol</strong></td>
      <td>I2C Serial Bus (Hardware Address: <code>0x3C</code>, or <code>0x3D</code> on alternate displays)[cite: 1].</td>
    </tr>
    <tr>
      <td><strong>I2C Speed Architecture</strong></td>
      <td>Supports Standard Mode (100 kbits/s) and Fast Mode (400 kbits/s).</td>
    </tr>
    <tr>
      <td><strong>Operating Temperature Profile</strong></td>
      <td>Industrial grade: -4°F to +185°F (-40°C to +85°C)[cite: 1].</td>
    </tr>
    <tr>
      <td><strong>Mechanical Dimensions</strong></td>
      <td>66.9 mm x 58.9 mm (2.64 in x 2.31 in) inclusive of base edge footprint[cite: 1].</td>
    </tr>
  </tbody>
</table>

<hr>

<h2>🔌 Pin Definitions & Shift Register Serialization</h2>
<p>To avoid consuming half of the available micro:bit GPIO pins just to read the 10 tactile joystick directions, this board integrates a pair of daisy-chained <strong>74LV165A parallel-to-serial shift registers</strong>[cite: 1]. This allows all 10 independent switch states to be packed and streamed into the host controller using only <strong>3 digital pins</strong>[cite: 1].</p>

<h3>Joystick Serial Bus Interface</h3>
<ul>
  <li><strong>Pin 8 (DPAD_LATCH):</strong> Connected to the 74LV165A shift/load (/PL) control line. Software pulls this line LOW to instantly latch the physical button states into internal memory registers, then returns it HIGH to enable bit shifting.</li>
  <li><strong>Pin 9 (DPAD_CLK):</strong> Rising-edge clock signal. Clocking this pin steps through the registers, presenting the next bit in sequence to the data stream.</li>
  <li><strong>Pin 16 (DPAD_DOUT):</strong> The active serial input data line streaming back to the micro:bit. <em>Note: Pin 16 must strictly remain set to INPUT mode in your software toolchain to avoid bus contention.</em></li>
</ul>

<h3>Hardware Bit Map Byte Ordering</h3>
<p>When executing a read sequence, 16 bits are clocked out in the following hardware sequence:</p>
<blockquote>
  <code>(MSB) [ 1, 1, 1, Right_Joy_Right, Right_Joy_Left, Right_Joy_Fire, Right_Joy_Down, Right_Joy_Up | 1, 1, 1, Left_Joy_Right, Left_Joy_Left, Left_Joy_Fire, Left_Joy_Down, Left_Joy_Up ] (LSB)</code>
</blockquote>

<p align="center">
  <img src="YOUR_IMAGE_FOLDER_URL/badge_schematic_crop.jpg" alt="74LV165A Shift Register Schematic Configuration" width="70%">
</p>

<hr>

<h2>🛠️ Software Environment & Toolchains</h2>
<p>The Micro:bit Electronic Badge is fully open and compatible across multiple block-coding and text-based developer sandboxes[cite: 1]:</p>

<h3>1. Microsoft MakeCode (Blocks / Python / JavaScript)</h3>
<p>You can leverage pre-built extensions directly within the MakeCode Extensions store by searching for "OLED" or "SSD1306". The following historical extensions are fully compatible with this board's display line:</p>
<ul>
  <li><strong>XinaBox OD01:</strong> Excellent text configuration API featuring built-in 1X and 2X scaling parameters (<code>OD01.set1_x()</code>, <code>OD01.set2_x()</code>).</li>
  <li><strong>Kitronik OLED Extension:</strong> Robust formatting engine optimized for structural layout printing.</li>
  <li><strong>Tinker PXT:</strong> Lean, lightweight display configuration tool optimized for rapid prototyping blocks.</li>
</ul>

<h3>2. Native MicroPython (python.microbit.org / Thonny)</h3>
<p>For high-performance execution speed, advanced memory layouts, or real-time gaming architectures, native MicroPython is heavily recommended. The repository contains libraries supporting two prominent text drivers:</p>
<ul>
  <li><strong>Core Electronics PiicoDev Driver:</strong> Highly accurate and precise frame buffer controller. Supports literal string prints, float positioning variables, geometric lines, and rectangle fills.</li>
  <li><strong>Fizban99 Driver (Advanced Gaming Engine):</strong> Maximizes your processing capability by dropping the active frame-buffer matrix down to a high-speed 64x32 grid using 2x2 macro-pixels. This cuts display memory space by 75% and boosts screen updates by 400%, allowing fluid retro game loops.</li>
</ul>

<hr>

<h2>📁 Repository Directory Structure & Demos</h2>
<p>This repository provides a comprehensive pipeline of driver libraries and pre-compiled software configurations to get you prototyping immediately:</p>

<ul>
  <li>📁 <code>/Drivers</code>
    <ul>
      <li><code>microbit_MicroPython_Joystick_Driver_01.py</code> - Standardized standalone library to automatically parse, clock, and decode the 74LV165A registers into standard boolean arrays.</li>
      <li><code>PiicoDev_SSD1306.py</code> / <code>PiicoDev_Unified.py</code> - Production framework for the Core Electronics text/graphics engine.</li>
      <li><code>ssd1306.py</code> (including text, stamp, px, and bitmap sub-modules) - Full modular framework suite for the high-performance Fizban99 engine.</li>
    </ul>
  </li>
  <li>📁 <code>/Demos</code>
    <ul>
      <li>🎮 <code>/AstroRun</code> — A fast-paced arcade shooter variant of the classic Asteroids/BitFlyer archetype. Uses the Fizban99 engine with active dual joystick movement, projectile spawning, collision arrays, score tracking, and instant states.</li>
      <li>🕹️ <code>/BreakOut</code> — An implementation of the traditional brick-breaking layout using specialized macro-pixel bounding frameworks.</li>
      <li>🏓 <code>/Pong</code> — A two-player local desktop pong setup powered via the PiicoDev layout tracking system.</li>
      <li>📦 <code>/JoystickDemos</code> — Minimal operational diagnostics patterns to render physical joystick movement tracking values dynamically on the screen.</li>
      <li>🎞️ <code>/AnimationDemo</code> — Demonstrates full-screen 128x64 image processing arrays. Cycles through 1-bit custom dithered frames at uniform refresh rates to produce fluid 3D render loops.</li>
    </ul>
  </li>
  <li>📁 <code>/BitmapImageConverter</code>
    <ul>
      <li>Contains an integrated standalone HTML/JavaScript asset parsing platform. Drop any standard <code>.PNG</code>, <code>.BMP</code>, or <code>.JPG</code> into the app, adjust thresholding limits or dither frameworks (Bayer 4x4 or Floyd-Steinberg), and export a raw packed binary <code>.BIN</code> asset stream ready for <code>show_bitmap()</code> execution.</li>
    </ul>
  </li>
</ul>

<hr>

<h2>🚀 Quick Start Code: Latching & Shifting the Joysticks</h2>
<p>To implement your own bare-metal joystick read loop in native MicroPython without importing libraries, drop this standard routine directly into your tracking loop:</p>

```python
from microbit import pin8, pin9, pin16, sleep

# Establish Hardware Pin Mapping
PIN_LATCH = pin8   # Latch Control Pin (/PL)
PIN_CLK   = pin9   # Shift Clock Signal Pin (CP)
PIN_DATA  = pin16  # Serial In Data Pin (/Q7)

# Pre-set safe system idle configuration states
PIN_LATCH.write_digital(1)
PIN_CLK.write_digital(0)

def read_joystick_hardware():
    # Pulse LATCH low to lock the momentary button states into the registers
    PIN_LATCH.write_digital(0)
    sleep(1)
    PIN_LATCH.write_digital(1)
    
    right_byte = 0
    left_byte  = 0
    
    # Extract the first 8 clock cycles (Right Joystick map data)
    for i in range(8):
        bit = PIN_DATA.read_digital() & 1
        right_byte = (right_byte << 1) | bit
        PIN_CLK.write_digital(1)
        PIN_CLK.write_digital(0)
        
    # Extract the following 8 clock cycles (Left Joystick map data)
    for i in range(8):
        bit = PIN_DATA.read_digital() & 1
        left_byte = (left_byte << 1) | bit
        PIN_CLK.write_digital(1)
        PIN_CLK.write_digital(0)
        
    return left_byte, right_byte
