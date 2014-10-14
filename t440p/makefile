WORK=/tmp/work
PATCHED=$(WORK)/patched
UNPATCHED=$(WORK)/unpatched
EXTRACTED=$(WORK)/extracted
EFIDIR=/Volumes/EFI
EFIVOL=/dev/disk2s1
IASLFLAGS=-vr -w1
LAPTOPGIT=../thirdparty/Laptop-DSDT-Patch
PRODUCTS=$(PATCHED)/dsdt.aml $(PATCHED)/ssdt2.aml $(PATCHED)/ssdt6.aml $(PATCHED)/ssdt9.aml 

all: $(PRODUCTS)

$(PATCHED)/dsdt.aml: $(PATCHED)/dsdt.dsl
	iasl $(IASLFLAGS) -p $@ $<
	
$(PATCHED)/ssdt2.aml: $(PATCHED)/ssdt2.dsl
	iasl $(IASLFLAGS) -p $@ $<

$(PATCHED)/ssdt6.aml: $(PATCHED)/ssdt6.dsl
	iasl $(IASLFLAGS) -p $@ $<

$(PATCHED)/ssdt9.aml: $(PATCHED)/ssdt9.dsl
	iasl $(IASLFLAGS) -p $@ $<

extract:
	rm -rf $(WORK)
	mkdir $(WORK)
	cp -R dsdt+ssdt/linux_native $(EXTRACTED)
	iasl -da $(EXTRACTED)/DSDT $(EXTRACTED)/SSDT* $(EXTRACTED)/dynamic/SSDT*
	mkdir $(UNPATCHED)
	cp $(EXTRACTED)/*.dsl $(EXTRACTED)/dynamic/*.dsl $(UNPATCHED)/
	for f in $(UNPATCHED)/*; do mv "$$f" "`echo $$f | tr "[:upper:]" "[:lower:]"`"; done
	rm -f $(UNPATCHED)/ssdt7.dsl
	rm -f $(UNPATCHED)/ssdt4.dsl
	

patch:
	rm -rf $(PATCHED)
	cp -R $(UNPATCHED) $(PATCHED)

	patchmatic $(PATCHED)/dsdt.dsl $(U430GIT)/patches/syntax_dsdt.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(U430GIT)/patches/cleanup.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(U430GIT)/patches/remove_wmi.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/ssdt2.dsl $(U430GIT)/patches/cleanup.txt $(PATCHED)/ssdt2.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(U430GIT)/patches/keyboard.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(U430GIT)/patches/audio.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(U430GIT)/patches/sensors.txt $(PATCHED)/dsdt.dsl

	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_IRQ.txt $(PATCHED)/dsdt.dsl

	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/graphics/graphics_Rename-PCI0_VID.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/ssdt2.dsl $(LAPTOPGIT)/graphics/graphics_Rename-PCI0_VID.txt $(PATCHED)/ssdt2.dsl
	patchmatic $(PATCHED)/ssdt6.dsl $(LAPTOPGIT)/graphics/graphics_Rename-PCI0_VID.txt $(PATCHED)/ssdt6.dsl
	patchmatic $(PATCHED)/ssdt9.dsl $(LAPTOPGIT)/graphics/graphics_Rename-PCI0_VID.txt $(PATCHED)/ssdt9.dsl

	patchmatic $(PATCHED)/ssdt6.dsl $(LAPTOPGIT)/graphics/graphics_PNLF_haswell.txt $(PATCHED)/ssdt6.dsl
	patch $(PATCHED)/ssdt6.dsl < dsdt+ssdt/patches/fix_external_DOS.patch

	#patchmatic $(PATCHED)/$(GFXssdt).dsl patches/hdmi_audio.txt $(PATCHED)/$(GFXssdt).dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/usb/usb_7-series.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_WAK2.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_OSYS.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_MCHC.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_HPET.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_RTC.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_SMBUS.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_Mutex.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_PNOT.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/system/system_IMEI.txt $(PATCHED)/dsdt.dsl
	patchmatic $(PATCHED)/dsdt.dsl $(LAPTOPGIT)/battery/battery_Lenovo-T440p.txt $(PATCHED)/dsdt.dsl

install: extract patch $(PRODUCTS) mount
	cp config.plist $(EFIDIR)/EFI/CLOVER/config.plist
	cp $(PATCHED)/dsdt.aml $(EFIDIR)/EFI/CLOVER/ACPI/patched
	cp $(PATCHED)/ssdt2.aml $(EFIDIR)/EFI/CLOVER/ACPI/patched/ssdt-2.aml
	cp $(PATCHED)/ssdt6.aml $(EFIDIR)/EFI/CLOVER/ACPI/patched/ssdt-6.aml
	cp $(PATCHED)/ssdt9.aml $(EFIDIR)/EFI/CLOVER/ACPI/patched/ssdt-9.aml
	rm -rf $(EFIDIR)/EFI/CLOVER/kexts/10.9/T440pYekkiBluetoothInjector.kext
	unzip kext/T440pYekkiBluetoothInjector.kext.zip -d $(EFIDIR)/EFI/CLOVER/kexts/10.9/
	unzip kext/VoodooPS2Controller.kext.zip -d $(EFIDIR)/EFI/CLOVER/kexts/10.9/

umount:
	diskutil unmount $(EFIDIR)
	if [ -d "/Volumes/EFI" ]; then rmdir /Volumes/EFI; fi

mount:
	if [ ! -d $(EFIDIR) ]; then mkdir $(EFIDIR); fi
	if [ ! -d $(EFIDIR)/EFI ]; then diskutil mount -mountPoint /Volumes/EFI $(EFIVOL); fi
