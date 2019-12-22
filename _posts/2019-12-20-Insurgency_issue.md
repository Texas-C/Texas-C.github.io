---
layout:	post
title: A small issue on Insurgency and the solution
published: true
---

# Issue decript:

Insurgency is a  Source based FPS game on Steam by `New World Interactive`. Fortunately, developer made it available on Linux.

But can not launch Insurgency in Steam after system upgrade recently.

(Similar issue appeared in NewWorlds' other game `Day of Infamy`)

Enviroment: Archlinux

# Solution:

Update `libgcc_s.so.1` (which located at `~/.steam/steam/steamapps/common/insurgency2/bin/libgcc_s.so.1`) 

Copy system's `libgcc_s.so.1` to replace the old one:

	cp /usr/lib/libgcc_s.so.1 ~/.steam/steam/steamapps/common/insurgency2/bin/libgcc_s.so.1

# How do I found the solution:

Launch game in console (ensure Stream is already opened), and got following messages: (I replaced the real home path wth `MY_HOME_PATH`)

	failed to dlopen MY_HOME_PATH/.local/share/Steam/steamapps/common/insurgency2/bin/engine.so error=MY_HOME_PATH/.steam/steam/steamapps/common/insurgency2/bin/libgcc_s.so.1: version `GCC_7.0.0' not found (required by /usr/lib32/libopenal.so.1)
	failed to dlopen MY_HOME_PATH/.local/share/Steam/steamapps/common/insurgency2/bin/engine.so error=MY_HOME_PATH/.steam/steam/steamapps/common/insurgency2/bin/libgcc_s.so.1: version `GCC_7.0.0' not found (required by /usr/lib32/libopenal.so.1)

	failed to dlopen engine.so error=MY_HOME_PATH/.steam/steam/steamapps/common/insurgency2/bin/libgcc_s.so.1: version `GCC_7.0.0' not found (required by /usr/lib32/libopenal.so.1)
	failed to dlopen engine.so error=MY_HOME_PATH/.steam/steam/steamapps/common/insurgency2/bin/libgcc_s.so.1: version `GCC_7.0.0' not found (required by /usr/lib32/libopenal.so.1)

	AppFramework : Unable to load library module engine.so!
	AppFramework : Unable to load library module engine.so!

	Unable to load interface VCvarQuery001 from engine.so, requested from EXE.
	Unable to load interface VCvarQuery001 from engine.so, requested from EXE.

It seems that `libopenal` failed to open engine.so, cause `libgcc_s.so.1` not support gcc version `GCC_7.0.0`. (em... so what's relation between `libopenal`, `libgcc_s.so.1` and `engine.so`?)

So check the `libgcc_s.so.1` first. I use `strings libgcc_s.so.1 | grep "^GCC_"` to check which version it supports. 

Insurgency's origin one support:

	GCC_3.0 
	GCC_3.3 
	GCC_3.3.1 
	GCC_3.4 
	GCC_3.4.2 
	GCC_4.0.0 
	GCC_4.2.0 
	GCC_4.3.0 
	GCC_4.4.0 
	GCC_4.5.0 
	GCC_4.0.0 
	GCC_3.3.1 
	GCC_4.4.0 
	GCC_4.5.0 
	GCC_4.2.0 
	GCC_4.3.0 
	GCC_3.0 
	GCC_3.4 
	GCC_3.3 
	GCC_3.4.2

Native system's `libgcc_s.so.1` is (which seems newer):

	GCC_3.0
	GCC_3.3
	GCC_3.3.1
	GCC_3.4
	GCC_3.4.2
	GCC_3.4.4
	GCC_4.0.0
	GCC_4.2.0
	GCC_4.3.0
	GCC_4.7.0
	GCC_4.8.0
	GCC_7.0.0
	GCC_3.3.1
	GCC_3.4
	GCC_4.7.0
	GCC_7.0.0
	GCC_3.4.2
	GCC_3.4.4
	GCC_4.2.0
	GCC_3.3

As you can see, Insurgency's didn't support `GCC_7.0.0`, so we can replace it with system's new one.

Hope this can help you.
