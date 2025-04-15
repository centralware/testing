# Example Builder

### Here we have a compilation script for the package "Dropbear" along with its INIT script
### This is just an example which is missing some components for dependency version checking
### and kernel checking.

Our TEST script above calls our builder's functions in a fashion similar to how the server farm will be using them.
The compilation has a test mode available which will fail when using TCL 15.x as its dependencies require a newer version than TCL 15 has available, but if the newer files were implemented, the test does in fact succeed.  For sake of this example, the builder will not exit regardless of that test's outcome.


