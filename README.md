# LSFM Autofluorescence Subtractor
This is a FIJI plugin created in 2020 to subtract an autofluorescent channel from a signal channel in Light Sheet Data. However, it should work with any fluorescence images. Was established with a UMII from LaVision Miltenyi Biotec. 

The main idea of this plugin is simple:

![grafik](https://user-images.githubusercontent.com/90180771/181046914-7cc0e2b7-ba51-4730-8915-eca2507788c3.png)

The subtraction is carried out either directly, or more importantly by an intensity adjustment of the Autofluorescent (AF) signal:

![grafik](https://user-images.githubusercontent.com/90180771/181050463-ef38f719-ccc1-481b-a654-7933b152363a.png)

A roi box is drawn on an autofluorescent structure which is used for automatic calculation of the intensity difference between Image 1 and Image 2.

**Important:** Note that you can bring in false positives with this technique if you have a lot of noise in your images. Example here:

![grafik](https://user-images.githubusercontent.com/90180771/181046547-c5f2e2c7-3660-4707-862a-aa7741c77e99.png)

## Example Results

**Bone:**

![grafik](https://user-images.githubusercontent.com/90180771/181058626-7896ebba-c083-4604-b56d-5dce9308f0f2.png)

(Raw data left, subtracted right)

**Brain:**

![grafik](https://user-images.githubusercontent.com/90180771/181044460-57ffc6cb-0c9b-4b0f-bc66-0aa875e85a76.png)

(Raw data left, subtracted right)

## Installation
To install the Plugin, simply move the “Autofluorescence_SubtractionForLSFM-0.1.jar” File in your ImageJ Folder under “Plugins”. You have to restart ImageJ if it was already open.

## Setup

Open the LSFM data by pressing File -> Import -> Image Sequence. Select the first image. For the Ultravision II (Miltenyi Biotec) the images from the first recorded channel are named C00, for the second C01, and so on. For example, during measurement the 561 channel and the 647 channel were selected. Images obtained by 561 nm excitation include the name C00 and the ones obtained from 647 nm excitation include the name C01. Import the channels seperately by typing C00 and C01 in the „File name contains“ field.

![grafik](https://user-images.githubusercontent.com/90180771/181041436-9f29964b-ae5d-49f9-bb03-c6cb82d2ddd5.png)

After importing both channels separately the screen should look something like:

![grafik](https://user-images.githubusercontent.com/90180771/181041530-c2d35bf2-ffc2-44f5-b168-4b15a186c233.png)

**Important: Before the next step, select the window with your Signal by pressing on it.**
Then select Process -> LSFM Autofluorescence Subtractor:

![grafik](https://user-images.githubusercontent.com/90180771/181041818-4fbdcae2-bd66-420f-b3da-15f2f2debfbf.png)

This opens the main window:

![grafik](https://user-images.githubusercontent.com/90180771/181041878-dc2fc18c-a919-435b-a530-aa32dc0de453.png)

## Operations and finding Optimal Settings

First, select the stack containing the autofluorescence in (I). Choose a operation that fits your data (II). There are five operations possible:

**1. Simple Subtraction:**

𝑶𝒖𝒕𝒑𝒖𝒕= 𝑺𝒊𝒈𝒏𝒂𝒍−𝑨𝒖𝒕𝒐𝒇𝒍𝒖𝒐𝒓𝒆𝒔𝒄𝒆𝒏𝒄𝒆

Simply subtracts the autofluorescence image from the signal.

**2. Intensity Based Subtraction:**

𝑶𝒖𝒕𝒑𝒖𝒕= 𝑺𝒊𝒈𝒏𝒂𝒍−𝑨𝒖𝒕𝒐𝒇𝒍𝒖𝒐𝒓𝒆𝒔𝒄𝒆𝒏𝒄𝒆∗𝑰𝒏𝒕𝒆𝒏𝒔𝒊𝒕𝒚 𝑫𝒊𝒗𝒊𝒅𝒆𝒏𝒕

Requires a rectangular ROI to perform. Calculates the difference of intensity between the autofluorescence and signal in the ROI of the currently selected slice. Then adjusts the intensity of the autofluorescence and subtracts afterwards. This is the recommended operation.

**3. Divident Decision Subtraction:**

𝑶𝒖𝒕𝒑𝒖𝒕=(𝑺𝒊𝒈𝒏𝒂𝒍−𝑨𝒖𝒕𝒐𝒇𝒍𝒖𝒐𝒓𝒆𝒔𝒄𝒆𝒏𝒄𝒆∗𝑰𝒏𝒕𝒆𝒏𝒔𝒊𝒕𝒚 𝑫𝒊𝒗𝒊𝒅𝒆𝒏𝒕) 𝒊𝒇> 𝑻𝒉𝒓𝒆𝒔𝒉𝒐𝒍𝒅

Requires a rectangular ROI to perform. Similar to “Intensity Based Subtraction” with two main differences: It only subtracts pixels above the signal times the value that was entered in (II). If the resulting value is below the threshold (III), the value is set to zero. Every pixel that was not in range for (II) is also set to zero.

**4. Multiplication Subtraction**

𝑶𝒖𝒕𝒑𝒖𝒕= 𝑺𝒊𝒈𝒏𝒂𝒍−𝑨𝒖𝒕𝒐𝒇𝒍𝒖𝒐𝒓𝒆𝒔𝒄𝒆𝒏𝒄𝒆∗𝑽𝒂𝒍𝒖𝒆(𝑽)

Requires a value entered in (V). Multiplies the autofluorescence times (V) and subtracts it from the signal image. This operation can also be used to reproduce Images from the preview function (explained in depth later).

**5. Filtered Subtraction**

𝑶𝒖𝒕𝒑𝒖𝒕= (𝑺𝒊𝒈𝒏𝒂𝒍−𝑨𝒖𝒕𝒐𝒇𝒍𝒖𝒐𝒓𝒆𝒔𝒄𝒆𝒏𝒄𝒆∗𝑽𝒂𝒍𝒖𝒆(𝑽)) 𝒊𝒇>𝑻𝒉𝒓𝒆𝒔𝒉𝒐𝒍𝒅

Requires a value entered in (V). Combination of DDS (3.) and MS (4.). Multiplies the autofluorescence times (V) and subtracts it from the signal image. The pixel will only be transferred into the new image if the result is higher than the value entered in (IV). This operation might provide less white noise in the background but it might falsify the data. Hence, it is currently not recommended to use this operation.

Note the DDS (3.) and FS (5.) are currently included only for testing.

This Plugin also allows to crop the stack before the final subtraction (i.e. pressing ok). This is done by entering the desired slice numbers in (VI) and (VII). If a zero is entered, the images will not be cropped and every image in the stack is subtracted. Note, this feature is not used during the preview. In many cases due to the thickness of the sample, a part of the recorded LSFM data is unfocused or does not contain the region of interest. The main function behind this feature, is to remove these parts facilitating any subsequent analysis and increase computing times.

The “Brightness Adjustment” feature (VIII) can be selected, to automatically enhance brightness and contrast upon subtraction. This can be undone manually by selecting Image -> Adjust -> Brightness / Contrast -> Reset. This feature is intended to make the results after subtraction more clear. However, since this might falsify data after exporting or saving, it is recommended to only select this feature when using the preview function (IX) explained in the next section. An example for the Brightness Adjustment can be seen in the following figure.

![grafik](https://user-images.githubusercontent.com/90180771/181042514-4e8f51d5-f5ea-4fba-bd3e-a0c243506977.png)

The preview checkbox (IX) returns a single image of the current parameters and selected slice. Due to technical limitations, it is currently required to turn the checkbox on and off to prevent errors. The resulting image is opened in a new window while the parameters are written in the title. This allows to check multiple parameters and being able to directly compare them.

**Important:** It is possible to reproduce an earlier result of an “intensity based subtraction” without redrawing the ROI. Select “Multiplication Subtraction” in (II) and enter the value of the divident (written in the window title) in (V). The result will be identical.

**Important:** The plugin currently does not take over meta data. You will have to add them afterwards again or use a global calibration in FIJI.
