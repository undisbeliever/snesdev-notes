---
title: Register Cheat Sheet
tags: SnesDev, Registers
nosidebar: True
copyright: © 2019 undisbeliever
license: CC BY-SA 4.0
license-url: https://creativecommons.org/licenses/by-sa/4.0/
---

<table class="register-cheatsheet" markdown>
<colgroup>
 <col class="address" style="width: 8ex;"/>
 <col class="register" style="width: 17ex;"/>
 <col class="timing" style="width: 13ex;"/>
 <col span="8" class="bits" style="width: 11ex;"/>
</colgroup>
<thead>
 <tr>
  <th colspan=2">Register</th>
  <th>Timing</th>
  <th>d7</th><th>d6</th><th>d5</th><th>d4</th><th>d3</th><th>d2</th><th>d1</th><th>d0</th>
 </tr>
</thead>
<tbody>
<tr class="reg">
 <th>$2100</th>
 <th><a href="inidisp">INIDISP</a></th>
 <td>write<br/>any</td>
 <td>Force blank</td><td>-</td><td>-</td><td>-</td><td colspan=4>Brightness</td>
</tr>
<tr class="reg">
 <th>$2101</th>
 <th>OBSEL</th>
 <td>write<br/>f/v blank</td>
 <td colspan=3>Object Size<td colspan=2>Name select</td><td colspan=3>Base address</td>
</tr>
<tr class="reg">
 <th>$2102</th>
 <th>OAMADDL</th>
 <td>write<br/>f/v blank</td>
 <td colspan=8>OAM address</td>
</tr>
<tr class="reg">
 <th>$2103</th>
 <th>OAMADDH</th>
 <td>write<br/>f/v blank</td>
 <td>Sprite priority rotation</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>OAM table</td>
</tr>
<tr class="reg">
 <th>$2104</th>
 <th>OAMDATA</th>
 <td>write<br/>f/v blank</td>
 <td colspan=8 markdown>OAM data write (write twice[^oamdata])</td>
</tr>
<tr class="reg">
 <th rowspan=2>$2105</th>
 <th rowspan=2>BGMODE</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=4>BG character size</td><td rowspan=2>Mode 1 BG3 priority</td><td rowspan=2 colspan=3>BG mode</td>
</tr>
<tr class="reg-bits">
 <td>BG4</td><td>BG3</td><td>BG2</td><td>BG1</td>
</tr>
<tr class="reg">
 <th rowspan=2>$2106</th>
 <th rowspan=2>MOSAIC</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td rowspan=2 colspan=4>Pixel size</td><td colspan=4>Enable</td>
</tr>
<tr class="reg-bits">
 <td>BG4</td><td>BG3</td><td>BG2</td><td>BG1</td>
</tr>
<tr class="reg">
 <th>$2107</th>
 <th>BG1SC</th>
 <td>write<br/>f/v blank</td>
 <td colspan=6>BG1 tilemap VRAM address</td><td colspan=2>BG1 map size</td>
</tr>
<tr class="reg">
 <th>$2108</th>
 <th>BG2SC</th>
 <td>write<br/>f/v blank</td>
 <td colspan=6>BG2 tilemap VRAM address</td><td colspan=2>BG2 map size</td>
</tr>
<tr class="reg">
 <th>$2109</th>
 <th>BG3SC</th>
 <td>write<br/>f/v blank</td>
 <td colspan=6>BG3 tilemap VRAM address</td><td colspan=2>BG3 map size</td>
</tr>
<tr class="reg">
 <th>$210a</th>
 <th>BG4SC</th>
 <td>write<br/>f/v blank</td>
 <td colspan=6>BG4 tilemap VRAM address</td><td colspan=2>BG4 map size</td>
</tr>
<tr class="reg">
 <th>$210b</th>
 <th>BG12NBA</th>
 <td>write<br/>f/v blank</td>
 <td colspan=4>BG2 character VRAM address</td><td colspan=4>BG1 character VRAM address</td>
