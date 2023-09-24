---
layout: post
title:  "Gamedev"
date:   2023-09-24 14:37:00 +0200
tags: gamedev games programming
author: Eljas Hyyrynen (hyrtsi)
---

I love games.
In fact they are the reason I started programming.
But for some reason I haven't made many games.
I don't think I ever finished a game project.
I haven't finished any other projects either.

I have discovered a rule that I broke many times: if you want to make games, don't make game engines.
The same thing applies for other things as well.
If you want to make something, don't make the tools for making that thing.

However, creating your own games from scratch will teach you many lessons.
In this series I will share my experiences and lessons learned from making games.

# Making your first game (engine) in c++

You have been warned: if you want to make games please pick up Unity or Unreal or Gamemaker.
Do NOT think that you're unique and you can make games by first making your own engine.
That does not work.

Ok now that we got that out of the way, let's get started.
I will not teach you to use Unity.
I will teach you [SFML](https://www.sfml-dev.org/).

# Tool for the job: SFML

The reason I have used SFML in many projects is that it gets you up and running really fast.
You will have your first circle rendered in a window in no time.

I use Ubuntu in my projects even though it's not the most popular platform for video games.
If you want to compile the hello world in some other platform, please set up your compiler, linker and SFML installation accordingly.
The tutorials can be found [here](https://www.sfml-dev.org/learn.php).

## Install SFML

```
sudo apt install libsfml-dev
```

## Set up CMakeLists.txt

```cmake
project(hello_sfml)
add_executable(hello_sfml main.cpp)
find_library(SFML REQUIRED)
target_link_libraries(hello_sfml sfml-graphics sfml-window sfml-system)
target_include_directories(hello_sfml PUBLIC ${SFML_INCLUDE_DIR})
target_include_directories(hello_sfml PUBLIC include)
```

## Set up `main.cpp`

```c++
#include <SFML/Graphics.hpp>

int main()
{
    sf::RenderWindow window(sf::VideoMode(200, 200), "SFML works!");
    sf::CircleShape shape(100.f);
    shape.setFillColor(sf::Color::Green);

    while (window.isOpen())
    {
        sf::Event event;
        while (window.pollEvent(event))
        {
            if (event.type == sf::Event::Closed)
                window.close();
        }

        window.clear();
        window.draw(shape);
        window.display();
    }

    return 0;
}
```

## Compile and run

```bash
cmake -S . -B build && cmake --build build -j16 && ./build/hello_sfml
```

## Expected output

A window with a green circle

## Debugging

If your installation did not succeed, this line will give you errors because we set SFML as `REQUIRED`.

```
find_library(SFML REQUIRED)
```

In any other case, consult to the [tutorials](https://www.sfml-dev.org/learn.php) or ask ChatGPT.

---

# Making games in c++

## Game loop

The game loop is the loop that runs as long as your game is running.
It is the loop that updates your game state and renders the game to the screen.

Pseudocode:
- initialize game
- while game is running
    - update game state
    - render game state

Simple, eh?
