# Basler Attributes Library #
This library supplies LabView VI's to be used to manipulate camera settings for the Basler acA2000 340kc camera. One of the reasons to not rely on a camera file is that once a camera is initialized within NI, camera attributes are no longer able to be referenced. This makes the camera file a good tool for initialization of many features, but not to change them during runtime.

The included VI's access the camera's registers via a serial signal. An overview of the registers can be reviewed in the [Camera Register Structure Access Methods](https://umich.box.com/s/d5nxfvnty6o53lrnci64) documentation. The "make_write-frame" VI is in charge of taking register address and data information and forming the final serial "frame" that gets sent to the camera. The other VI's employ this VI and use it as a base, while adding hardcoded hex strings that are accessible through simple controls (ex. enumerable).

## Adding a Feature ##
The "make_write-frame" VI handles all of the low-level data manipulations to form the serial string, so adding a new write function VI only requires knowledge of the register's address and the data to be sent. *Note that these VI's are using the HEX representation of a string value, which has been set via the right click menu.

To add a new feature VI:

1. Duplicate an existing VI within the folder (copy/paste in file folder), and make sure it is added to the project (drag it into the LabView pane).
2. Rename the VI using the existing nomenclature (accessible via right click).
3. Open the VI and change the icon text by double clicking it in the upper right side of the *Front Panel*.
4. Open the block diagram and change the *Address* value to the address of the register you wish to write to, as outline in the Basler documentation (see above). This string is being represented as a hex value, so disregard the *0x* characters (it should always be an eight-character address).
5. If you intend to use named features and an enumerator, first right click the enumerator and click *Properties*, then modify the list per your needs. Next, right click the case structure labels and click *Add Case for Every Value* (if it appears). Drop down the case structure list and be sure to delete any that are extra. The case structure controls the outgoing data. The values provided in the Basler documentation are given in decimal form, so be sure to convert them to hex, then enter the value. For most cases this will be an eight-character (32-bit, 4-byte) value, but for boolean features, it is acceptable to only use one byte (ex. *00* or 
*01*). If you do not need the case structure, just ensure that after manipulating the incoming data that it is structured as a hex string.
6. If you modified the input controls, revisit the *Front Panel* and connect the controls to the icon so they are accessible as a sub-VI.

## Writing Frames without Features ##
If you want finer control over the address and data fields, you can employ the "make_write-frame" VI by itself, and supply it with an *Address* value and *Data* value, both as hex strings. The resulting serial string is the same as if you were using a feature VI.

## Creating Serial Frames without the Tool ##
The Basler documentation offers several examples near the end of the document that explain how to compile a "frame", or serial string that communicates with the camera.

+ Frame trigger?
+ Frame trigger line?