# qowldecor
Quite OK wayland decor library written in Nim.

## Why
GNOME’s refusal to support server-side decorations (SSD) is an indefensible design choice that shifts the burden of its broken assumptions onto application developers and users.
Client-side decorations (CSD) only make things worse by encouraging inconsistent and incoherent window appearance across applications and toolkits.
This library, like `libdecor`, therefore implements CSD rendering as a practical workaround for a problem that should not exist in the first place.

## Why not libdecor?
Because it sucks. In my testing, `libdecor`’s GTK plugin consistently causes multi-second hangs during trivial headerbar interactions. Even merely hovering the close button can block the application long enough for the desktop to report it as unresponsive. This makes the GTK plugin unsuitable as a practical foundation for client-side window decorations. Besides this, libdecor can't provide a native-looking overlay for its primary purpose — GNOME. It doesn't use libadwaita, so it looks not native.

## Architecture
Unlike libdecor, this library deliberately avoids a plugin architecture. In practice, that model mostly adds indirection and maintenance cost without yielding meaningful extensibility. Instead, there are currently only two rendering backends (that cover 100% cases): a `wl_shm` fallback and a `Vulkan` using `VK_KHR_external_memory`. It uses `wl_subsurface`, so you don't have to worry about your app being OpenGL and window decorations being rendered via Vulkan.

Themes are kept separate from the rendering backend. This avoids the Cartesian product of multiple themes by multiple rendering backends.