</tr>
<tr class="reg">
 <th>$210c</th>
 <th>BG34NBA</th>
 <td>write<br/>f/v blank</td>
 <td colspan=4>BG4 character VRAM address</td><td colspan=4>BG3 character VRAM address</td>
</tr>
<tr class="reg">
 <th>$210d</th>
 <th>BG1HOFS<br/>M7HOFS</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>BG1 Horizontal Offset (10 bits (13 signed bits in Mode 7), write twice)</td>
</tr>
<tr class="reg">
 <th>$210e</th>
 <th>BG1VOFS<br/>M7VOFS</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>BG1 Vertical Offset (10 bits (13 signed bits in Mode 7), write twice)</td>
</tr>
<tr class="reg">
 <th>$210f</th>
 <th>BG2HOFS</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>BG2 Horizontal Offset (10 bits, write twice)</td>
</tr>
<tr class="reg">
 <th>$2110</th>
 <th>BG2VOFS</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>BG2 Vertical Offset (10 bits, write twice)</td>
</tr>
<tr class="reg">
 <th>$2111</th>
 <th>BG3HOFS</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>BG3 Horizontal Offset (10 bits, write twice)</td>
</tr>
<tr class="reg">
 <th>$2112</th>
 <th>BG3VOFS</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>BG3 Vertical Offset (10 bits, write twice)</td>
</tr>
<tr class="reg">
 <th>$2113</th>
 <th>BG4HOFS</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>BG4 Horizontal Offset (10 bits, write twice)</td>
</tr>
<tr class="reg">
 <th>$2114</th>
 <th>BG4VOFS</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>BG4 Vertical Offset (10 bits, write twice)</td>
</tr>
<tr class="reg">
 <th>$2115</th>
 <th>VMAIN</th>
 <td>write<br/>f/v blank</td>
 <td>Increment register</td><td>-</td><td>-</td><td>-</td><td colspan=2>Address increment</td><td colspan=2>Address remapping</td>
</tr>
<tr class="reg">
 <th>$2116</th>
 <th>VMADDL</th>
 <td>write<br/>f/v blank</td>
 <td rowspan=2 colspan=8>VRAM word address</td>
</tr>
<tr class="reg">
 <th>$2117</th>
 <th>VMADDH</th>
 <td>write<br/>f/v blank</td>
</tr>
<tr class="reg">
 <th>$2118</th>
 <th>VMDATAL</th>
 <td>write<br/>f/v blank</td>
 <td colspan=8>VRAM data write low byte</td>
</tr>
<tr class="reg">
 <th>$2119</th>
 <th>VMDATAH</th>
 <td>write<br/>f/v blank</td>
 <td colspan=8>VRAM data write high byte</td>
</tr>
<tr class="reg">
 <th rowspan=2>$211a</th>
 <th rowspan=2>M7SEL</th>
 <td rowspan=2>write<br/>f/v blank</td>
 <td colspan=8>Mode 7 settings</td>
</tr>
<tr class="reg-bits">
 <td>Out of field fill</td><td>Fill type</td><td>-</td><td>-</td><td>-</td><td>-</td><td>V flip</td><td>H flip</td>
</tr>
<tr class="reg">
 <th>$211b</th>
 <th>M7A</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Mode 7 matrix A (1:7:8 signed fixed point, write twice)</td>
</tr>
<tr class="reg">
 <th>$211c</th>
 <th>M7B</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Mode 7 matrix B (1:7:8 signed fixed point, write twice)</td>
</tr>
<tr class="reg">
 <th>$211d</th>
 <th>M7C</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Mode 7 matrix C (1:7:8 signed fixed point, write twice)</td>
</tr>
<tr class="reg">
 <th>$211e</th>
 <th>M7D</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Mode 7 matrix D (1:7:8 signed fixed point, write twice)</td>
</tr>
<tr class="reg">
 <th>$211f</th>
 <th>M7X</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Mode 7 centre X (13 signed bits, write twice)</td>
</tr>
<tr class="reg">
 <th>$2120</th>
 <th>M7Y</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Mode 7 centre Y (13 signed bits, write twice)</td>
