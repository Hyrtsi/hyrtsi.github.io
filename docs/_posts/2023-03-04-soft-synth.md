---
layout: post
title: "Introduction to Audio programming"
date: 2023-03-04 13:37:00 +0200
tags: audio c++
author: Eljas Hyyrynen
---

I love music and programming so it's a natural extension to try to create sounds using programming.
In this post we will create a naive and simple to understand program that creates a sound, an audio equivalent of `Hello World`.
We will work our way from there towards something more sophisticated.

As in graphics programming, I think [SFML](https://www.sfml-dev.org/) abstracts the unnecessary details of the audio pipeline for the purposes of a gentle introduction to the topic.

## Dependencies

```
sudo apt install libsfml-dev g++ cmake
```

If you're not using Linux, please follow the [SFML official tutorial](https://www.sfml-dev.org/tutorials/2.5/#getting-started) to get the library running on Windows / macOS.

### Create the file `main.cpp`:

Please replace the string `"ex.ogg"` with whatever the name of your audiofile is.

```c++
#include <SFML/Audio.hpp>

#include <chrono>
#include <cstdio>
#include <thread>

int main()
{
    sf::Music music;
    if (not music.openFromFile("ex.ogg"))
    {
        printf("Error: could not load audio file\n");
        return 1;
    }

    const int32_t lengthMs = static_cast<int32_t>(music.getDuration().asMilliseconds());
    music.play();
    std::this_thread::sleep_for(std::chrono::milliseconds(lengthMs));

    return 0;
}
```

### Create a file `CMakeLists.txt`:

```cmake
cmake_minimum_required(VERSION 3.5)
project(audiohelloworld)
find_package(SFML 2.5.1 COMPONENTS audio REQUIRED)
add_executable(music main.cpp)
target_link_libraries(music sfml-audio)
```

Download an audiofile or use one from our hard drive.
Note that SFML does not support `.mp3` audio file format.
You can use this ([link](https://commons.wikimedia.org/wiki/File:Example.ogg)) audio file as an example.

### Run the app:

```
cmake -S . -B build && cmake --build build
./build/music
```

You should hear whatever sound was in your audio file.

### Troubleshooting

- I do not hear audio when I run the app
  - Do you have a proper sound device and drivers?
  - Check your volume settings
  - Check with a 3rd party application that your audio file is valid
  - Test with several audio files. Remember to change the filename in the code accordingly
  - Check that the audio file is in the right location. It should be in the project root, not in the build folder where the app is
  - You can add debugging prints to check the audio sample properties: number of channels, number of samples, length. [Link1](https://www.sfml-dev.org/tutorials/2.5/audio-sounds.php) [Link2](https://www.sfml-dev.org/documentation/2.5.1/classsf_1_1Music.php)

I was surprised that they did not mention in the SFML tutorial that `play()` function call is nonblocking.
In other words, if you don't add the `sleep_for()` call you will not hear anything and the application will finish immediately.

Enough of that.
Time to create one more example: creating our own sounds.

## How does digital audio even work?

Sounds consist of samples.
Each sample is a 16-bit signed integer and the sample represents the signal value for $$ \Delta t = \frac{1}{N} $$ seconds where `N` is the sample rate.
Sample rate means simply, how many digital samples of the signal we do have per second.
A typical sample rate is 44100 samples per second.

We can represent the sound as an ordered array of these samples.
One such way is this:

```c++
std::vector<int16_t> sound;
// Fill the vector here
```

If you want a second of sound you have to create `N` samples.
That's exactly what we are going to do next.

The code:

```c++
#include <SFML/Audio.hpp>

#include <chrono>
#include <cstdio>
#include <thread>

int main()
{
    sf::SoundBuffer buffer;
    constexpr size_t nSeconds = 2LLU;       // How many seconds of sound we want in the buffer
    constexpr size_t sampleRate = 44100LLU; // Amount of samples per second
    constexpr size_t frequency = 440LLU;    // Hertz

    // Create the samples
    std::vector<sf::Int16> samples;
    for (size_t i = 0; i < sampleRate * nSeconds; ++i)
    {

        int16_t sample = i * std::numeric_limits<uint16_t>().max() / (sampleRate/frequency);

        // Transform the value from unsigned to signed integer
        sample = sample % std::numeric_limits<uint16_t>().max() - std::numeric_limits<int16_t>().min();
        samples.emplace_back(static_cast<sf::Int16>(sample));
    }

    if (not buffer.loadFromSamples(&samples[0], samples.size(), 1, sampleRate))
    {
        printf("Error: could not load buffer\n");
        return 1;
    }

    sf::Sound sound;
    sound.setBuffer(buffer);
    sound.play();
    std::this_thread::sleep_for(std::chrono::milliseconds(nSeconds * 1000));

    return 0;
}
```

Configure, build and run similarly to the previous example.
I tested this with a 440Hz frequency and the output of my program matches really closely to [this](https://en.wikipedia.org/wiki/A440_(pitch_standard)) sample.

The idea:
- create samples between `0` and `uint16_max` (`65535`).
- transform them between `-32768` and `32767` since the audio buffer needs to be signed integers
- load the buffer into SFML sound player and press play

## How to I calculate the sample value? i.e. How to create a sound for a given pitch?

I reasoned this to myself as follows.
The signal looks like this

![saw](https://upload.wikimedia.org/wikipedia/commons/d/d4/Synthesis_sawtooth.gif)

look at the first period of the signal. 
We want to define that the first point, the lowest point is `0` in our unsigned integer terms and the highest point is `65535`.
If the frequency is `f` then by definition we have `N/f` samples for each period of the signal.
You can verify this with pen and paper if its' not intuitive for you.
If we have `44100` samples each second and a signal of `440` Hz, each of the periods of the signal is `44100/440` samples long.

How can we achieve this in the code?
We create a linear interpolation between the minimum (`0`) and the maximum (`65535`).
For the example above, we calculate how much we need to increment the signal so that in `44100/440 = 100` increments it goes from `0` to `65535`.

## Improvements

I got a rather crackling sound with the code.
I studied a little bit and found out a few things:
- SFML has `SoundStream` for a continuous stream of sound. [Tutorial](https://www.sfml-dev.org/tutorials/2.5/audio-streams.php) | [Reference](https://www.sfml-dev.org/documentation/2.5.1/classsf_1_1SoundStream.php)
- There will be discontinuity in the data unless you use `free spinning oscillator` to create the sample as explained [here](https://github.com/Atrix256/MusicSynth/blob/master/Audio%20Synthesis%20For%20Music.pdf) (slide 7 on popping)


## More reading

If you want to learn more in-depth about digital signal processing, digital audio and synthesizers, I suggest the following resources:
- Music: a Mathematical Offering by David J. Benson. [Link](https://homepages.abdn.ac.uk/d.j.benson/pages/html/maths-music.html)
- Julius O. Smith's numerous teachings. [Link](https://ccrma.stanford.edu/~jos/)
- Aalto University courses. Or any other university for that matter. I suggest the following courses and topics:
  - Acoustics and the physics around sound and voice
  - Electronics if you're into analog synthesizers
  - Mathematics. Lots of it. Especially Fourier analysis, calculus and complex analysis
  - Basic programming courses are enough to keep you going. You have to learn pointers, data structures, algorithms and multithreading
  - Signal processing
- YouTube and other video streaming services have lots of content on the topics. I will not post any links because there are too many.
- Sound synthesis basics. [Link](https://theproaudiofiles.com/sound-synthesis-basics/)
- Code example [Link](https://github.com/Atrix256/MusicSynth)
- Blog posts by Atrix256 [Link](https://blog.demofox.org/)
