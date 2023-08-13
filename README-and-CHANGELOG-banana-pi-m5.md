This is a rundown on the adjustments that I made in order to be able to compile linuxsampler on Debian 10 on an Banana Pi M5. You should consider the changes to be experimental, although my experience so far indicates that the resulting programm can be used as a MIDI-Piano in a stable manner.


## Build and install on Debian 10

* Install all the required dependency libraries throught `apt`, make and install liblscp and libgig as found on [https://www.linuxsampler.org/downloads.html#linuxsampler](https://www.linuxsampler.org/downloads.html#linuxsampler). You probably need to install bison and flex just to make the linuxsampler compile.
* On Debian 10, automake has a different version. It can be cured by running `aclocal` in the root folder.
* To figure out missing dependencies, run `./configure` and read the output carefully.
* To build, run `AUTOMAKE=automake make`.
* Optionally, run `sudo make install` (seems to recompile, though), or use it in place, for instance, via `sudo ln -s ./src/linuxsampler $HOME/bin`.
* I use it with qsampler (which is available through apt) as frontend, the [Maestro Concert Grand v2](https://www.linuxsampler.org/instruments.html), a Focusrite Scarlet 4i4, an M-Audio Keystation 88 MK3 and qjackctl (also for patching) on a Banana Pi M5 which autostarts the sampler (no HIDs, no display, readonly mode).

## Weird issue with yyprhs

* Install bison on the system and reconfigure, then the error about `yyprhs` goes away. No changes to the code needed.

## atomic.h fixed

* I've added code from linux kernel source v4.1 for arm64 to `src/common/atomic.h`. This routines seem to be used by the virtual midi driver.


## rdtsc fix in RTMath.cpp

* Found solution to problem on [stack overflow](https://stackoverflow.com/questions/40454157/is-there-an-equivalent-instruction-to-rdtsc-in-arm), added arch64 code from there. The code has probably been taken from the kernel source as well.

