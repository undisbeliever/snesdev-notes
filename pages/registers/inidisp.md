---
title: INIDISP Register
tags: SnesDev, Registers
---

<!-- ::TODO Add timing page (and link VBlank to it) :: -->


INIDISP
=======

<aside class="register"><table>
 <tr><th><tt>$2100</tt></th><th>INIDISP</th></tr>
 <tr><td align="center" colspan="2">Screen Display Register</td></tr>
 <tr><th>Size:</th><td>byte</th></tr>
 <tr><th>Access:</th><td>write</th></tr>
 <tr><th>Timing:</th><td>any</th></tr>
 <tr><th>Chips:</th><td>S-PPU1 and S-PPU2</th></tr>
</table></aside>


<table class="register">
 <thead><tr>
  <td colspan=2></td><th>d7</th><th>d6</th><th>d5</th><th>d4</th><th>d3</th><th>d2</th><th>d1</th><th>d0</th>
 </tr></thead>
 <tbody><tr class="reg">
  <th>$2100</th><th>INIDISP</th>
  <td>force blank</td><td>-</td><td>-</td><td>-</td><td colspan=4>brightness</td>
 </tr>
</tbody></table>


Brightness
----------

The *brightness* bits control the intensity of the video output.
Brightness 15 (`b1111`) will output full intensity and brightness 0
(`b0000`) will output a very weak (but not black) signal.
[^brightness-measurements]

Internally the *bightness* bits are connected to a 4 bit
Digital-to-Analog converter inside S-PPU2, whose output is used as
reference voltage for the Blue-Green-Red DACs.  [^brightness-dac]

<figure>
 <img src="possible-dac-layout.svg" alt="Possible INIDISP DAC Layout"/>
 <figcaption>Possible DAC Layout for the INIDISP Register</figcaption>
</figure>

