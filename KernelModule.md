## Introduction ##

The module has an interface in /proc/overclock/ that allows enabling and disabling of overclock in runtime without rebooting. It works by changing several parameters directly in kernel memory to fool both CPUfreq and its lowlevel OMAP driver.

The MPU (Microprocessor Unit) clock driver has a number of discrete pairs of possible rate frequencies and respective voltages. In the Milestone, there are 5 pairs, of which only 4 are passed down to CPUfreq as you can see with a tool such as SetCPU. The default frequencies are 125, 250, 500 and 550 MHz (and a hidden 600). By using this module, you are changing the highest pair in the tables of both cpufreq and MPU frequencies, so it becomes 125, 250, 500 and, say, 800.  It's quite stable up to 1200; beyond that it quickly becomes unusable, specially over 1300, with lockups or spontaneous reboots. The exact values vary for each phone model, and even in the same model, not all phones can handle the same frequency/voltage combinations due to manufacturing differences.

For this to work, the module must know at least one memory address that is specific to each kernel (and another address if you want cpufreq stats working). Since version 1.2 the kernel attempts to autodetect them if the parameter omap2\_clk\_init\_cpufreq\_table\_addr is set (more on this below).

The app was initially developed in a Motorola Milestone UK with official Android 2.1 for Central Europe, build number SHOLS\_U2\_02.31.0. Fortunately Motorola appears to have used the same kernel in most of the 2.1 and 2.0 firmwares. Users with different firmwares often reported failures at first, but autodetection of the kernel parameters solved the problem. Then came new android versions and new phones, but the same method has proved successful in all cases, usually requiring little more than a recompile.

## Usage ##

The module is created at /data/data/pt.com.darksun.milestoneoverclock/files/overclock.ko when it is first loaded through the app. Afterwards you can load and use it directly, if you know the address for your phone:

```
insmod overclock.ko
echo 0xc050a848 > /proc/overclock/mpu_opps_addr # will vary for each phone/firmware!
echo 62 > /proc/overclock/max_vsel
echo 800000 > /proc/overclock/max_rate
```

You should set max\_vsel before max\_rate if the new rate is going to be higher than the current one, because higher frequencies often require more voltage than supplied by default.  Likewise, lower the max\_rate first before max\_vsel if you want to reduce both frequency and voltage:

```
echo 550000 > /proc/overclock/max_rate
echo 56 > /proc/overclock/max_vsel
```

To set a specified frequency and voltage at load time:
```
insmod overclock.ko mpu_opps_addr=0xc050a848 max_rate=800000 max_vsel=62
```

Remember that you are merely changing the maximum possible value that CPUfreq can choose to use. The current speed may well be lower than the one specified if the phone is idle. I recommend the use of [SetCPU](http://www.pokedev.com/setcpu/) to effectively change the current frequency through CPUfreq policies.

## Autodetection ##

Finding the correct mpu\_opps\_addr can be difficult and fortunately there's an easier way. You can ask the module to try to autodetect it if you give it the address of omap2\_clk\_init\_cpufreq\_table. The app does this automatically. You can easily find it manually with:

```
# grep omap2_clk_init_cpufreq_table /proc/kallsyms
c004e498 T omap2_clk_init_cpufreq_table
```

So now you can load the module with:

```
insmod overclock.ko omap2_clk_init_cpufreq_table_addr=0xc004e498
```

And in dmesg you should see something like this:

```
[ 9361.096099] overclock: found mpu_opps_addr at 0xc050c888
```

If autodetection doesn't work for you, you can specify the values manually as stated earlier. Finding out the values will require live disassembly of kernel code. See the [Disassembly](Disassembly.md) page for more information.

## Changing the frequency/vsel tables ##

Also for the more hardcore among you, you can now change values directly in freq\_table and mpu\_opps with (that's "index freq" and "index rate vsel"):

```
echo "1 400000" > /proc/overclock/freq_table
echo "3 400000000 50" > /proc/overclock/mpu_opps
```

Check the results with cat /proc/overclock/freq\_table and cat /proc/overclock/mpu\_opps. This can be used for example to "underclock" your CPU and save battery, or to change any frequencies you wish, not just the highest one.

## Frequencies for Milestone/Droid ##

The following table lists frequencies/voltages that appear to work well together, albeit with very conservative voltages. These are the values available in the app by default (thanks to kabaldan for corrections):

| **Frequency (KHz)** | **vsel** | **Voltage (V)** |
|:--------------------|:---------|:----------------|
| 550000              | 56       | 1.3             |
| 600000              | 62       | 1.375           |
| 800000              | 68       | 1.45            |
| 1000000             | 74       | 1.525           |
| 1200000             | 80       | 1.6             |

The maximum vsel I could use was 127. The safe upper limit in the processor specs is 1.8V which translates to vsel 96 (according to [wikidroid](http://wiki.droidmod.org/doku.php?id=vsel_calc)). Frequencies above 1250000 usually reboot spontaneously.

The community at [android-hilfe](http://www.android-hilfe.de/root-hacking-modding-fuer-motorola-milestone/28049-overclocking-verwendete-spannungen.html) has made extensive tests to find exactly what range of voltage selections could be used in each frequency. There are variations because not all Milestones are made equal, some can handle higher voltages and frequencies than others. Here is a copy of the table they produced, available [here](http://www.android-hilfe.de/root-hacking-modding-fuer-motorola-milestone/28049-overclocking-verwendete-spannungen.html) (thanks doc.payce!):

<table cellpadding='3' border='1'>
<tbody align='center'>
<tr><td></td><td><b>max_vsel</b></td></tr>
<tr><td><b>Frequency</b></td><td><b>Stable (>=)</b></td><td><b>Possibly unstable</b></td><td><b>Probably unstable (<=)</b></td></tr>
<tr><td>550</td><td>56</td><td></td><td></td></tr>
<tr><td>800</td><td>58</td><td>56 .. 54</td><td>52</td></tr>
<tr><td>1000</td><td>60</td><td>58 .. 56</td><td>54</td></tr>
<tr><td>1100</td><td>64</td><td>62 .. 60</td><td>58</td></tr>
<tr><td>1200<code>*</code></td><td>76</td><td>74 .. 70</td><td>68</td></tr>
<tr><td>1330<code>*</code></td><td></td><td>84</td><td></td></tr>
</tbody>
</table>
`*`: Always unstable in some CPUs! Will damage CPU on prolonged use. According to OMAP3430 datasheet, max\_vsel up to 66 should be acceptable. Above 80 will certainly severly damage the CPU on long-term scale. The most appropriate and stable settings seem to be either 800 MHz or 1000 MHz at max\_vsel of 56 to 60.