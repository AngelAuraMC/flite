# Android Native Flite builds

This directory contains the `Makefile` for building flite libraries as stripped `.so` files in addition to `.a` files.
> Note that the `.so` files will not have any version suffixes (`.so.X.X`) due to android just not being able to use that inside jniLibs.
This is primarily intended for [Amethyst-Android](https://github.com/AngelAuraMC/Amethyst-Android) however if you have an applicable use case, feel free to use this.

If you are looking to implement flite as part of a native android application, please refer to the repository below.
https://github.com/happyalu/Flite-TTS-Engine-for-Android
