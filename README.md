RtkGps+
=======

RTKLIB rtknavi port on android.


### Features

#### [rtklib][rtklib] features:

* Version pre 2.4.2p9
* GPS, GLONASS, Galileo, QZSS, BeiDou and SBAS Navigation systems
* Single, DGPS/DGNSS, Kinematic, Static, Moving-Baseline, Fixed,
  PPP-Kinematic, PPP-Static and PPP-Fixed positioning modes.
* RTK and PPP integer ambiguities resolution (PPP AR is experimental)
* GNSS formats: 
  RINEX 2.10,2.11,2.12 OBS/NAV/GNAV/HNAV/LNAV/QNAV, RINEX 3.00,3.01,3.02
    OBS/NAV,RINEX 3.02 CLK,RTCM ver.2.3,RTCM ver.3.1 (with amendment 1-5),
    RTCM ver.3.2, BINEX, NTRIP 1.0, NMEA 0183, SP3-c, ANTEX 1.4, IONEX 1.0,
    NGS PCV and EMS 2.0.
* Proprietary protocotols: 
  NovAtel: OEM4/V/6,OEM3,OEMStar,Superstar II, Hemisphere: Eclipse,Crescent,
    u-blox: LEA-4T/5T/6T, SkyTraq: S1315F, JAVAD GRIL/GREIS, Furuno
    GW-10-II/III, NVS NV08C BINR, SiRF III/IV, and Trimble RT17.
* TCP/IP, NTRIP, local log file
* Support for multiple geoids (see explanation)

#### Android features:

* Test mode for obtaining positions from internal GPS (without any RTKLIB function)
* Bluetooth communication
* USB OTG communication with speed/parity/stop configuration (ACM, PL2303 chips and alpha for FTDI chips)
* SiRF IV protocol (experimental)
* Show altitude in status view if Height/Geodetic is choosen
* Send mock location to other applications if the checkbox is ticked in the solution option screen. (Not working yet with apps using Google Maps api)
* Upload Log/Solution to Dropbox if Sync to Dropbox is ticked in the FileSettings screen. (the sync occurs after the stop of the server)
* Ability to zip files before uploading to Dropbox for reducing the data volume.
* Generate a GPX track (Output view/GPX Track tab)
* Show UTM coordinates in solution view
* Support for french Geoportail maps (cadastral parcels, roads and satellite) - needs api license key see src/gpsplus/rtkgps/geoportail/License.java.sample 

#### Proj4
* Due to multiple issues (error in projection and lacks of features) Proj4J was removed and C lib proj4 was added
* All conversion are done with proj4 4.8.0
* French projections are done with IGN certified method, Lambert II extended is computed with IGN certified grid ntf_r93.gsb
* One custom proj4 specification string can be specified (take care of exact syntax).  
* all standard proj4 formats and grids are included, all grids from http://download.osgeo.org/proj/proj-datumgrid-1.5.zip are also included. In other words you have access to this external files: alaska, chenyx06etrs.gsb, conus, epsg,esri, esri.extra, FL, GL27, hawaii, IGNF, MD, nad27, nad83, ntf_r93.gsb, ntv1_can.dat, null, nzgd2kgrid0005.gsb, other.extra, proj_def.dat, prvi, stgeorge, strlnc, stpaul, TN, WI, WO, world
* If you need another specific "free" or freely distributable grid or definition file, do not hesitate to drop me an email.

