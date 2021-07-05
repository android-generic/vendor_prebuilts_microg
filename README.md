Adapted from https://github.com/CalyxOS/platform_prebuilts_calyx_microg 

Changes Made: 

- worked into Android-Generic Project 
- added patches required
- created this README.md

Clone into vendor/prebuilts/microg

	git clone https://github.com/android-generic/vendor_prebuilts_microg vendor/prebuilts/microg
	
then run:

	cd vendor/prebuilts/microg
	bash patches/autopatch.sh

This will patch the current projects frameworks/base & packages/apps/Settings with the required commits. 

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
