graphical-ninja
===============

A C++ library that allows you to hook into the rendering pipeline of DirectX 8-11 and OpenGL 3-4 applications on Windows using a common, simple interface. The rest is up to you. 

**Note (September 2012): This library is currently hosted on a private git server. I've been asked to put the readme onto GitHub from a few users. graphical-ninja and its dependencies may become an open source library in the near future.**

## installation
graphical-ninja depends on `freedompeace/graphical-abstractions`, `freedompeace/x64-detours`. Place each dependency in its own directory `dep/{library}`, relative to the working directory.

Assuming you've got `make` and `gcc`, build the lib and headers and reference them in your own project:

```bash
$ cd ~/graphical-ninja
$ make lib
```

And optionally, unit tests:
```bash
$ make tests 
```
## anti-cheat
graphical-ninja does not have any issues working with the following anti-cheat engines:

- Valve Anti-Cheat (checked January 2012)
- Blizzard Warden (checked November 2011)
- AhnLab HackShield (checked January 2012)
- nProtect GameGuard (checked January 2012)

Some cheat engines will throw up if you try to add a callback to a protected function (because
`graphical-ninja` will patch the function assembly code to create a codecave). For example, you
might be able to add a callback to the `PostEndScene` function but not the
`Direct3dPreSetStreamSource` function on some cheat engines. Please
[file an issue](http://internal.freedompeace.net/projects/graphical-ninja/issues) if the above
information is incorrect and I'll update the information above and maybe work on a fix. Make sure
graphical-ninja is causing the detection and not what you choose to do with it. Other anti-cheat
engines have not been detected.

## usage
Using the library is quite simple. Refer to the [documentation](http://internal.freedompeace.net/projects/graphical-ninja/docs):

```cpp
// GraphicalNinja that binds to the current process.
auto ninja = new GraphicalNinja();

// Locates the first graphics device found. GraphicalNinja::FindOne() returns a 
// GraphicalAbstractions::GraphicsDevice or nullptr if a graphics device couldn't be
// found.
auto graphics = ninja->FindOne();

// Let's do something after the application renders a new scene.
auto callbackToken = graphics->AddCallback(
	CallbackType.PostEndScene,
	(auto device)
	{
		// `device` refers to the same object as `graphics`
		auto backbuffer = device->GetBackBuffer();

		// ...
		//   Perhaps copy the backbuffer into a video?
		//     Set your imagination free!
	});

// Remove the callback.
graphics->RemoveCallback(callbackToken);
```
I've been frequently asked about native device access. As per the `graphical-abstractions` readme, `graphics->GetNativeDevice()`:
```cpp
if (graphics->NativeDeviceType == NativeDeviceType.Direct3D11)
	auto direct3d11 = reinterpret_cast<ID3D11Device*>(graphics->GetNativeDevice());
```

## license
Shhh! graphical-ninja is still a work in progress! Don't leak our hard work! graphical-ninja will eventually be licensed under the terms of the MIT license.