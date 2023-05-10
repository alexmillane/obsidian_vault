How to debug in vscode:

* Change the target to the right of the bug symbol to the Cmake target you'd like to debug
![[Pasted image 20230504113413.png]]
* Set some breakpoints
* click the bug symbol

## Things I did
Things didn'y always work when compiling with `DebWithRelInfo` so I changed `CMAKE_BUILD_TYPE`  in the `CMakeCache.txt` to `Debug`. Manually changing this isn't ideal and I bet it could be done better. But I'll leave that as homework.

