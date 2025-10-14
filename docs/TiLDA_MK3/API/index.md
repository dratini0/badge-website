## Built-in

- [documentation](http://docs.micropython.org/en/latest/pyboard/) -
  General Micropython library
- [uGFX](ugfx.md) - The TiLDA LCD colour screen
- <a href="TiLDA_MK3/documentation/cc3100" class="wikilink"
  title="CC3100">CC3100</a> - The wifi chip
- <a href="TiLDA_MK3/rtc" class="wikilink" title="RTC">RTC</a> (real
  time clock)
- <a href="TiLDA_MK3/adc" class="wikilink" title="ADC">ADC</a> (analogue
  reading)
- <a href="TiLDA_MK3/timer" class="wikilink" title="Timer">Timer</a>
- Microcontroller peripherals
  [1](https://docs.micropython.org/en/latest/pyboard/library/pyb.html)
  (Timers, PWM, serial,
  <a href="TiLDA_MK3/spi" class="wikilink" title="SPI">SPI</a> etc)

## TiLDA Libraries

On top of the build-in modules above we have also created a bunch of
helpful libraries written in python. If you go through the bootstrap
process or use the App Library you should always have a full set of
those on your badge. If for some reason this isn't the case you can
download our repository from <https://github.com/emfcamp/Mk3-Firmware>
and copy the `lib` folder onto your badge.

TBD, for now please have a look at the libraries themselves:
<https://github.com/emfcamp/Mk3-Firmware/tree/master/lib>

- <a href="TiLDA_MK3/lib/buttons" class="wikilink"
  title="buttons">buttons</a>
- database
- dialogs
- filesystem
- http_client
- imu
- wifi
- NTP Example:
  <https://gist.github.com/drrk/4a17c4394f93d0f9123560af056f6f30>
- On board LED "<a href="TiLDA_MK3/NeoPixel" class="wikilink"
  title="NeoPixel">NeoPixel</a>" example:
  <https://github.com/mayhem/tilda-mk3-led-demo>

Full hardware files are on GitHub
[2](https://github.com/emfcamp/Mk3-Hardware)

(feel free to add additional ideas, and create links new wiki pages to
on-going projects, perhaps someone will want to contribute)
