### Version 1.4.8 - 2011-03-20 ###

App:

  * Support Motorola Milestone with official Froyo (2.2.1) rom
  * Temporarily disabled stats lookup because of reboots

Kernel Module:

  * Build process for motorola milestone 2.2

### Version 1.4.7 - 2011-01-16 ###

App:

  * Support Droid 2 Global, Motorola XT711, Defy running chinese Froyo
  * Fixed race condition between insmod and setting module parameters

Kernel Module:

  * Added sr\_adjust\_vsel field when reading the /proc/overclock/mpu\_opps table if SMARTREFLEX is enabled (thanks tekahuna)

### Version 1.4.6.1 - 2010-12-17 ###

Kernel Module:

  * Rolled back the automatic detection, needs more testing

### Version 1.4.6 - 2010-12-16 ###

App:

  * Support Motorola Milestone 2, Motorola Flipout, Samsung Galaxy A and Archos Tablet A70/A101

Kernel Module:

  * Automatic detection of omap2\_clk\_init\_cpufreq\_table\_addr and cpufreq\_stats\_update\_addr in most 2.6.32 kernels (by Skrilax\_CZ and kabaldan)
  * Build process for Motorola Flipout, Samsung Galaxy A and Archos Tablet A70/A101

### Version 1.4.5 - 2010-12-09 ###

App:

  * Support Droid 1 with Froyo and Motorola Defy
  * Numeric fields in settings now only allow numbers (thanks Nicolas Frenay)

Kernel Module:

  * Build process for Motorola Defy

### Version 1.4.4.1 - 2010-10-29 ###

App:

  * Support Droid 2

### Version 1.4.4 - 2010-10-28 ###

App:

  * Support leaked Milestone 2.6.32
  * Smaller slider bar
  * Moved Autoload checkbox to the main page and added sdcard status
  * Improved settings writing logic

Kernel Module:

  * Build process for leaked Milestone 2.6.32 kernel

### Version 1.4.3.1 - 2010-10-26 ###

App:

  * Fixed slider bar in Droid X

### Version 1.4.3 - 2010-10-25 ###

App:

  * Improved the preset frequencies for Droid X
  * Support XT800W/XT800 and more XT720 versions

Kernel Module:

  * Build process for Droid X 2.2
  * Fix vsel setting for Droid X 2.x and others (thanks to kabaldan for hinting at this; the struct omap\_opp changed to support "Smart Reflex class 1.5" technology)
  * Remove call to omap\_pm\_cpu\_get\_freq() because it's no longer exported in Froyo (thanks tekahuna)

### Version 1.4.2 - 2010-08-31 ###

App:

  * Support Samsung Galaxy Beam
  * Support Motorola XT702
  * Reversed su usage from 1.4.1, should fix multiple permissions problem experienced with some Superuser apps

Kernel Module:

  * Updated build process to support Samsung Galaxy Beam

### Version 1.4.1 - 2010-08-30 ###

App:

  * Fix support for XT701
  * Fix cpufreq\_stats\_table\_addr when saving settings and loading the module

Kernel Module:

  * Change cpufreq stats when writing to /proc/overclock/freq\_table

### Version 1.4 - 2010-08-27 ###

App:

  * Autoload overclock on boot. This requires a mounted external storage. If the phone freezes during boot, simply delete the file /sdcard/Android/data/pt.com.darksun.milestoneoverclock/files/autoload (or remove the sdcard) to disable autoload.
  * Support for Droid X (2.1), A854 and XT720
  * Improved root detection (tries to do a "su echo test" to check for a working su)

Kernel module:

  * Support kernel 2.6.32 (AOSP's Froyo), although I can't get it to load on Droid no matter what vermagic I use
  * Improve the build process to compile against multiple kernels
  * Stop using default addresses for mpu\_opps\_addr and cpufreq\_stats\_table\_addr to avoid confusion
  * Get the address of freq\_table automatically with cpufreq\_frequency\_get\_table()
  * Fix assignments when writing to /proc/overclock/omap2\_clk\_init\_cpufreq\_table\_addr (thanks kabaldan)
  * Fix cpufreq stats (patch by kabaldan)

### Version 1.3.2 - 2010-05-31 ###

App:

  * Restore support for Milestone A853

### Version 1.3 - 2010-05-30 ###

App:

  * **Works with the Droid**, at least with stock kernels

### Version 1.2 - 2010-05-25 ###

App:

  * **Should work with any Milestone with stock Android 2.x**
  * Searches /proc/kallsyms for the address of omap2\_clk\_init\_cpufreq\_table and passes it to the kernel module when it is loaded

Kernel module:

  * Change the values of freq\_table by writing "index frequency" to /proc/overclock/freq\_table such as: echo "1 400000" > /proc/overclock/freq\_table
  * Change the values of mpu\_opps by writing "index rate vsel" to /proc/overclock/mpu\_opps such as: echo "3 400000000 50" > /proc/overclock/mpu\_opps
  * Autodetect freq\_table and mpu\_opps given omap2\_clk\_init\_cpufreq\_table's address at load time (load with omap2\_clk\_init\_cpufreq\_table=0xc004e498) or in runtime with: echo 0xc004e498 > /proc/overclock/omap2\_clk\_init\_cpufreq\_table\_addr (you can find the address with: grep omap2\_clk\_init\_cpufreq\_table /proc/kallsyms)

### Version 1.1 - 2010-05-23 ###

App:

  * Created settings menu with custom rate and custom vsel (available in the seekbar), and manual freq\_table and mpu\_opps addresses (used when the module is loaded)
  * Corrected vsel units

Kernel module:

  * mpu\_opps now configurable in load time
  * Improved mpu\_opps address lookup
  * Corrected vsel units
  * Applied mattih's patch to set policy->user\_policy.max

### Version 1.0 - 2010-05-17 ###

  * Initial release