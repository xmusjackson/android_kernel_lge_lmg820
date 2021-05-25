# Stock+ 4.14.117 for LG G8 US (TMO)
This will be the source for my LG G8 kernel.

The kernel should now be working.
I am not finished making changes, and I'd also like to merge some kernel updates
as well at some point, but for now, this is a better kernel for our LG G8's,
namely because we can compile modules against this source for use inside
of android.

In order to get this working, you have to pull the modules out of your
'out' directory or wherever your compiled files ended up.
Take a look at your /vendor/lib/modules to get a list of the modules you need.
Non audio modules needed will be 'wmc_drv.ko' (for LG Pay) and 'vpnclient.ko'
(for LG proprietary VPN client)
You may note that there is an extra module on your system that we can't compile,
'texfat.ko'. Our kernel will have native exfat support, but getting vold to use
the native support will be trickier. We may have to compile vold seperately later,
but I have a bit to learn before I can do that. For now we have a couple of options.
We can force the kernel to load modules, reguardless of signatures.
We could try to resign the module with the new signing keys, but I'm not sure the
modversion will pass. I'll have to look into this.

## For now we lose native automounting of exfat partitions.

Audio modules will compile as <module>_dlkm.ko
They need to be renamed to audio_<module>.ko
One of the modules, machine_dlkm.ko needs to be renamed to audio_dlkm_msmnile.ko
This is the only file that needs to be renamed differently, the rest can be done
with simple commands like:

find . -name \*_dlkm.ko -exec cp {} modules \;
rename -- _dlkm.ko '.ko' *_dlkm.ko
and
for file in *; do mv $file audio_$file; done;

There will be one extra audio module, 'wcd_cpe_dlkm.ko'


This is the kernel source for the LG G8 T-Mobile (LM-G820TM) downloaded
from opensource.lge.com, specifically for the 20J build. Compatibility with
devices outside of the Tmo LG G8 and 20J build is to be determined. 
