# mist-binaries-test

## fixes-for-minimig-core
Binaries of the repositories/branches
* https://github.com/retrofun/mist-firmware/tree/fixes-for-minimig-core
* https://github.com/retrofun/minimig-mist/tree/fixes-for-minimig-core

### mist-firmware

Branched from [_mist-firmware/master_](https://github.com/mist-devel/mist-firmware), a257988

* Fix fatal error 4 when choosing new FPGA core

  minimig setup in fpga_init() calls ChangeDirectory(DIRECTORY_ROOT) that
  causes a "No FPGA configuration file found!" fatal error 4 when choosing
  a new core immediatly after the minimig core has been started. Save old
  directory and restore it after minimig setup.

* Make Kickstart 1.2/1.3 honour changes of memory configuration

  Kickstart 1.2 & 1.3 don't honour memory configuration changes on reset.
  This is annoying. Added a patch that forces Kickstart 1.2/1.3 to reconfigure memory on every reset.

  Works with standard (256KB) and overdumped (512KB) 1.2/1.3 ROM images.

  Patch is active by default and this is shown in the minimig startup screen left
  to memory configuration: ``[Kick 1.x patch enabled]``.

  Kickstart ROM indicates the memory detection with a dark grey screen background.

  Patch can be disabled in mist.ini, section _minimig_config_:

        [minimig_config]
        kick1x_memory_detection_patch=0

  **TODO/not tested (yet):**
  * does this work with resident programs?
  * does the resident ram disk RAD: still work?

* Improve stability of core startup when a new configuration is loaded

  * After Kickstart ROM upload clear CPU Vector Table before resetting the CPU
  * Clear Kickstart Mirror at $E0xxxx when Kickstart ROM size is 256KB

* Fix IDE on/off, ask to reboot when changed

  Ask to reboot when IDE on/off changes, not only when hard disk master/slave configuration changes.

### minimig-mist

Branched from [_minimig-mist/dev_](https://github.com/rkrajnc/minimig-mist/tree/dev), 34ccb73

* minimig_mist_20190924.rbf

  same as minimig_mist_20190922.rbf but synthesized with _Fitter effort: Standard Fit_

* minimig_mist_20190922.rbf

  * TG68K: implementation of RTD instruction

    Fixes crash of mmu.library from MMULib package (http://aminet.net/package/util/libs/MMULib)

* minimig_mist_20190727.rbf

  * Disable joystick1/mouse when OSD is active

    This is a follow-up commit for [gyurco's](https://github.com/gyurco/minimig-mist/tree/dev) CD 32 pad changes
  * Code cleanup with changes from Minimig-AGA_MiSTer

    Cleanup with cherry-picks/backports from [Minimig-AGA_MiSTer](https://github.com/MiSTer-devel/Minimig-AGA_MiSTer), mainly memory map changes/fixes
    * Kickstart ROM memory areas are now write-protected(!)
    * new memory layout enables up to 1.5MB SLOW MEM instead if just 512KB.
    Now max. memory is 2MB CHIP + 1.5MB SLOW + 24MB FAST = 27.5MB!
