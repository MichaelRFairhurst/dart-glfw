# Dart bindings to [GLFW](http://glfw.org/) 3.1.1.

It is unlikely that programs written with these bindings will work on OSX due to
requirements that the main thread be the one handling events.

# Steps to generate the bindings
```shell
mkdir lib/src/generated/
cd tools
./generate_bindings.sh
cp generated/* ../lib/src/generated/
```
# Steps to compile the bindings

## Linux with GNU Make:
If your `dart_api.h`, `GLFW/glfw3.h`, or `libglfw.so` files are not in the
standard locations, set the enviroment variables `DART_INCLUDE`, `GLFW_INCLUDE`,
and/or `GLFW_LIB` and run the following commands.

```shell
cd lib/
make
```
Note that if you set the `GLFW_LIB` variable when compiling, you must also set
`LD_LIBRARY_PATH` to include the same directory when running your program or
the Dart VM will not be able to find `libglfw.so`.

TODO(hstern): It is convenient for development to use the .so file,
but for distribution purposes it is less useful. It would be nice to have an
option to use the .a library as well.

## Other Platforms
TODO

# Notes about the auto-generated bindings

- `glfwWindowShouldClose` returns an `int`, rather than a `bool`, but unlike
  in C, the expression `!(1)` returns true so you must explicitly test
    `glfwWindowShouldClose(window) != 1`
  - There is also a convenience function\
      `bool glfwWindowShouldCloseAsBool(GLFWwindow window)`

# The following functions have changed from the C GLFW API
These changes are due to the C library's use of pointer arguments to return
values. See also lib/src/manual\_bindings.dart.

- glfwGetVersion
  - Returns a `GLFWVersion` instance which has `major`, `minor`, `rev` fields.
- glfwGetMonitors
  - Returns a `List<GLFWmonitor>` instance.
- glfwGetMonitorPos
  - Returns a `Point` instance.
- glfwGetMonitorPhysicalSize
  - Returns a `Rectangle` instance with `xpos` and `ypos` set to 0.
- glfwGetVideoModes
  - Returns a `List<GLFWvidmode>` instance.
- glfwGetWindowPos
  - Returns a `Point` instance.
- glfwGetWindowSize
  - Returns a `Rectangle` instance with `xpos` and `ypos` set to 0.
- glfwGetFramebufferSize
  - Returns a `Rectangle` instance with `xpos` and `ypos` set to 0.
- glfwGetWindowFrameSize
  - Returns a `Rectangle` instance.
- glfwSetWindowUserPointer
  - `pointer` parameter is a Dart `Object`.
- glfwGetWindowUserPointer
  - Returns an `Object` instance.
- glfwGetCursorPos
  - Returns a `Point` instance.
- glfwGetJoystickAxes
  - Returns a `List<double>` instance.
- glfwGetJoystickButtons
  - Returns a `List<int>` instance.
