# A Swift Script for Detecting Silence in Audio Files: Made with Reactive Programming in RxSwift

Detecting silence in audio files is an essential capability to ensure correct audio processing. When handling large numbers of files, this can be a cumbersome and time-consuming task to perform manually. I've written a script in Swift to automate this job. It uses `ffmpeg` as a subprocess along with its `silencedetect` filter. I've written the script using Reactive Programming in Swift 3 and RxSwift. It's capable of relatively fast, recursive scanning of huge numbers of files given a single starting point while reliably reporting all of its results to the console.

This project is somewhat experimental since Mac command line tools do not yet have full support for linking external frameworks. The libraries for Swift still cannot be statically linked. Anything else containing Swift code cannot be built as a static framework. If a framework containing Swift code is linked, an external source of the Swift libraries must be provided. Thus, there is a high barrier to writing Swift scripts with RxSwift, but it is one that can be overcome.

I will cover more aspects of Reactive Programming in a future tutorial.

## Usage

Usage for the script is:

    $ detectSilence ${ANY_VALID_BASE_PATH} 2>/dev/null

Here is a sample of the output:

    Silence found in file:///Audio-Files/2017-Mar-15/01.flac
        🚩 start 1894.99, end 1283.66, duration 2.02633
    Silence found in file:///Audio-Files/2017-Mar-17/02.flac
        start 437.04, end 437.603, duration 0.563469
    Silence found in file:///Audio-Files/2017-Mar-18/01.flac
        start 479.881, end [none], duration [none]
    Silence found in file:///Audio-Files/2017-Mar-19/01.flac
        🚩 start 2081.81, end 2088.02, duration 6.20592

The red flags indicate silences that have exceeded a given duration. There are also silences that are detected that have no end or no duration. These are likely false positives but they are shown for the sake of completeness. The level at which audio is considered silence is set by a noise floor variable in units of dB in the script.

## Installation

### Dependencies

The additional dependencies for this script beyond Xcode 8 are:

* [RxSwift](https://github.com/ReactiveX/RxSwift) (installed by [Carthage](https://github.com/Carthage/Carthage))
* [ffmpeg](https://trac.ffmpeg.org/wiki/CompilationGuide/MacOSX) with the `silencedetect` filter

Installation of RxSwift is accomplished with:

	$ carthage update

### Building

Build the project with Xcode. Show the Products folder in the Finder from Xcode. Copy the contents of the Products folder to a location of your choice. The `detectSilence` binary is needed along with the RxSwift.framework directory and its contents. The detectSilence.swiftmodule directory is not needed because the Swift libraries are accessed from the toolchain (the command-line tools) of Xcode. This is the external source mentioned in the introduction.

That concludes the installation process. Once installed, the compiled script can be accessed as a normal command. 

## Conclusion

RxSwift for scripting is a probably an uncommon idea but, then again, Swift for scripting is still catching on, too. This project is a demonstration of the possibilities of both while providing a practical tool that can be enjoyed now.

---

## Release Notes

* v1.0.0 First release verified with Xcode 8.3, Swift 3.1 and RxSwift 3.3.1.
* v1.0.1 Made sure that the presence of variable width encoded characters do not affect text matches.
* v1.0.2 Handled negative numbers. Fixed output to show only when duration is available.

## Repositories

The script has an open-source MIT license and repository links are here:

* [Github](https://github.com/ikiapps/detectSilence)
* [Bitbucket](https://bitbucket.org/ikiapps/detectsilence)
