# LoadingScreenManager
## A loading screen / slideshow mod for Kerbal Space Program 1.2.2
### By @paulprogart

LoadingScreenManager (LSM) is a simple plugin for KSP 1.2.2 that will
show a slideshow of custom images while KSP loads.

For discussion/information/support go to the
[KSP Forum Thread](http://forum.kerbalspaceprogram.com/index.php?/topic/156064-wip-122-loadingscreenmanager-show-your-own-images-while-ksp-loads/).


## Current Features (v0.9 PRERELEASE)

* Show your own images while KSP loads (PNG or JPG formats, any size)
* Show multiple random images during loading as a slideshow
* Folder is configurable within the KSP install directory (defaults to Screenshots)
* Customize the time each image shows for as well as the transition fade times
* Optionally include the default loading images in the slideshow
* Customize the length each "witty loading tip" shows for
* BONUS:  Will dump all the tips to the log if you really want to see them all


## Potential features for full release:

Roughly in order of priority.  Implementation depends on interest and (more
importantly) how many technical issues occur during prerelease.

* Pull images from multiple customizable folders
* Add your own loading tips (with option to keep/discard defaults)
* File masks (e.g. *.jpg, *.png)
* Eliminate possible repeats
* Specify a fixed list of images with an ordering, rather than just random
* Possibly show images on other in-game loading screens (e.g. building/vessel change)
* Maybe a settings screen tab (current customization is via .cfg file)


## Known issues:

* It is possible for the slideshow to repeat an image - probably not desired but not a bug
* When KSP changes from asset to part load, the progress bar will jump - just a 
  cosmetic issue; loading is not affected.
* Also when load changes, the tip will always change regardless of timing.


## Licence

Licence is **CC BY-NC-SA** - see here for more:  https://creativecommons.org/licenses/by-nc-sa/4.0/


## Installation

To use the mod in KSP, download the appropriate zip file from the `Release` folder:

* [32-bit Windows (x86)](Release/v0.9/KSP-LSM-0-90-x86.zip)
* [64-bit Windows (x64)](Release/v0.9/KSP-LSM-0-90-x64.zip)
  * Use 64-bit version only if running 64-bit KSP (`KSP_x64.exe`).
  * If there are issues, try the x32 version.
* **NOTE**: Non-Windows users will have to build from source (see [below](#source)).

Unzip this to `<KSP Install Folder>/GameData`.  It will create its own
folder within, and KSP should pick it up automatically the next time it's run.

**NOTE**: **Only KSP 1.2.2 is officially supported.**  I have no idea if the appropriate API calls are
available in earlier versions.  You can try, but if it doesn't work you're out of luck.


## Configuration

When first run, LSM will create a `LoadingScreenManager.cfg` within the
install folder with default settings.  The location may vary by OS but it should be `PlugInData\LoadingScreenManager\LoadingScreenManager.cfg`.

The configuration settings are as follows:

* **`debugLogging`** - If `True`, will include extra diagnostic info in the log.
  * **_PLEASE LEAVE THIS ENABLED FOR THE PRERELEASE_**
    * At least until you know the mod works reliably...
  * **NOTE**: KSP output logging must be turned on.
* **`dumpScreens`** - If `True`, dumps a list of installed screens to the log.  Mainly for development use.
  * **NOTE**: `debugLogging` must be `True` for this to work!
* **`dumpTips`** - If `True`, dumps a list of the "loading tips" for each screen to the log.
  * This is a borderline game **_SPOILER_** so it is not enabled by default.
  * **NOTE**: `debugLogging` must be `True` for this to work!
* **`screenshotFolder`** - The folder to pull images from, relative to the KSP install folder.  Include the trailing slash.
  In the generated config, this will be `Screenshots/`.
  * Files in subfolders will also be included.
  * **NOTE**: Folder should not contain anything other than screenshots!
    If so you will see a bunch of errors and may see blank slides.
  * If OS and user permissions allow (note LSM only reads image files), it may
    be possible to use folders outside of the KSP install folder using `../`,
    but **_this is not officially supported_**.
* **slidesToAdd** - Number of slides to add.  Performance tuning setting that normally does not need to be changed.
  * This value + 1 is the maximum number of slides KSP will show.
  * It's important to note that the images only loaded once, they are not copied.
  * To show only 1 slide (like current KSP), set this to 0.
  * Actual # of slides shown depends on loading time and timing settings.
  * The approximate maximum slideshow length (in seconds) will be:
    * `(slidesToAdd + 1) * (fadeInTime + displayTime + fadeOutTime)`
  * If this is too low, the image will be blank for the last portions of loading.
* **includeOriginalScreens** - If `True`, includes the provided KSP screens in the slideshow.
  * Whether they are actually shown or not is random.
* **runWithNoScreenshots** - Normally LSM won't touch anything if there are no screenshots in the
  folder, which is mainly for new KSP players (or empty installations).  Set to
  `True` to override this behaviour.
  * **NOTE**:  If you want a blank screen during loading, use an empty `screenshotFolder`, set this to `True`,
    and turn `includeOriginalScreens` off, but note _this will cause an error (non-fatal) in KSP_!
* **displayTime** - Time in seconds each slide is to be shown.
  * If you only want one image, set this to a really long value.
* **fadeInTime** - Time in seconds for the fade-in transition to a new slide.
* **fadeOutTime** - Time in seconds for the fade-in transition after a slide.
* **tipTime** - Time in seconds each "witty loading tip" is to remain showing.
  * Lower this time to see more tips, raise it to see fewer.

**_IMPORTANT_: It is _not_ possible via configuration to alter or adjust the
developer logo loading screen that is shown initially.  Such behaviour is not
permitted by KSP forum rules.  _Don't request this ability, and don't submit
code changes that do it.  Full stop._**


## Source

If you want to build from source, be aware of the following:

* Project & solution files are provided for Visual Studio 2015 only.
  * Before building for the first time, go into the project properties
  `Build Events` tab and update the folder in the Post-build event to your own
  KSP GameData folder, otherwise you will get an error after a successful build.
  * Also you will need to fix the KSP and Unity DLL references.
  * Build configurations are provided for both x86 and x64.
* As there's only one code file, it's easy enough to create a new project (or
equivalent in other dev environments) if needed, assuming you are familiar
with building KSP Add-Ons already.
* **NOTE**: LSM is written using C# 6.0, which is not supported by older
  versions of Visual Studio or by other older editors.
* **NOTE**: LSM contains ReSharper annotation attributes, which on Windows are
  provided (accidentally IMO) by UnityEngine, so I haven't included them.  Not
  sure if they are present on other platforms, but if not you can just remove
  them to get LSM to build.  Or maybe get the JetBrains.Annotations NuGet
  package, though I haven't tried that.
