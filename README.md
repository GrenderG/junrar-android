Junrar-Android
==============

**Current version forked from** [inorichi/junrar-android](https://github.com/inorichi/junrar-android)

* Removed unused classes and imports.
* Added support for Jitpack builds.
* Now `UnrarCallback` is finally implemented.

**Original junrar-android forked from** [Albertus82/JUnRAR](https://github.com/Albertus82/JUnRAR)

* Removed commons-logging dependency and VFS support.
* Built with Gradle.

#### ~~Plain Java~~ Android UnRAR Utility. Original project by [Edmund Wagner](https://github.com/edmund-wagner/junrar) (formerly on [SourceForge](https://sourceforge.net)).

* **Fixed critical bug in the extraction algorithm that caused wrong bytes in the extracted files in some cases** (see [this issue](https://github.com/edmund-wagner/junrar/issues/36), thanks to [Roy Damman](https://github.com/RDamman)).
* Fixed major bug that prevented the extraction of files larger than 2.147.483.647 bytes (see [this pull request](https://github.com/junrar/junrar/pull/3), thanks to [acc15](https://github.com/acc15)).
* Fixed minor bug that caused inaccurate file times (see [this pull request](https://github.com/edmund-wagner/junrar/pull/20), thanks to [Luke Quinane](https://github.com/tmyroadctfig)).
* Multi-volume archives are supported in both old and new flavours (`.rXX` & `.partX.rar`).
* **Encrypted and password protected archives are not yet supported.**
* **RAR 5 format is not supported**.


## Prerequisites

Add this in your root `build.gradle` file (**not** your module `build.gradle` file):

```gradle
allprojects {
	repositories {
		...
		maven { url "https://jitpack.io" }
	}
}
```

## Dependency

Add this to your module's `build.gradle` file (make sure the version matches the JitPack badge above):

```gradle
dependencies {
	...
	compile 'com.github.GrenderG:junrar-android:1.0.2'
}
```

## Basic usage

```java
new RarExtractor().extractArchive("path/to/rar", "destination/path");
```

### Using `UnrarCallback`

```java
new RarExtractor().extractArchive("path/to/rar", "destination/path", new UnrarCallback() {
            
    // Checks if the next volume is ready to be processed. (You can ignore this if you only
    // want to know the extraction progress).
    @Override
    public boolean isNextVolumeReady(Volume volume) {
        return false;
    }
                
    // Once a file of the RAR is decompressed, this method is called. (Size is in bytes).
    @Override
    public void volumeProgressChanged(long current, long total) {
        Log.d("Progress", (current * 100) / total + "% - " + current + " out of " + total);
    }
});
```
