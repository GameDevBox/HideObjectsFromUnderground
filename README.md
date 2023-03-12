# HideObjectsFromUnderground
Here's a shader for Unity that hides objects from underground (-Y)

## Setup 
Download 
This shader uses a discard statement to discard fragments that have a world normal pointing downwards (-Y direction), effectively hiding objects from underground. You can adjust the condition to discard fragments based on other criteria as well.

The shader also includes a texture sample to avoid any compilation errors, but you can remove it if you don't need it. Just make sure to update the properties accordingly.
