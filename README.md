# OplNotes
The OPL notes I wrote in the late 90s and early 00s for the Nokia Communicators 9200, 9300 and 9500 series and Psion 5, 6 and 7 series. OPL is obviously a dead language now, but the routines can be quite useful as reference.

Read more about OPL here: https://en.wikipedia.org/wiki/Open_Programming_Language

OPL's main strength was the ease with which it could #include C++ libraries for PDA and mobile phone features such as camera, Bluetooth, IrDA, SMS, clipboard, touch-screen and much more.

dpNotes are a collection of - hopefully - useful ideas for the furtherance of OPL programming. In dpNotes we focus primarily on Symbian OS v6 (ER6) and later, though some of it was around already at v5 (ER5). A big difference between v1-v5 and v6-v7 and later is that the former was based on ASCII whereas the latter was on Unicode.

dpNote 0001 - Finding out the number of images in an MBM file
dpNote 0002 - Using the Nokia 9200 Series Communicator system fonts
dpNote 0003 - Getting the path to the location of an OPL application
dpNote 0004 - IOSEEK - documented and undocumented features
dpNote 0005 - Calculating UID checksum
dpNote 0006 - Handling of simple stacks
dpNote 0007 - Multi-dimensional arrays
dpNote 0008 - Stack of buffers with configurable size
dpNote 0009 - Getting the serial (IMEI) number of a Nokia 9200 Series Communicator
dpNote 0010 - Making the v6/S80 WINS Emulator more programmer friendly for OPL development
dpNote 0011 - Showing the amount of free memory
dpNote 0012 - Naming conventions
dpNote 0013 - Conversion between large hexadecimal and decimal numbers
dpNote 0014 - Useful POKEs and PEEKs
dpNote 0015 - Endianism
dpNote 0016 - Toolbar buttons with text only
dpNote 0017 - dpToolbar - a better Toolbar for Psion Teklogix netBook and Psion Series 7
dpNote 0018 - Loading a complete file into a buffer
dpNote 0019 - Converting long text buffers between Unicode and Ascii
dpNote 0020 - LOC function which gives correct answer for control and Unicode characters
dpNote 0021 - Character conversions
dpNote 0022 - Converting between Unicode and SCSU
dpNote 0023 - Clipboard - accurate copying and pasting of text in ER6 and ER7
dpNote 0024 - Adding a bitmap to an MBM file
dpNote 0025 - Useful IO functions and wrappers
dpNote 0026 - Predictable Pause
dpNote 0027 - Launching an application from OPL and wait until it finishes before returning
dpNote 0028 - Asynchronous event loop with inactivity timer
dpNote 0029 - Key event codes for Series 60 phones
dpNote 0030 - Returning more than one value

dpNote 0001, 2 July 2002, All OPL versions - Finding out the number of images in an MBM file
--------------------------------------------------------------------------------------------

This procedure is quite useful when the number of images in an MBM file is unknown.
Usage: NumberOfImages&=IoMbmImages&:(aMbmFileName$)

CONST KMbmFileImageCounterOffset&=&00000010
CONST KLongSize&=4

PROC IoMbmImages&:(aMbmFileName$)
LOCAL IoStatus%,hMbm%,IoMode%,Offset&,NoOfImages&
// open the image file
IoMode%=KIoModeOpen% OR KIoFormatBinary% OR KIoAccessRandom% OR KIoAccessShare%
IoStatus%=IOOPEN(hMbm%,aMbmFileName$,IoMode%)
IF IoStatus%<0
  RAISE KErrNotExists%
ENDIF
// move to the position of the offset address of the image counter
Offset&=KMbmFileImageCounterOffset&
IOSEEK(hMbm%,1,Offset&)
// read the position of the image counter
IOREAD(hMbm%,ADDR(Offset&),KLongSize&)
// move to the position of the image counter
IOSEEK(hMbm%,1,Offset&)
// read the image counter
IOREAD(hMbm%,ADDR(NoOfImages&),KLongSize&)
// close the image file
IOCLOSE(hMbm%)
// return number of images in file
RETURN NoOfImages&
ENDP

dpNote 0002, 5 July 2002, v6 Series 80 R1	- Using the Nokia 9200 Series Communicator system fonts
-------------------------------------------------------------------------------------------------

The Const.oph file for Symbian OS v6.0 does not include the font UIDs for the Nokia 9200 Series Communicator. You could add the following constants to your Const.oph file to address this:

CONST KFontLindaBold16&=         268457209 // &100054F9
CONST KFontLindaBold18&=         268457210
CONST KFontLindaBold20&=         268457211 // in CBA titles for 9210
CONST KFontLindaBold22&=         268457212
CONST KFontLindaBold24&=         268457213
CONST KFontLindaBold29&=         268457214
CONST KFontLindaBoldItalic20&=   268457215
CONST KFontLindaItalic20&=       268457216
CONST KFontLindaNarrow20&=       268457217
CONST KFontLindaNarrow24&=       268457218
CONST KFontLindaNarrow29&=       268457219
CONST KFontLindaNormal18&=       268457220
CONST KFontLindaNormal20&=       268457221 // in gIPRINT for 9210
CONST KFontLindaNormal24&=       268457222
CONST KFontLindaNormal29&=       268457223 // &10005507

CONST KFontTerminalNormal8&=     268437778 // &10000912, same in ER5
CONST KFontTerminalZoomed15&=    268437779 // &10000913, same in ER5

The fonts are now selectable with gFONT as normal.

The designations given are now taken from the 9210 screen layout document. They are slightly modified to comply with OPL coding standards.

The KFontLindaBold20& UID is particularly useful if you wish to have a consistent font on your CBA titles.

And the KFontLindaNormal20& UID is useful if you wish to create variants of gIPRINT and ALERT messages. By the way, the green frame has the RGB value KRgbMessageBorderGreen&=&008800.


