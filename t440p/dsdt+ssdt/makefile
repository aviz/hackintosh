WORK=/tmp/work
PATCHED=$(WORK)/patched
UNPATCHED=$(WORK)/unpatched
EXTRACTED=$(WORK)/extracted

LAPTOPGIT=../../thirdparty/Laptop-DSDT-Patch

extract:
	rm -rf $(WORK)
	mkdir $(WORK)
	cp -R linux_native $(EXTRACTED)
	iasl -da $(EXTRACTED)/DSDT $(EXTRACTED)/SSDT* $(EXTRACTED)/dynamic/SSDT*
	mkdir $(UNPATCHED)
	cp $(EXTRACTED)/*.dsl $(EXTRACTED)/dynamic/*.dsl $(UNPATCHED)/
	

patch:
	rm -rf $(PATCHED)
	cp -R $(UNPATCHED) $(PATCHED)

	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_IRQ.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/graphics/graphics_Rename-PCI0_VID.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/ssdt2.dsl $(LAPTOPGIT)/graphics/graphics_Rename-PCI0_VID.txt $(PATCHED)/ssdt2.dsl
	patchmatic $(PATCHED)/ssdt6.dsl $(LAPTOPGIT)/graphics/graphics_Rename-PCI0_VID.txt $(PATCHED)/ssdt6.dsl
	patchmatic $(PATCHED)/ssdt9.dsl $(LAPTOPGIT)/graphics/graphics_Rename-PCI0_VID.txt $(PATCHED)/ssdt9.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/graphics/graphics_PNLF_haswell.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/ssdt2.dsl $(LAPTOPGIT)/graphics/graphics_PNLF_haswell.txt $(PATCHED)/ssdt2.dsl
	patchmatic $(PATCHED)/ssdt6.dsl $(LAPTOPGIT)/graphics/graphics_PNLF_haswell.txt $(PATCHED)/ssdt6.dsl
	patchmatic $(PATCHED)/ssdt9.dsl $(LAPTOPGIT)/graphics/graphics_PNLF_haswell.txt $(PATCHED)/ssdt9.dsl

	#patchmatic $(PATCHED)/$(GFXssdt).dsl patches/hdmi_audio.txt $(PATCHED)/$(GFXssdt).dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/usb/usb_7-series.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_WAK2.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_OSYS.txt $(PATCHED)/dsdt.dsl
	##patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_MCHC.txt $(PATCHED)/dsdt.dsl
	##patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_HPET.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_RTC.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_SMBUS.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_Mutex.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_PNOT.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_IMEI.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/battery/battery_Lenovo-Ux10-Z580.txt $(PATCHED)/dsdt.dsl
	#patchmatic $(PATCHED)/ssdt6.dsl $(LAPTOPGIT)/graphics/graphics_Rename-GFX0.txt $(PATCHED)/ssdt6.dsl	

compile:
	iasl $(PATCHED)/*.dsl 