</tr>
<tr class="reg">
 <th>$2121</th>
 <th>CGADD</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>CGRAM word address</td>
</tr>
<tr class="reg">
 <th>$2122</th>
 <th>CGDATA</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>CGRAM data write (write twice)</td>
</tr>
<tr class="reg">
 <th rowspan=2>$2123</th>
 <th rowspan=2>W12SEL</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=2>BG2 window 2</td><td colspan=2>BG2 window 1</td><td colspan=2>BG1 window 2</td><td colspan=2>BG1 window 1</td>
</tr>
<tr class="reg-bits">
 <td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td>
</tr>
<tr class="reg">
 <th rowspan=2>$2124</th>
 <th rowspan=2>W34SEL</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=2>BG4 window 2</td><td colspan=2>BG4 window 1</td><td colspan=2>BG3 window 2</td><td colspan=2>BG3 window 1</td>
</tr>
<tr class="reg-bits">
 <td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td>
</tr>
<tr class="reg">
 <th rowspan=2>$2125</th>
 <th rowspan=2>WOBJSEL</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=2>Colour window 2</td><td colspan=2>Colour window 1</td><td colspan=2>OBJ window 2</td><td colspan=2>OBJ window 1</td>
</tr>
<tr class="reg-bits">
 <td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td><td>Enable</td><td>Invert</td>
</tr>
<tr class="reg">
 <th>$2126</th>
 <th>WH0</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Window 1 left position</td>
</tr>
<tr class="reg">
 <th>$2127</th>
 <th>WH1</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Window 1 right position</td>
</tr>
<tr class="reg">
 <th>$2128</th>
 <th>WH2</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Window 2 left position</td>
</tr>
<tr class="reg">
 <th>$2129</th>
 <th>WH3</th>
 <td>write<br/>f/v/h blank</td>
 <td colspan=8>Window 2 right position</td>
</tr>
<tr class="reg">
 <th rowspan=2>$212a</th>
 <th rowspan=2>WBGLOG</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=8>Window mask logic</td>
<tr class="reg-bits">
 <td colspan=2>BG4 window</td><td colspan=2>BG3 window</td><td colspan=2>BG2 window</td><td colspan=2>BG1 window</td>
</tr>
<tr class="reg">
 <th rowspan=2>$212b</th>
 <th rowspan=2>WOBJLOG</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=8>Window mask logic</td>
<tr class="reg-bits">
 <td colspan=2>-</td><td colspan=2>-</td><td colspan=2>Colour window</td><td colspan=2>OBJ window</td>
</tr>
<tr class="reg">
 <th rowspan=2>$212c</th>
 <th rowspan=2>TM</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=8>Main screen designation</td>
<tr class="reg-bits">
 <td>-</td><td>-</td><td>-</td><td>OBJ</td><td>BG4</td><td>BG3</td><td>BG2</td><td>BG1</td>
</tr>
<tr class="reg">
 <th rowspan=2>$212d</th>
 <th rowspan=2>TS</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=8>Sub screen designation</td>
<tr class="reg-bits">
 <td>-</td><td>-</td><td>-</td><td>OBJ</td><td>BG4</td><td>BG3</td><td>BG2</td><td>BG1</td>
</tr>
<tr class="reg">
 <th rowspan=2>$212e</th>
 <th rowspan=2>TMW</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=8>Window Mask designation for the main screen</td>
<tr class="reg-bits">
 <td>-</td><td>-</td><td>-</td><td>OBJ</td><td>BG4</td><td>BG3</td><td>BG2</td><td>BG1</td>
</tr>
<tr class="reg">
 <th rowspan=2>$212f</th>
 <th rowspan=2>TSW</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=8>Window Mask designation for the sub screen</td>
<tr class="reg-bits">
 <td>-</td><td>-</td><td>-</td><td>OBJ</td><td>BG4</td><td>BG3</td><td>BG2</td><td>BG1</td>
</tr>
<tr class="reg">
 <th rowspan=2>$2130</th>
 <th rowspan=2>CGWSEL</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=8>Colour addition select</td>
