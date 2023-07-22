# QuickSync4Linux
It is annoying that companies always forget to implement their software for the most important operating system. This is a minimal implementation of the Gigaset QuickSync software for Linux.

The communication with the device is based on AT commands over a USB/Bluetooth serial port. For file transfer, the device is set into Obex mode.

## Hardware Setup
Make sure your user is in the dialout group in order to access the serial port.
```
sudo usermod -aG dialout <username>
# logout and login again to apply group membership
```

## Usage
First, find out the correct serial port device. After connecting, a serial port like `/dev/ttyACM0` (USB on Linux), `/dev/tty.usbmodem` (USB on macOS) or `/dev/rfcomm0` (Bluetooth) should appear. `/dev/ttyACM0` is used by default. If your device differs, you can use the `--device` parameter. More information regarding Bluetooth serial connections can be found [here](https://gist.github.com/0/c73e2557d875446b9603).

Then, you can use one of the following commands:
```
# read device metadata
./quicksync.py info

# read device contacts and print VCF to stdout (use --file to store it into a file instead)
./quicksync.py getcontacts

# create new contacts on device from vcf file
./quicksync.py createcontacts --file mycontacts.vcf

# overwrite a contact with given luid 517 (the luid can be found in `getcontacts` vcf output)
./quicksync.py editcontact 517 --file mycontact.vcf

# delete contact with luid 517 from device
./quicksync.py deletecontact 517

# show files on device
./quicksync.py listfiles

# download file "/Pictures/Gigaset.jpg" from device into local file "gigaset.jpg"
./quicksync.py download "/Pictures/Gigaset.jpg" --file gigaset.jpg

# upload local file "cousin.jpg" into "/Clip Pictures/cousin.jpg" on device
# your image size should match the screen/clip size which can be found by the `info` command - the device will crash and reboot otherwise!
./quicksync.py upload "/Clip Pictures/cousin.jpg" --file cousin.jpg

# delete file "/Clip Pictures/cousin.jpg" on device
./quicksync.py delete "/Clip Pictures/cousin.jpg"

# start a call
./quicksync.py dial 1234567890
```

For debug purposes and reporting issues, please start the script with the `-v` parameter and have a look at the serial communication.

## VCF Structure
The Gigaset devices expect a VCF like the following example:
```
BEGIN:VCARD
VERSION:2.1
X-IRMC-LUID:769
N:Last Name;First Name
TEL;HOME:+49123456789
TEL;CELL:+49456789123
TEL;WORK:+49789123456
BDAY:2020-01-01T09:00
END:VCARD
```

And with special chars encoded as Quoted Printable:
```
BEGIN:VCARD
VERSION:2.1
X-IRMC-LUID:1099
N;ENCODING=QUOTED-PRINTABLE;CHARSET=UTF-8:|\\=C2=A7\;;=C3=A4=C3=B6=C3=BC=
=C3=9F
TEL;HOME:+49123
END:VCARD
```

## Tested Devices
Please let me know if you tested this script with another device (regardless of whether it was successful or not).

- Gigaset S68H (Bluetooth working, no USB port)
- Gigaset CL660HX (USB working, no Bluetooth)
- Gigaset SL450HX (USB + Bluetooth working)
- Gigaset S700H PRO (USB + Bluetooth working)

## Support
If you like this project please consider making a donation using the sponsor button on [GitHub](https://github.com/schorschii/QuickSync4Linux) to support further development. If your financial resources do not allow this, you can at least leave a star for the Github repo.

Furthermore, you can hire me for commercial support or adjustments and support for new devices. Please [contact me](https://georg-sieber.de/?page=impressum) if you are interested.
