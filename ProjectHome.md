## Overview ##

This project consists of a kernel module for OMAP3 phones with Android to unlock any processor frequency/voltage combination and a companion graphical app to ease the configuration. The phone must be rooted so that the kernel module can be loaded.

The module has an interface in /proc/overclock/`*` that allows enabling and disabling of overclock in runtime without rebooting. No flashing of custom roms or kernels is needed, since that is not even possible with the Milestone, Droid X and others.

## Supported phones ##

Most phones with OMAP3 processors are supported:

  * Motorola: Milestone, Milestone 2, Droid, Droid 2 [Global](Global.md), Droid X, A853/A854, XT701/XT702, XT711, XT720, XT800/XT800W, Defy, Flipout
  * Samsung: Galaxy Beam, Galaxy A
  * Archos: Tablet A70/A101

Not supported: most other phones, since they use Qualcomm, Snapdragon or Hummingbird processors. This means Evo, Eris, Legend, other Galaxy versions, X10, Ally, etc, are not supported and likely never will be, unless I get my hands on one to properly study it and try to port the overclock kernel module to it, or someone else does it.

## News ##

  * 2011-03-20 - Release 1.4.8 ([ChangeLog](ChangeLog#Version_1.4.8_-_2011-03-20.md)), At last, support Motorola Milestone with Froyo (2.2.1)
  * 2011-01-16 - Release 1.4.7 ([ChangeLog](ChangeLog#Version_1.4.7_-_2011-01-16.md)), Support Droid 2 Global, Motorola XT711 and some bugfixes
  * 2010-12-17 - Release 1.4.6.1 ([ChangeLog](ChangeLog#Version_1.4.6.1_-_2010-12-17.md)), **Motorola Milestone 2**, **Motorola Flipout**, **Samsung Galaxy A** and **Archos Tablet A70/A101** are now supported!
  * 2010-12-09 - Release 1.4.5 ([ChangeLog](ChangeLog#Version_1.4.5_-_2010-12-09.md)), **Droid 1 with Froyo** and **Motorola Defy** are now supported! Also, numeric fields in settings now only allow numbers (thanks Nicolas Frenay)
  * 2010-10-29 - Release 1.4.4.1, **Droid 2** is now supported as well
  * 2010-10-28 - Release 1.4.4 ([ChangeLog](ChangeLog#Version_1.4.4_-_2010-10-28.md)), **leaked Milestone 2.6.32 kernel** is now supported! And minor improvements to the UI.
  * 2010-10-26 - Bugfix release 1.4.3.1, fixed slider bar in Droid X
  * 2010-10-25 - Release 1.4.3 ([ChangeLog](ChangeLog#Version_1.4.3_-_2010-10-25.md)), **Froyo on Droid X** is now supported! Also supports XT800/XT800W and more XT720 versions
  * 2010-08-31 - Release 1.4.2 ([ChangeLog](ChangeLog#Version_1.4.2_-_2010-08-31.md)), **Samsung Galaxy Beam** is now supported, also support Motorola XT702 and fixed su multiple permission problem for some users introduced in 1.4.1
  * 2010-08-30 - Bugfix release 1.4.1 ([ChangeLog](ChangeLog#Version_1.4.1_-_2010-08-30.md)), XT701 and cpufreq stats
  * 2010-08-27 - Released version 1.4 ([ChangeLog](ChangeLog#Version_1.4_-_2010-08-27.md)), now with **autoload** overclock on boot, support for **Droid X** (2.1), A854 and XT720, and improved root detection. Still no froyo, I'm working on it.
  * 2010-05-31 - Bugfix release 1.3.2, restores support for Milestone A853
  * 2010-05-30 - Released version 1.3 on the [Market](http://www.cyrket.com/p/android/pt.com.darksun.milestoneoverclock/) (thanks to all who [donated](Donate.md)) - **now with Droid support!** (if you have 1.2 installed you must uninstall it or 1.3 will fail the signature check)
  * 2010-05-27 - Added [FAQ](FAQ.md) page and joined [Flattr](http://flattr.com/thing/6956/Android-MilestoneDroid-overclock-app)
  * 2010-05-26 - Added [Screenshots](Screenshots.md) page
  * 2010-05-25 - Released app/module version 1.2 ([ChangeLog](ChangeLog#Version_1.2_-_2010-05-25.md)) - **Should work with any Milestone with stock Android 2.x!**
  * 2010-05-24 - Confirmed to work with Android 2.0.1! Addresses: freq\_table\_addr=0xc0509b24, mpu\_opps\_addr=0xc050a848 (French and UK Milestone -- Thanks laurent.commarieu and clausgedved)
  * 2010-05-23 - Released app version 1.1 ([ChangeLog](ChangeLog#Version_1.1_-_2010-05-23.md))
  * 2010-05-22 - Uploaded busybox with working dd to assist in disassembly

## How does it work? ##

It works by changing several parameters directly in kernel memory to fool both cpufreq and its lowlevel driver. Like the Droid, the Milestone is stable up to 1.2 GHz, with 1.33 GHz being the best I could achieve, albeit completely unstable. For more information please read the [KernelModule](KernelModule.md) description. Afterwards I recommend you define better cpufreq policies with the excellent application [SetCPU](http://www.pokedev.com/setcpu/) (make sure to use Autodetect, not the Droid/Milestone preset).

For this to work, the module must know two memory addresses that are specific to each kernel. Since version 1.2 the app attempts to autodetect the needed values and should work with any Milestone, Droid, Droid X and similar phones with stock Android 2.x. See [KernelModule](KernelModule.md) for more information. To port the module to other kernels please follow the instructions in the [Disassembly](Disassembly.md) page.

## Other phones ##

In theory this technique is perfectly feasible in any Android phone, and will surely work in phones based on Texas Instruments OMAP3. You may say it's unneeded if you can use custom firmwares with overclocking enabled; but you must flash a specific kernel to get a particular speed. It would be much better to be able to set any maximum frequency/voltage on the fly without flashing or rebooting, and that's what this project enables. You'd have to port the kernel module over to the new kernel; I've written a detailed howto in the [Disassembly](Disassembly.md) page explaining this. The number of OMAP3 phones supported are proof that the theory is sound. Other platforms should work, albeit with significant developer efffort.

## Donating ##

If you'd like to support this project please visit the [Donate](Donate.md) page.

<p>
<b>Have fun,</b><br />
<i>Tiago Sousa aka mirage</i>
</p>