<tr class="reg-bits">
 <td colspan=2>Clip colours to black</td><td colspan=2>Prevent colour math</td><td>-</td><td>-</td><td>Add sub screen</td><td>Direct colour mode</td>
</tr>
<tr class="reg">
 <th rowspan=2>$2131</th>
 <th rowspan=2>CGADSUB</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td rowspan=2>Add/<wbr/>Subtract</td><td rowspan=2>Half colour math</td><td colspan=6>Enable color math</td>
</tr>
<tr class="reg-bits">
 <td>Backdrop</td><td>OBJ</td><td>BG4</td><td>BG3</td><td>BG2</td><td>BG1</td>
</tr>
<tr class="reg">
 <th rowspan=2>$2132</th>
 <th rowspan=2>COLDATA</th>
 <td rowspan=2>write<br/>f/v/h blank</td>
 <td colspan=3>Color plane(s)</td><td rowspan=2 colspan=5>Intensity</td>
</tr>
<tr class="reg-bits">
 <td>Blue</td><td>Green</td><td>Red</td>
</tr>
<tr class="reg">
 <th>$2133</th>
 <th>SETINI</th>
 <td>write<br/>f/v/h blank</td>
 <td>External sync</td><td>Mode 7 EXTBG</td><td>-</td><td>-</td><td>Psuedo hires</td><td>Overscan mode</td><td>OBJ interlace</td><td>Screen interlace</td>
</tr>
<tr class="reg">
 <th>$2134</th>
 <th>MPYL</th>
 <td>read<br/>not mode 7</td>
 <td rowspan=3 colspan=8>Multiplication result<br/>{signed 16 bit value of `M7A`} * {signed 8 bit of `M7B`}</td>
</tr>
<tr class="reg">
 <th>$2135</th>
 <th>MPYM</th>
 <td>read<br/>not mode 7</td>
</tr>
<tr class="reg">
 <th>$2136</th>
 <th>MPYH</th>
 <td>read<br/>not mode 7</td>
</tr>
<tr class="reg">
 <th>$2137</th>
 <th>SLHV</th>
 <td>read<br/>any</td>
 <td colspan=8>Software latch for H/V counter</td>
</tr>
<tr class="reg">
 <th>$2138</th>
 <th>OAMDATAREAD</th>
 <td>read<br/>f/v blank</td>
 <td colspan=8>OAM data read</td>
</tr>
<tr class="reg">
 <th>$2139</th>
 <th>VMDATALREAD</th>
 <td>read<br/>f/v blank</td>
 <td colspan=8>VRAM data read low byte</td>
</tr>
<tr class="reg">
 <th>$213a</th>
 <th>VMDATAHREAD</th>
 <td>read<br/>f/v blank</td>
 <td colspan=8>VRAM data read high byte</td>
</tr>
<tr class="reg">
 <th>$213b</th>
 <th>CGDATAREAD</th>
 <td>read<br/>f/v/h blank</td>
 <td colspan=8>CGRAM data read (read twice)</td>
</tr>
<tr class="reg">
 <th>$213c</th>
 <th>OPHCT</th>
 <td>read<br/>any</td>
 <td colspan=8>Horizontal scanline location (9 bits, read twice)</td>
</tr>
<tr class="reg">
 <th>$213d</th>
 <th>OPVCT</th>
 <td>read<br/>any</td>
 <td colspan=8>Vertical scanline location (9 bits, read twice)</td>
</tr>
<tr class="reg">
 <th>$213e</th>
 <th>STAT77</th>
 <td>read<br/>any</td>
 <td>OBJ time over flag</td><td>OBJ range over flag</td><td>Master/<wbr/>Slave flag</td><td>-</td><td colspan=4>5C77 chip version</td>
</tr>
<tr class="reg">
 <th>$213f</th>
 <th>STAT78</th>
 <td>read<br/>any</td>
 <td>Interface field</td><td>External latch flag</td><td>-</td><td>NTSC/<wbr/>PAL</td><td colspan=4>5C78 chip version</td>
