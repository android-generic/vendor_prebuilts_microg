Adapted from https://github.com/CalyxOS/platform_prebuilts_calyx_microg 

# MicroG Prebuilts:

- MicroG GMS - GmsCore
- MicroG GSF - GsfProxy
- MicroG FakeStore - FakeStore
- DejaVuLocationService
- MozillaNlpBackend
- NominatimNlpBackend

Changes Made: 

- worked into Android-Generic Project and https://github.com/BlissRoms-x86/vendor_foss
- added patches required
- created this README.md

## Cloning Repos

Clone into vendor/prebuilts/microg

	git clone https://github.com/BlissRoms-x86/vendor_foss vendor/foss
	git clone https://github.com/android-generic/vendor_prebuilts_microg vendor/prebuilts/microg

## Setting Up

Add this inherit to your device tree:

	# foss apps
	$(call inherit-product-if-exists, vendor/foss/foss.mk)
	
Download Latest FOSS apps: 

**If no option is passed, it will prompt you to make a choice**

- 1 = x86_64 ABI **default options**
- 2 = arm64-v8a ABI
- 3 = x86 ABI

Usage:

	cd vendor/foss
	bash update.sh 2

Run Patching Script:

	cd vendor/prebuilts/microg
	bash patches/autopatch.sh
	
This will patch the current projects frameworks/base & packages/apps/Settings with the required commits. 

## Troubleshooting 

If this process shows conflicts, you will need to cd into the correct directory (frameworks/base or packages/apps/Settings)
and manually apply the patch again in order to manally resolve the conflict. 

	cd frameworks/base
	git am vendor/prebuilts/microg/patches/common/frameworks/base/0001-Allow-microG-and-only-microG-to-spoof-package-signat.patch
	
Expecting that to fail initially, we then use the 'patch' command for a more verbose result

	patch -p1 < vendor/prebuilts/microg/patches/common/frameworks/base/0001-Allow-microG-and-only-microG-to-spoof-package-signat.patch
	
This will result in a .rej & .orig file for the conflicting parts. Look at those to see what it was trying to do, and look at the file it's 
referencing in the source to resolve the commit properly. When you are done, you can delete the .orig * .rej files, then continue:

	git add -A
	git am --continue
	
This should complete the commit with proper git attribution intact towards the origonal author. 
