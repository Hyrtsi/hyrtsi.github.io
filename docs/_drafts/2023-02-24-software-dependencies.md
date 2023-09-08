How do software dependencies even work?

When I started coding using dependencies was just magic. I mostly resorted to the standard library.
Using something like Qt to draw things or SFML was pain. I spent weeks fighting with linker errors.
Even upon making something worked I never understood why it worked.

I'm gonna share what I know about software dependencies.

# Definitions and scope

I am going to first focus on using third party libraries in c++ code.
Simply it means using someone elses code in your codebase to avoid coding it yourself.

# Case: c++

The simplest and fastest way to include someone elses code is to include headers from the c++ (or c) standard library.

```
#include <vector>
```

You don't have to do _anything_ else than include it given that you're using the right compiler and right c++ version.

You don't have to add any headers in your CMake or Makefile, you don't have to install any packages from apt, nothing.
Just include them, rtfm and use them happily.

---

The second simplest case: copy pasting the code. Either copypaste the lines of code or the files.
Remember to include the new files to your build and take care of the dependencies that the files need (includes and 3rd party libraries).

This is the worst way to use dependencies and the hardest to maintain but is indeed sometimes possible.

The downsides are that you end up making a Frankenstein monster of a code.
You don't get any bugfixes.
You lose the trackability and openness of the code.

---

Then, installing packages from `apt`.

Example: SFML

1. Install the package: `sudo apt install libsfml-dev`
2. Add this line in your `CMakeLists.txt`: `find_package(SFML 2.5.1 REQUIRED)`
3. Add this line, too, in `CMakeLists.txt`: `target_link_libraries(your_app_name PRIVATE sfml-graphics sfml-window sfml-system)`

Now SFML is part of your build. If you try to build your project you should see either a successful build or a failed build.
- Failed build: did SFML install correctly? Do you have a typo somewhere?
- Successful build: time to move forward!

Add the code from [here](https://www.sfml-dev.org/tutorials/2.5/start-linux.php#compiling-a-sfml-program) to test using SFML. It should work and you should see a circle in your screen in that case.

Finally, 

4. Add the necessary includes of SFML headers to your program and use it happily.

Note that SFML has been built in components.