</tr>
<tr class="reg">
 <th>$2140</th>
 <th>APUIO0</th>
 <td>read/write<br/>any</td>
 <td colspan=8>APU I/O register 0</td>
</tr>
<tr class="reg">
 <th>$2141</th>
 <th>APUIO1</th>
 <td>read/write<br/>any</td>
 <td colspan=8>APU I/O register 1</td>
</tr>
<tr class="reg">
 <th>$2142</th>
 <th>APUIO2</th>
 <td>read/write<br/>any</td>
 <td colspan=8>APU I/O register 2</td>
</tr>
<tr class="reg">
 <th>$2143</th>
 <th>APUIO3</th>
 <td>read/write<br/>any</td>
 <td colspan=8>APU I/O register 3</td>
</tr>
<tr class="reg">
 <th>$2180</th>
 <th>WMDATA</th>
 <td>read/write<br/>any</td>
 <td colspan=8>WRAM data read/write</td>
</tr>
<tr class="reg">
 <th>$2181</th>
 <th>WMADDL</th>
 <td>write<br/>any</td>
 <td rowspan=3 colspan=8>WRAM address (17 bit)</td>
</tr>
<tr class="reg">
 <th>$2182</th>
 <th>WMADDM</th>
 <td>write<br/>any</td>
</tr>
<tr class="reg">
 <th>$2183</th>
 <th>WMADDH</th>
 <td>write<br/>any</td>
</tr>
<tr class="reg">
 <th rowspan=2>$4016</th>
 <th>JOYSER0<br/>(write)</th>
 <td>write<br/>not auto&#8209;joy</td>
 <td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>Latch Line</td>
</tr>
<tr class="reg">
 <th>JOYSER0<br/>(read)</th>
 <td>read<br/>not auto&#8209;joy</td>
 <td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>Port 1<br/>Data 2</td><td>Port 1<br/>Data 1</td>
</tr>
<tr class="reg">
 <th>$4017</th>
 <th>JOYSER1</th>
 <td>read<br/>not auto&#8209;joy</td>
 <td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>Port 2<br/>Data 2</td><td>Port 2<br/>Data 1</td>
</tr>
<tr class="reg">
 <th rowspan=2>$4200</th>
 <th rowspan=2>NMITIMEN</th>
 <td rowspan=2>write<br/>any</td>
 <td rowspan=2>Enable NMI</td><td rowspan=2>-</td><td colspan=2>Enable IRQ</td><td rowspan=2>-</td><td rowspan=2>-</td><td rowspan=2>-</td><td rowspan=2>Enable auto&#8209;joy</td>
</tr>
<tr class="reg-bits">
 <td>Vertical</td><td>Horizontal</td>
</tr>
<tr class="reg">
 <th>$4201</th>
 <th>WRIO</th>
 <td>write<br/>any</td>
 <td>Port 2<br/>Pin 6</td><td>Port 1<br/>Pin 6</td><td>x</td><td>x</td><td>x</td><td>x</td><td>x</td><td>x</td>
</tr>
<tr class="reg">
 <th>$4202</th>
 <th>WRMPYA</th>
 <td>write<br/>any</td>
 <td colspan=8>Multiplicand A</td>
</tr>
<tr class="reg">
 <th>$4203</th>
 <th>WRMPYB</th>
 <td>write<br/>any</td>
 <td colspan=8>Multiplicand B</td>
</tr>
<tr class="reg">
 <th>$4204</th>
 <th>WRDIVL</th>
 <td>write<br/>any</td>
 <td rowspan=2 colspan=8>Dividend C</td>
</tr>
<tr class="reg">
 <th>$4205</th>
 <th>WRDIVH</th>
 <td>write<br/>any</td>
</tr>
<tr class="reg">
 <th>$4206</th>
 <th>WRDIVB</th>
 <td>write<br/>any</td>
 <td colspan=8>Divisor B</td>
</tr>
<tr class="reg">
 <th>$4207</th>
 <th>HTIMEL</th>
 <td>write<br/>any</td>
 <td rowspan=2 colspan=8>H-count timer (9 bits)</td>