#### Geoids
* you can select different geoid model in "Solution Option"/Geoid model but except for embedded model (EGM96 1°x1°)  
     
  After receiving some emails for asking me how to install the geoid model, I added a new menu "Tools" in this menu you can automatically download and install
  some geoid models. Take attention to the size! I think you may prefer to download this models via a Wifi connection rather than with your data plan!  
    

  you will need to place the corresponding model in the RtkGps storage (probably /storage/sdcard0/RtkGps)  
  the model format is RTKLIB dependent, check jni/RTKLIB/src/geoid.c  
  the model file MUST be one of:  
  ```
    EGM96_M150.geoid  
    EGM2008_M25.geoid  
    EGM2008_M10.geoid  
    GSI2000_M15.geoid
    RAF09_M15x20.geoid  
  ```
  
  for example if you want to use EGM2008 2.5'x2.5' model you need:
  ```
    1.  download from National Geospatial Intelligence Agency the model  
  			  on august 2014 file can be found http://earth-info.nga.mil/GandG/wgs84/gravitymod/egm2008/Small_Endian/Und_min2.5x2.5_egm2008_isw=82_WGS84_TideFree_SE.gz   
    2.  extract the Und_min2.5x2.5_egm2008_isw=82_WGS84_TideFree_SE file and rename it EGM2008_M25.geoid (149Mo)   
    3.  place this model in RtkGps storage path (where all the trace and log go), on my tools it is /storage/sdcard0/RtkGps/
  ```

  for example if you want to use IGN RAF09 1.5'x2.5' model you need:
  ```
    1.  download from IGN wich is the french national geographical institute the model  
  			  on august 2014 file can be found http://geodesie.ign.fr/contenu/fichiers/documentation/grilles/metropole/RAF09.mnt   
    2.  rename it RAF09_M15x20.geoid    
    3.  place this model in RtkGps storage path (where all the trace and log go), on my tools it is /storage/sdcard0/RtkGps/
  ```
   
  if you do not have the correct model or if model is inconsistent it will not show an error, rather than it will use the EGM96 1°x1° embedded model or it use ellipsoidal height.
  				
#### Precise ephemeris
If you need precise ephemeris you have 2 ways for using them:  
* Manualy: In the correction tab you select "File" and type SP3 , in the filename you put the filename of the file you provide in RtkGps directory (ending with .SP3)  
* Automatically: When the server is running, hit the "Tools" menu, here you have an option to download and inject automatically the latest ultra-rapid ephemeris from IGS or simply inject them if you already have the good file.  
  
#### Building on Windows
Android is Unix so it is easier to build under an *nix system. Personnaly I use MacOSX but it can be done under WIndows  
You need a correctly installed ndk (under windows I use ndk-r9d), a correctly Installed ADT (I use x86_64-20140702)  
Also you will need a working Cygwin installation with make, gcc-core gcc-c++ bash at least 
Define ANDOID_NDK and ANDROID_SDK variable to there correct path, and add ANDROID_NDK path in PATH  
copy RtkGps\jni\simonlynen_android_libs\lapack\jni\clapack\INCLUDE\*.h to:  
  RtkGps\jni\simonlynen_android_libs\lapack\jni\clapack\SRC  
  RtkGps\jni\simonlynen_android_libs\lapack\jni\clapack\INSTALL  
  RtkGps\jni\simonlynen_android_libs\lapack\jni\clapack\BLAS\SRC  
This is a workaround for the symlinks  
You also need to deactivate the use of lapack since it cannot be build under windows  
For that please modify RtkGps/jni/rtklib.mk and Android.mk for removing LAPACK flag and clapck module import  
now under a cygwin terminal move to your RtkGps directory and build  
Under Eclipse be sure that you do not set to build the native library since it fails  
  
#### Translations
Contributors are welcomed for translating RTKGPS+, the translation can be easily managed on [Crowdin](https://crowdin.com/project/gpsplusrtkgps/invite).   
You can freely create a translator account and with it you will be able request for a new translation.  
I already made this translations:
* English (source language)
* French  
* Chinese (fully translated by Yong Zhang)
* Spanish (need some corrections)


### Requirements

* android > 4.0
* Bluetooth or USB OTG GPS receiver supported by rtklib
* Allow mock locations in your device developer settings (optional but required for sending mock locations)
* A Dropbox account for uploading log/solution. (you will be asked for authorizing this app by Dropbox)

###### Download original version from Alexey Illarionov.
[original version](https://github.com/illarionov/RtkGps)

### Download alpha version (1.0apha20) from Google Play.

![qr](https://raw.githubusercontent.com/eltorio/RtkGps/master/qr_googleplay.png)

[Google play](https://play.google.com/store/apps/details?id=gpsplus.rtkgps)


[rtklib]: http://www.rtklib.com/

###### Binary distribution

* the latest apk wich is maybe very instable can be found in bin directory, but prefer the Google Play version