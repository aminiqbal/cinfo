# A Portable Eclipse for C [Windows x64 ONLY]

## Credits
* C, GCC, MinGW authors (https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/).
* Eclipse Foundation & Eclipse IDE authors (https://www.eclipse.org/downloads/).
* Eclispe CDT authors (https://www.eclipse.org/cdt/).

## This should be working regardless of your setup given you follow directions below:

### Set-up Directions:
* Download the ZIP file:
```
https://drive.google.com/file/d/1n6wF5Nxx6DIDg7Tvermzt5N5zFoGx2Xc/view?usp=sharing
```
* Extract it to a place of your desire (Preferably one that does not require ADMINISTRATOR privilege).
My recommendation:
```
C:/Users/YourUserName/
```
* Don't change the name of the extracted directory as required files have been preset via relative path.
* All your projects will be located in the main (extracted) directory's subdirectory named _projectdata_. __Don't change this__.
* Don't __import__ your previous files: create project as shown below, create the .c files and copy/ paste the CODE from previous files.
* __DO NOT SIMPLY LAUNCH ECLIPSE BY BROWSING INTO THE ZIP FILE, YOU HAVE TO EXPLICITLY EXTRACT THE ZIP FILE.__

### IF THERE ARE ANY ISSUES STARTING ECLIPSE, RUN THE ```eclipsefix.bat``` FILE.

### How to use:
* Create C project: ```File -> Project... -> C/C++ -> C Project```
  * Project type: Empty Project
  * Toolchains: MinGW GCC
  * Both Debug & Release checked.
  * Finish
* Right-click on your project folder on left panel (Project Explorer) and create a new file; name it accordingly but:
  * Make sure you include the proper extension ```.c``` or ```.h``` after the name. For example: ```testfile.c``` or ```testheader.h```
* Right click on project then click on ```Build```
* Right click on project then click ```Run -> Run as Local C/C++ Application```

### How to export a project:
* Right-click on your project & click on Export.
* Expand General label and select ```Archive File```
* Press ```Next```
* Make sure only your target project folder is checked at the left panel of the pop-up window.
* In the options section of the poop-up window, only these items should be selected:
  * ```Save in zip format```
  * ```Create directory structure for files```
* In the options section of the poop-up window, only these items should be checked:
  * ```Compress the contents of the file```
* Select a proper destination path and click ```Finish```