</tr>
<tr class="reg">
 <th>$4208</th>
 <th>HTIMEH</th>
 <td>write<br/>any</td>
</tr>
<tr class="reg">
 <th>$4209</th>
 <th>VTIMEL</th>
 <td>write<br/>any</td>
 <td rowspan=2 colspan=8>V-count timer (9 bits)</td>
</tr>
<tr class="reg">
 <th>$420a</th>
 <th>VTIMEH</th>
 <td>write<br/>any</td>
</tr>
<tr class="reg">
 <th rowspan=2>$420b</th>
 <th rowspan=2>MDMAEN</th>
 <td rowspan=2>write<br/>any</td>
 <td colspan=8>Enable DMA channel</td>
</tr>
<tr class="reg-bits">
 <td>Ch 7</td><td>Ch 6</td><td>Ch 5</td><td>Ch 4</td><td>Ch 3</td><td>Ch 2</td><td>Ch 1</td><td>Ch 0</td>
</tr>
<tr class="reg">
 <th rowspan=2>$420c</th>
 <th rowspan=2>HDMAEN</th>
 <td rowspan=2>write<br/>any</td>
 <td colspan=8>Enable HDMA channel</td>
</tr>
<tr class="reg-bits">
 <td>Ch 7</td><td>Ch 6</td><td>Ch 5</td><td>Ch 4</td><td>Ch 3</td><td>Ch 2</td><td>Ch 1</td><td>Ch 0</td>
</tr>
<tr class="reg">
 <th>$420d</th>
 <th>MSEL</th>
 <td>write<br/>any</td>
 <td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>FastROM enable</td>
</tr>
<tr class="reg">
 <th>$4210</th>
 <th>RDNMI</th>
 <td>read<br/>any</td>
 <td>NMI flag</td><td>-</td><td>-</td><td>-</td><td colspan=4>5A22 chip version</td>
</tr>
<tr class="reg">
 <th>$4211</th>
 <th>TIMEUP</th>
 <td>read<br/>any</td>
 <td>IRQ flag</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td>
</tr>
<tr class="reg">
 <th>$4212</th>
 <th>HVBJOY</th>
 <td>read<br/>any</td>
 <td>V-blank flag</td><td>H-blank flag</td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td><td>Auto-joy flag</td>
</tr>
<tr class="reg">
 <th>$4213</th>
 <th>RDIO</th>
 <td>read<br/>any</td>
 <td>Port 2<br/>Pin 6</td><td>Port 1<br/>Pin 6</td><td>x</td><td>x</td><td>x</td><td>x</td><td>x</td><td>x</td>
</tr>
<tr class="reg">
 <th>$4214</th>
 <th>RDDIVL</th>
 <td>read<br/>any</td>
 <td rowspan=2 colspan=8>Quotient of divide result<br/>Unsigned 16 bit result of `{WRDIVH WRDIVL} / {WRDIVB}`. (16 CPU cycles after writing to `WRDIVB`)</td>
</tr>
<tr class="reg">
 <th>$4215</th>
 <th>RDDIVH</th>
 <td>read<br/>any</td>
</tr>
<tr class="reg">
 <th>$4216</th>
 <th>RDMPYL</th>
 <td>read<br/>any</td>
 <td rowspan=2 colspan=8>
  <div>Divide remainder<br/>Unsigned 16 bit result of `{WRDIVH WRDIVL} % {WRDIVB}`. (16 CPU cycles after writing to `WRDIVB`)</div>
  <strong>-OR-</strong>
  <div>Multiplication product<br/>Unsigned 16 bit result of `{WRMPYA} * {WRMPYB}` (8 CPU cycles after writing to `WRMPYB`)</div>
 </td>
</tr>
<tr class="reg">
 <th>$4217</th>
 <th>RDMPYH</th>
 <td>read<br/>any</td>
</tr>
<tr class="reg">
 <th>$4218</th>
 <th>JOY1L</th>
 <td>read<br/>not auto&#8209;joy</td>
 <td rowspan=2 colspan=8>Joypad 1 data</td>