[^brightness-measurements]: [Brightness DAC measurements by lidnariq](https://forums.nesdev.org/viewtopic.php?p=257831#p257831)
[^brightness-dac]: [SNES-Chips decapped (2PPU, 1CHIP, APU, DSP) - circuit-board.de forum](https://circuit-board.de/forum/index.php/Thread/25396-SNES-Chips-decapped-2PPU-1CHIP-APU-DSP/)


Force Blank
-----------

When the *force-blank* bit is set, the PPU will disable the screen
output and allow the CPU to safely access the PPU registers outside the
blanking periods.

Force-blank is normally used by CPU code to initialize the PPU registers
and transfer large amounts of data to the VRAM/CGRAM/OAM at the
beginning of a scene or level.

Enabling *force-blank* does not stop PPU execution, the PPU will
continue to process the display frame.  It is theorized that
*force-blank* prevents the PPU from latching
PPU-registers/VRAM/CGRAM/OAM and thus process junk data.  This junk data
will persist until the PPU's tile and object buffers are overridden with
good data.


Failing to enable *force-blank* before accessing the PPU registers
outside their timing window can cause
[bus contention](https://en.wikipedia.org/wiki/Bus_contention) inside
the PPU which can result in ignored reads/writes, data corruption and/or
address bus corruption.


 * VRAM access outside of Vertical-Blank or Force-Blank is silently
   ignored.  The VRAM address is incremented as per normal but no data
   is read or written.

 * CGRAM access outside of Horizontal-Blank, Vertical-Blank or
   Force-Blank will read or write to the wrong CGRAM address.

 * OAM access outside of Vertical-Blank or Force-Blank will read or
   write to the wrong OAM address.


<figure class="fixed-width" markdown="span">
 <img src="forgot-to-force-blank.jpeg" alt="A screen capture of grabage tiles" />
 <figcaption markdown="1">
  Garbage tiles when transferring a large amount of data to VRAM without enabling *force-blank*.
  <br/>The VRAM writes were ignored by the PPU and console is outputting stale data.
 </figcaption>
</figure>

<figure class="fixed-width" markdown="span">
 <img src="forgot-to-force-blank-2.png" alt="Four screen captures of corrupted CGRAM and OAM" />
 <figcaption markdown="1">
  Corrupted CGRAM and OAM when transferring data outside of the blanking periods.
  <br/>The corruption is inconsistent, all four screen captures are executing the same binary.
  </figcaption>
</figure>


<aside markdown="1">
What exactly happens to invalid reads or writes to OAM is currently unknown.

Most emulators do not emulate this behaviour, it is the largest difference
between emulators and real hardware.
</aside>


<aside markdown="1">
Writing to the PPU outside the appropriate blanking period is a common
mistake newcomers to SNESdev make.

If a binary shows vastly different outputs between emulators and real
hardware, do a quick check to confirm there are no PPU reads or writes
in the middle of a scanline.
</aside>



Recommend Usage
===============

When to write to INIDISP
------------------------

The best time to write to `INIDISP` is during the Vertical Blanking
period.  This prevents graphical glitches when *force-blank* is cleared
and screen-tearing when changing the *brightness* bits.

When enabling *force-blank*, the *brightness* bits should also be set to
mitigate the [brightness delay glitch](#brightness-delay-glitch) on
1-CHIP consoles.


It is a good idea to create a shadow variable for the `INIDISP`
register.  A shadow variable is a RAM variable that can be read or
written at any time and will be transferred to `INIDISP` by the VBlank
routine.

Shadow variables have three advantages over directly writing to the
`INIDISP` register:

 1. The `INIDISP` register will be written inside the Vertical Blanking
    period.
 2. The game code can read the value of the shadow variable.
 3. The game code can write to the shadow variable at any time.

One important note about enabling *force-blank* via an `INIDISP` shadow
variable:  You will need to set the shadow variable and then **wait
until the end of the VBlank routine** before transferring data to or
from the PPU.


### Writing to INIDISP outside of Vertical Blank {#writing-outside-vblank}

Writes to `INIDISP` outside of the Vertical Blanking period should be
handled with care to avoid glitches and hardware bugs.

All `INIDISP` writes using HDMA should be preformed using the *2
registers write once* transfer mode (`DMAPx` = `0x01`) to B-Bus address
`$21ff` (`BBADx` = `0xff`)[^hdma-to-21ff].  Both bytes in the HDMA
table must contain the same value.  This mitigates both the [early read
glitch](#early-read-glitch) and the [DMA failure hardware bug](#dma-failure-hw-bug).

All `INIDISP` writes by a CPU instruction outside of the Vertical
Blanking period should be preformed with long addressing, using address
`$802100` if the *force-blank* bit is set or address `$0f2100` if the
*force-blank* bit is clear or unknown  (to mitigate the [early read
glitch](#early-read-glitch)).

Any `INIDISP` write that clears the *force-blank* bit outside of
the Vertical Blanking period will cause [graphical glitches](#mid-frame-activation-glitch).



Fade-in/Fade-out screen transitions {#screen-transition}
-----------------------------------

The `INIDISP` register can be used to create a simple and inexpensive
fade-to-black and fade-in screen transitions.

A fade-in transition is preformed by slowly incrementing the
*brightness* bits over multiple frames.

A fade-to-black transition is preformed by slowly decrementing the
*brightness* bits over multiple frames.


<figure markdown="span">
 <img src="fadeout.png" alt="16 frames of a fade-to-black screen transition" />
 <figcaption markdown="1">
  16 frames of a fade-to-black screen transition using the `INIDISP` register.
 </figcaption>
</figure>



Minimizing glitched V-Blank overruns
------------------------------------

A V-Blank overrun occurs when the V-Blank routine takes too long and
does not finish before the end of the Vertical Blank period.  This
usually results in the V-Blank routine writing to the PPU during active
display, leading to glitches that can persist for multiple frames.

<!-- ::TODO add link to HVBJOY register:: -->
V-Blank overruns can be detected by reading the *VBlank flag* of the
`HVBJOY` register at the end of the V-Blank routine.

A simple way to minimize a glitched V-Blank overrun is to enable
*force-blank* at the start of the V-Blank routine and clear
*force-blank* at the end of the V-Blank routine.

If a V-Blank overrun occurs and *force-blank* is enabled, the PPU writes
will still be valid and no long-term glitches will occur.  Unfortunately,
this introduces three new glitches when V-Blank overruns; a small black
bar at the top of the display and a [mid frame screen activation
glitch](#mid-frame-activation-glitch), both lasting for a single frame.
Finally, a HDMA glitch can occur if the HDMA register setup was not
completed before the Vertical-Blanking period ends.

V-Blank overruns should rarely occur.  Any V-Blank routine that
regularly overruns should be redesigned and/or rewritten.



Glitches and Hardware Bugs
==========================

Mid frame screen activation {#mid-frame-activation-glitch}
---------------------------
Turning on the screen (clearing the *force-blank* bit) mid frame will
create graphical glitches.

Activating the display during Horizontal Blank can cause sprite glitches
on the next scanline.

<figure class="fixed-width" markdown="span">
  <img src="enable-display-hblank.jpeg" alt="enable display during h-blank screen capture (cropped)" />
  <figcaption markdown="1">Object tile corruption (green) and single scanline with missing object tiles when clearing *force-blank* during Horizontal Blank.  

Recorded on a 1-CHIP SFC console [(raw screen capture)](enable-display-hblank-raw.jpeg).  3-chip consoles show similar output.</figcaption>
</figure>


Activating the display in the middle of a scanline can cause tile
glitches for the current scanline and sprite glitches for the next two
scanlines.

<figure class="fixed-width" markdown="span">
  <img src="enable-display-mid-scanline.jpeg" alt="enable display mid-scanline screen capture (cropped)" />
  <figcaption markdown="1">BG tile corruption (red), object tile corruption (green) and two scanlines containing missing object tiles when clearing *force-blank* outside Horizontal and Vertical Blanking period.  
Recorded on a 1-CHIP SFC console [(raw screen capture)](enable-display-mid-scanline-raw.jpeg).  3-chip consoles show similar output.</figcaption>
</figure>



Brightness Delay {#brightness-delay-glitch}
----------------
The brightness DAC in 1-CHIP consoles has a surprisingly large [rise
time](https://en.wikipedia.org/wiki/Rise_time), exhibiting as a glitched
brightness gradient that can last multiple pixels.  There are reports of
this glitch lasting multiple scanlines on modded 1-CHIP consoles with
the C11 capacitor anti-ghosting mod.


<figure markdown="span">
 <img src="brightness-delay.png" alt="3 Chip SFC console: ~2.5 dot fall time.  1 Chip SFC console: ~72 dot fall time.  C11 anti-ghosting modded 1 Chip console: ~332 dot fall time." />
 <figcaption markdown="1">
   *Brightness* DAC rise time measurements.
 </figcaption>
</figure>


Most instances of this glitch occur when the value `0x80` (*force-blank*
set, 0 *brightness*) is written to `INIDISP`, followed by a `0x0f`
(*force-blank* clear, full *brightness*) some time later.

```
// 8 bit A
// DB access PPU registers

    // Disable screen (0 brightness)
    lda     #0x80
    sta     INIDISP

    // DMA transfer to VRAM/CGRAM/OAM
    // Update PPU registers

    // Enable screen (full brightness)
    lda     #0x0f
    sta     INIDISP
```


The fix for this glitch is to set the brightness bits whenever
*force-blank* is enabled.  There will still be a transition delay if the
brightness changes, but it now occurs during force-blank and should be
invisible to the player.

```
// 8 bit A
// DB access PPU registers

    // Disable screen (and set brightness bits)
    lda     inidisp_shadow
    ora     #0x80
    sta     INIDISP

    // DMA transfer to VRAM/CGRAM/OAM
    // Update PPU registers

    // Enable screen
    lda     inidisp_shadow
    sta     INIDISP
```


This glitch affects multiple retail games, usually games that
[extended VBlank](#extended-vblank) or change the screen brightness
mid-screen.

<aside markdown="1">
FXPAK firmware v1.8.0 includes a patch[^sd2snes-brightness-patch]
(labelled "1CHIP transient fixes" in the *In-game Settings* menu) that
alleviates this glitch by detecting `INIDISP` writes and overriding the
*brightness* bits if *force-blank* is set.
</aside>


[^sd2snes-brightness-patch]: [sd2snes git commit - FPGA: experimental brightness patching for 1CHIP systems](https://github.com/mrehkopf/sd2snes/commit/a8cd32af3d360b1c91ea1222321734fbe240f573), <br/> [Firmware v1.8.0 released](https://sd2snes.de/blog/archives/980)

<!-- ::TODO write a testing checklist (include test on a 1CHIP console with "1CHIP transient fixes" Off) -->



INIDISP Early Read Glitch {#early-read-glitch}
-------------------------

In 2021, it was discovered the PPU erroneously latches the data-bus too
early during a write to `INIDISP`.  This can causes the PPU to use the
previous value on the data-bus for the *brightness* and *force-blank*
bits for about a single dot. [^early-read-nesdev-forums]
[^early-read-ikari]


This hardware bug can result in the following glitches:


  * Corrupted object tile data on 3-chip consoles when a write to
    `INIDISP` occurs outside of Vertical-Blank and the previous value on
    the data-bus has bit 7 (MSB) set.

    A HDMA write to `INIDISP` can easily exhibit this glitch.

    It is unknown why a 1-chip console does not exhibit corrupted tile data.
    More investigation is required.

    <figure class="fixed-width">
     <img src="ob-glitch-hdma-3chip.jpeg" alt="Cropped screen capture showing corrupted object tiles" />
     <figcaption>
      Corrupted object tiles caused by a HDMA write to `INIDISP` on every scanline.  
      Recorded on a 3 Chip SFC console [(raw screen capture)](ob-glitch-hdma-3chip-raw.jpeg).  
      1-CHIP consoles do not show object tile corruption [(raw screen capture)](ob-glitch-hdma-1chip-raw.jpeg).
     </figcaption>
    </figure>

  * The display erroneously turning on for a single dot when
    *force-blank* is enabled and an `INIDISP` write occurs outside the
    Horizontal or Vertical blanking-periods while the previous value on
    the data-bus has bit 7 (MSB) clear.

    On 1-CHIP consoles the glitch is extremely faint and hard to see. [^early-read-1chip-0f8f]

    <figure class="fixed-width">
     <img src="ob-glitch-hammer-8f0f-3chip.jpeg" alt="Cropped screen capture showing diagonal brown lines" />
     <figcaption>
      Test ROM that repeatedly writes `0x8f` to `INIDISP` when the previous value on the data-bus was `0x0f`.  
      Recorded on a 3 Chip SFC console [(raw screen capture)](ob-glitch-hammer-8f0f-3chip-raw.jpeg).  
      1-CHIP consoles show an extremely faint glitch. [^early-read-1chip-0f8f]
     </figcaption>
    </figure>

  * A sudden change in screen brightness, lasting for a single dot, when
    an `INIDISP` write occurs outside the Horizontal or Vertical
    blanking-periods and the previous value of the data-bus does not
    match the value of the `INIDISP` write.  This can also cause tile
    corruption on 3-chip consoles if previous value on the data-bus has
    bit 7 (MSB) set.

    <figure class="fixed-width">
     <img src="ob-glitch-hammer-0f00-3chip-obj.jpeg" alt="Cropped screen capture showing faint dots above background and object tiles" />
     <img src="ob-glitch-hammer-0f00-3chip-bg.jpeg" alt="Cropped screen capture showing faint dots above background tiles" />
     <figcaption>
      Tiny dots on the screen, no tile corruption.  
      Test ROM that repeatedly writes `0x0f` to `INIDISP` when the previous value on the data-bus was `0x00`.  
      Recorded on a 3 Chip SFC console [(raw screen capture)](ob-glitch-hammer-0f00-3chip-raw.jpeg).  
      1-CHIP consoles show similar output [(raw screen capture)](ob-glitch-hammer-0f00-1chip-raw.jpeg).
     </figcaption>
    </figure>
    <figure class="fixed-width">
     <img src="ob-glitch-hammer-0f8f-3chip-obj.jpeg" alt="Cropped screen capture showing object tile corruption and black diagonal lines" />
     <img src="ob-glitch-hammer-0f8f-3chip-bg.jpeg" alt="Cropped screen capture showing background tile corruption and black diagonal lines" />
     <figcaption>
      Black dots and corrupted tiles.  
      Test ROM that repeatedly writes `0x0f` to `INIDISP` when the previous value on the data-bus was `0x8f`.  
      Recorded on a 3 Chip SFC console [(raw screen capture)](ob-glitch-hammer-0f8f-3chip-raw.jpeg).  
      1-CHIP consoles do not show tile corruption.  [(raw screen capture)](ob-glitch-hammer-0f8f-1chip-raw.jpeg).
      </figcaption>
     </figure>


<aside markdown="1">
FXPAK firmware 1.10.3 and earlier contains a bug that can activate this
glitch. [^early-read-ikari]

The bug has been fixed in FXPAK Firmware v1.11.0 beta 1. [^early-read-fxpak-fix]
</aside>


[^early-read-nesdev-forums]: [NESdev forums - Errors with INIDISP](https://forums.nesdev.org/viewtopic.php?t=21971)
[^early-read-ikari]: [Status update – important stability fixes](https://sd2snes.de/blog/archives/1116)<br/>(Contains a detailed description of the `INIDISP` early-read glitch)
[^early-read-1chip-0f8f]: While I am able to see the glitch on screen it is too faint to capture with my screen capture setup or with a camera.
[^early-read-fxpak-fix]: FXPAK [Firmware v1.11.0 beta 1](https://sd2snes.de/blog/archives/1157)


### Mitigations {#early-read-mitigations}

Writes to `INIDISP` during Vertical-Blank will not exhibit this glitch.

If the `INIDISP` write is emitted by a CPU instruction, an `sta long`
instruction can be used to pre-load the data-bus in the CPU-cycle before
the `INIDISP` write. [^early-read-mitigations]

 * When enabling *force-blank*, use a `sta $802100` (long addressing) instruction (8 bit accumulator).  
   The data-bus will hold the value `$80` before the write-cycle to `INIDISP`.

 * When disabling *force-blank*, use a `sta $0f2100` (long addressing) instruction (8 bit accumulator).  
   The data-bus will hold the value `$0f` before the write-cycle to `INIDISP`.

 * If the state of the *force-blank* bit is unknown, prefer `sta $0f2100`.  
   It may erroneously enable the display for a single dot (if the write
   occurs outside of the blanking-period) but it should not result in
   glitched Object tiles.


Alternatively the bug can be mitigated by preforming a 16 bit write to
`$0020ff` with identical high and low bytes. [^early-read-mitigations]
[^early-read-20ff]

```
// DB access registers
// 8 bit Accumulator
// 16 bit Index

// tmp is a 16 bit variable

    sta  tmp
    sta  tmp + 1

    ldx  tmp
    stx  $20ff
```


[^early-read-mitigations]: This mitigation will not work if HDMA activates in the middle of the write instruction.

[^early-read-20ff]: This mitigation can only be used if address `$0020ff` is unmapped by the cartridge.



### Mitigations (HDMA) {#early-read-hdma-mitigation}

When writing to `INIDISP` via HDMA, a *2 registers write once* HDMA
transfer (`DMAPx` = `0x01`) to B-Bus address `$21ff` (`BBADx` = `0xff`)
can be used to pre-load the data-bus with the correct value before the
write to `INIDISP`. [^hdma-to-21ff]


[^hdma-to-21ff]: This mitigation can only be used if B-Bus address `$21ff` is unmapped by the cartridge.



DMA failure when HDMAing to INIDISP {#dma-failure-hw-bug}
-----------------------------------

On a S-CPU-A (CPU v2) SNES console, a DMA transfer can fail if there was
a recent HDMA transfer with a `BBADx` (DMA Bus B address) value of
`0x00` (transfer to/from `INIDISP`).

If the transfer fails, no data is transferred during DMA and the `DASx`
(DMA Size) register remains unchanged.

Only S-CPU-A is affected.  S-CPU (v1), S-CPU-B and S-CPUN (1-CHIP) do
not experience this hardware bug. [^dma-fail-investigation]

The exact cause of this hardware bug is still unknown.

[^dma-fail-investigation]: [@undisbeliever - Over the past few days I have been working on a test ROM to investigate a new hardware bug that Near found.](https://twitter.com/UnDisbeliever/status/1346459716168286209)

<br/>

The most reliable mitigation for this bug (aside from not HDMAing to
`INIDISP`) is to use a *2 registers write once* HDMA transfer (`DMAPx` =
`0x01`) to B-Bus address `$21ff` (`BBADx` = `0xff`).  The first byte
will be transferred to `$21ff`, the second byte to `$2100`.  Both bytes
should contain the same value to prevent the [early read glitch](#early-read-glitch)
mentioned above. [^hdma-to-21ff]

Retrying the DMA transfer if `BBADx` is non-zero is not recommended.


<br/>

Scattered reports of DMA failing in homebrew code have been around for
years.  The cause was unknown until late December 2020 when Near
encountered a corrupted text box while working on their Bahamut Lagoon
localization and investigated why DMA was failing with Ikari_01.



Advanced Usage
==============


Extending VBlank time with force-blank {#extended-vblank}
--------------------------------------

Extended VBlank (commonly called letterboxing) is a common technique to
increase the amount of data that can be transferred to the PPU during
Vertical-Blank.  An IRQ Interrupt is used to disable the screen (with
*force-blank*) and start the VBlank routine earlier then normal,
decreasing the number of visible scanlines and allocating them to the
VBlank routine.

For aesthetic purposes, the screen remains disabled at the start of the
next frame and will be enabled (by clearing *force-blank*, usually by
an IRQ Interrupt) a number of scanlines later to create a letterbox
effect.

Every scanline removed from active display adds an extra 165 bytes of
DMA time to the extended VBlank.

To prevent background tile glitches the screen must be disabled and
reenabled during Horizontal-Blank.

One downside of extended VBlank is that the visible first scanline will
be missing sprites and may contain sprite glitches.  It is possible to
hide them by using one the following techniques:

  * Disabling the Sprite layer on the first visible scanline
    (useful for status bars at the top of the screen).
  * Disabling the Background and Sprite layers for the first visible
    scanline.  (Works best if the background colour is black.)
  * Covering the sprites with a black background tile
    (using Mode 1 with BG3-priority).
  * Covering the background and sprites with a Window.


<figure markdown="span">
  <img src="extended-vblank.png" alt="Annotated Mesen-S Event Viewer screenshot" />
  <figcaption markdown="1">
Using letterboxing to transfer 8736 bytes of data during VBlank.  
The glitched scanline with missing/corrupted Object tiles is hidden by disabling Background and Sprite layers with the `TM` register.
  </figcaption>
</figure>


<aside markdown="1">
Some 2-player horizontal split-screen games enable *force-blank* in the
middle of the screen to transfer a second OAM buffer to OAM to increase
the number of onscreen sprites.
</aside>



Extended VBlanks are more sensitive to timing issues then regular
VBlanks.  Extra care must be taken to ensure correct operation at all
times.  Always ensure an extended-VBlank will never overrun.

Depending on how the extended VBlank is implemented, an extended-VBlank
overrun can result in one or more of the following:

  * Fullscreen flashing (major photosensitity issue).
  * A blank display frame.
  * A model-1 DMA/HDMA crash.
  * Corrupted VRAM/CGRAM/OAM data.
  * Corrupted HDMA effects.
  * Missing lines at the top of the screen (photosensitity issue).

It is better to shrink the active display and add more scanlines to
VBlank then to overrun an extended-VBlank.



Using INIDISP as a quick and dirty CPU usage meter {#cpu-usage-meter}
--------------------------------------------------

As the *brightness* bits can be written to at any time, the `INIDISP`
register can be used to create a quick and dirty CPU usage meter.

By changing the *brightness* bits just before the main-loop pauses
execution, a visual representation of CPU usage will be shown on screen.

The screen will output a sudden change in brightness at the point where
the main-loop is waiting for the VBlank interrupt, allowing developers
to quickly estimate the amount of free CPU time available.


```
// DB access registers
// 8 bit Accumulator

    lda     #10
    sta     INIDISP

    // Wait until the end of the VBlank routine
    jsr     WaitUntilVBlank

    // VBlank routine will restore INIDISP back to full brightness
```


<figure class="two-images" markdown="span">
 <img src="cpu-usage-meter-frame-08.jpeg" alt="Screen capture showing player, 4 crossbowmen, 3 crossbow bolts.  The lower 94% of the image is at reduced brightness." />
 <img src="cpu-usage-meter-frame-09.jpeg" alt="Same scene as the previous image, but 1 display frame later.  The lower 90% of the image is at reduced brightness." />
 <figcaption markdown="1">
  Two sequential display frames using an `INIDIDP` CPU usage meter.
  <p markdown="1">
  The first frame has ~15 scanlines at full brightness. `(224 - 15) / 262` = ~79.8% CPU free, ~20.2% CPU used.  
  The second frame has ~25½ scanlines at full brightness. `(224 - 25.5) / 262` = ~75.8% CPU free, ~24.2% CPU used
  </p>
 </figcaption>
</figure>



<div class="warning" markdown="1">
### Photosensitivity Warning

**This technique will create an unpredictable flashing pattern.**

The flashing is worsened if the game ever experiences a lag frame.
During a lag frame **the flash can cover the entire screen**.  For
example, one frame would display 100% of the screen at full brightness,
the next frame would display over 95% of the screen at reduced
brightness.

<figure class="two-images" markdown="span">
 <img src="cpu-usage-meter-lag-1.jpeg" alt="Screen capture of the first lag frame.  The entire screen is at full brightness." />
 <img src="cpu-usage-meter-lag-2.jpeg" alt="Screen capture of the second lag frame.  The entire screen is at reduced brightness." />
 <figcaption markdown="1">
  30 Hz flashing during a lag-frame.  
  In this example, the reduced brightness `INIDISP` write occurs during
  the Vertical Blanking period of every second display frame.
 </figcaption>
</figure>

This technique is only included for legacy reasons.  
**You should not include an `INIDISP` CPU Usage meter in a home-brew game.**


<!-- ::TODO create a page about different methods of implementing a CPU Usage meter:: -->


For more information about photosensitity in video games, please read the
[Xbox Accessibility Guideline 118: Photosensitivity](https://docs.microsoft.com/en-us/gaming/accessibility/xbox-accessibility-guidelines/118).

</div>


