# Ignorance: A reliable UDP Transport for Mirror Networking.
Ignorance is a wrapper that provides ENET-powered reliable UDP transport layer that plugs into the [Mirror Networking HLAPI](https://github.com/vis2k/Mirror). It uses nxrighthere's ENET-C# Wrapper as the glue between the ENET Native C plugin.
[If you use this, I'd appreciate a coffee to keep me caffeinated.](https://ko-fi.com/coburn)

This transport is currently developed and actively used by [Oiran Studio](http://www.oiran.studio).

## Mac OS Compatibility Issue
Due to a issue inside the Mac Editor of Unity, if you attempt to connect an Editor instance to a Editor instance on the same machine (localhost), then usually you will not be able to connect, even if the server seems to be running.

I **cannot** fix this bug. It seems that the **Unity Editor's Mono runtime** is to blame, however standalone client connecting to a Editor server apparently works. Go figure that one out. Please see the [upstream ENET-CSharp issue](https://github.com/nxrighthere/ENet-CSharp/issues/46) for more information.

NX has reported that ENET-CSharp works fine with the version of Mono that Unity 2019 alpha uses. *I do not recommend using alpha or beta versions of Unity Editor in production.*

## WARNING!
This version of Ignorance is for the *master* branch of Mirror which has "pluggable" transports. **Do not use this branch if you are using the 2018 version of Mirror**. Instead, select the "mirror2018" branch under "Branch" [or click here if you're lazy](https://github.com/SoftwareGuy/Ignorance/tree/mirror2018) to go to the right branch.

### What happens if I use the wrong version of Ignorance with the wrong version of Mirror?
* Best case scenario: It works, somehow.
* Worse case scenario: You will likely get import errors that say "X does not implement function Y". I will not be able to provide support if you are using the **wrong version of Ignorance** on the **wrong version of Mirror**. I'm not trying to be a dick or anything, I simply cannot support people *mix and matching* versions in hope that it'll work.

## Compatibility
- 32Bit Standalone targets are **not supported** as I am not able to get a DLL compiled that supports 32Bit targets. Please make sure you target 64Bit for standalone builds.
- Android 32Bit ARMv7 and 64Bit ARM64, Windows x64, Mac OS x64, Linux x64 platforms are supported.
- Tested and confirmed to work on both Unity 2017.4 and Unity 2018.2 (using the respective branches of Mirror and Ignorance, of course).

Ensure you correctly configure the Redist plugins (included in the repo and releases). You need to make sure that you follow the readme file inside the Redist to the letter or you can expect very weird things to happen...

## Installation
### Release Method
1. Grab a release from the [releases page](https://github.com/SoftwareGuy/Ignorance/releases) that says it's a **Pluggable Transport** version.
2. Make sure you have [Mirror](https://github.com/vis2k/Mirror) installed in your project.
3. Extract the downloaded release archive into your project, maybe under `Assets/Packages/IgnoranceTransport`.
4. Let Unity detect the new scripts and compile.

### Git Clone Method
Okay fine, I get it. You want the bleeding edge, huh? Sure, sure. Mirror upstream master branch does change a lot in very little time, so I understand.
1. Git clone this repository.
2. If you haven't already done so, make a folder called `Ignorance` in your project. Also make a `Dependencies` folder just for house keeping under that.
3. Copy the repos' `ENet.cs` file from the Dependencies folder into your projects `Ignorance/Dependencies` folder.
4. Copy the repos' Redist folder to your project's `Ignorance` folder.
5. Delete any duplicate Redist folders from your project that may contain duplicate copies of the ENET binary blobs. **Only one copy of each blob must be present in your project at one time.**
6. Import, and let Unity recongize everything. There should be no errors.
7. To get the latest, simply open the cloned repository, do a `git pull` to sync your local copy and you'll get the latest changes. Follow the steps above to update your project to latest Ignorance master.

## Dependencies
- [Mirror](https://github.com/vis2k/Mirror)
- UnityAsync (Mirror 2018 branch only)
- [ENet-CSharp](https://github.com/nxrighthere/ENet-CSharp)
- ENET itself. As in the C library.

## How to use
- Add the Ignorance Transport script onto your NetworkManager game object and remove any existing ones, like Telepathy, TCP, etc.
- Configure to your liking
- You're good to go!

## Why should I use reliable UDP?
Since UDP is designed to be a scattershot shotgun approach to networking, there's no promises that UDP packets will get from A to B without going through hell and back. That is why UDP ignores a lot of stuff that TCP fusses over. 

However, reliable UDP tries to mimic TCP to some extent with the sequencing and retransmission of packets until they land at the destination. This makes sense in some usage cases like VoIP and multiplayer shooter games where you need a data firehose rather than reliablity.

Please do consider TCP (Telepathy) if you're doing Mission Critical networking. There's a reason why big name MMOs use TCP to keep everything in check. However, for some usage cases TCP may be a little overkill, so UDP is recommended.

## Credits
- **Coffee Donators**: Thank you so much.
- **Petris**: Code refactoring and tidy up (you rock man!)
- **[nxrighthere](https://github.com/nxrighthere)**: Debugging my broken code and identifying my packet code fuckups
- **[Draknith](https://github.com/FizzCube)**: Testing and mapping Reliable/Unreliable channels in Mirror to ENET Channels, testing.
- **[vis2k](https://github.com/vis2k)**: The mad man behind the scenes that made Mirror happen. Much respect.
- **Mirror Discord**: Memes, Courage, LOLs, awesome folks to chat with