</tr>
<tr class="reg">
 <th>$4219</th>
 <th>JOY1H</th>
 <td>read<br/>not auto&#8209;joy</td>
</tr>
<tr class="reg">
 <th>$421a</th>
 <th>JOY2L</th>
 <td>read<br/>not auto&#8209;joy</td>
 <td rowspan=2 colspan=8>Joypad 2 data</td>
</tr>
<tr class="reg">
 <th>$422b</th>
 <th>JOY2H</th>
 <td>read<br/>not auto&#8209;joy</td>
</tr>
<tr class="reg">
 <th>$421c</th>
 <th>JOY3L</th>
 <td>read<br/>not auto&#8209;joy</td>
 <td rowspan=2 colspan=8>Joypad 3 data</td>
</tr>
<tr class="reg">
 <th>$421d</th>
 <th>JOY3H</th>
 <td>read<br/>not auto&#8209;joy</td>
</tr>
<tr class="reg">
 <th>$421e</th>
 <th>JOY4L</th>
 <td>read<br/>not auto&#8209;joy</td>
 <td rowspan=2 colspan=8>Joypad 4 data</td>
</tr>
<tr class="reg">
 <th>$421f</th>
 <th>JOY4H</th>
 <td>read<br/>not auto&#8209;joy</td>
</tr>
<tr class="reg">
 <th>$43x0</th>
 <th>DMAPx</th>
 <td>read/write<br/>any</td>
 <td>Transfer direction</td><td>HDMA addressing mode</td><td>-</td><td>DMA address increment</td><td>Fixed transfer</td><td colspan=3>Transfer mode</td>
</tr>
<tr class="reg">
 <th>$43x1</th>
 <th>BBADx</th>
 <td>read/write<br/>any</td>
 <td colspan=8>DMA bus B address</td>
</tr>
<tr class="reg">
 <th>$43x2</th>
 <th>A1TxL</th>
 <td>read/write<br/>any</td>
 <td rowspan=2 colspan=8>DMA bus A address</td>
</tr>
<tr class="reg">
 <th>$43x3</th>
 <th>A1TxH</th>
 <td>read/write<br/>any</td>
</tr>
<tr class="reg">
 <th>$43x4</th>
 <th>A1Bx</th>
 <td>read/write<br/>any</td>
 <td colspan=8>DMA bus A bank</td>
</tr>
<tr class="reg">
 <th>$43x5</th>
 <th>DASxL</th>
 <td>read/write<br/>any</td>
 <td rowspan=2 colspan=8>
  <div>DMA size</div>
  <strong>-OR-</strong>
  <div>HDMA indirect address</div>
</tr>
<tr class="reg">
 <th>$43x6</th>
 <th>DASxH</th>
 <td>read/write<br/>any</td>
</tr>
<tr class="reg">
 <th>$43x7</th>
 <th>DASBx</th>
 <td>read/write<br/>any</td>
 <td colspan=8>HDMA indirect address bank</td>
</tr>
<tr class="reg">
 <th>$43x8</th>
 <th>A2AxL</th>
 <td>read/write<br/>any</td>
 <td rowspan=2 colspan=8>HDMA table address</td>
</tr>
<tr class="reg">
 <th>$43x9</th>
 <th>A2AxH</th>
 <td>read/write<br/>any</td>
</tr>
<tr class="reg">
 <th>$43xa</th>
 <th>NLTRx</th>
 <td>read/write<br/>any</td>
 <td>Repeat select</td><td colspan=7>Line count</td>
</tr>
</tbody>
</table>

[^oamdata]: OAMDATA register is write twice if OAMDATA writes to the low table, write once if OAMDATA writes to the high table.


Sources
=======
 * Anomie's Register Doc
 * [higan](https://higan.dev/) source code,
   [sfc/ppu/io.cpp](https://github.com/higan-emu/higan/blob/master/higan/sfc/ppu/io.cpp) and
   [sfc/cpu/io.cpp](https://github.com/higan-emu/higan/blob/master/higan/sfc/cpu/io.cpp),
   by [Near](https://near.sh/)

