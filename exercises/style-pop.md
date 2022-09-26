
# Style: `Pop`

This `Pop` style code uses a more realistic drumsound in a uptempo house beat combined with a funky bass synthesizer that alternates in octaves and a lead pad. Pop style rhythms typically have a BPM (Beats Per Minute) somewhere between 100 - 130, but please don't hesitate to experiment with different tempos. While working on the various steps to program the music I like to encourage you to try many different values instead of the ones suggested in the tutorial.

## 1. Rhythm & Samples

We start by creating a hi-hat rhythm of 8th notes (1 bar divided into 8 segments: `1/8`) and we will also set the `tempo` at `125`

```java
set tempo 125

new sample hat_909 time(1/8)
```

Since the 909 hi-hat does not really sound like a acoustic drums hihat we will stretch the sound by using the playback `speed()` function. But because this also lengthens the sound we will use the `shape()` function to give it a shorter fade-in and fade-out.

```java
new sample hat_909 time(1/8) speed(0.4) shape(1 50)
```

Now we will include extra percussion sounds such as the kick (or also called bassdrum) and snare (or snaredrum). We will play the kick twice, once at the beginning of every measure and once half way through (1 bar divide into 2 segments: `1/2`) and we will play the snare also twice per measure, but we will play it in between the two kicks,so we need an offset of a quarter of the bar (`1/4`). We will also lower the `gain()` (volume) of the hihat a little bit so it sounds a bit better compared to the other drum sounds.

```java
new sample hat_909 time(1/8) speed(0.4) shape(1 50) gain(0.5)

new sample kick_vintage time(1/2)
new sample snare_ac time(1/2 1/4)
```

To give the snare a snappier sound we will overlay it with a new `sample` of a `clap_909`.

```java
new sample clap_909 time(1/2 1/4)
```

Now we can use a `list` `[1 0.9 1 0.3]` to create a more complicated hihat rhythm. In this list we will use probability values (between 0-1) to have the hihat play twice as fast often. In order to allow this double speed we will need to set the standard `time()` of the hihat to 16nd notes (`1/16`). We add the name of the list as an argument to the `play()` function and as a second argument we let play reset the pattern every bar by adding a `1`. 

```java
list hatBeat [1 0.9 1 0.3]
new sample hat_909 time(1/16) play(hatBeat 1) speed(0.4) shape(1 50) gain(0.5)
```

We can make some of the sounds less predictable by adding a probability to their `play()` function. The value between 0-1 determines how likely it is the sound will be played. 

```java
new sample kick_vintage time(1/2) play(0.85)
```

Lastly we can play around with offsetting the clap to create different composite rhythms (a rhythm that is a result of all the sounds together). So the final result will look something like this:

```java
set tempo 125

list hatBeat [1 0.9 1 0.3]
new sample hat_909 time(1/16) play(hatBeat 1) speed(0.4) shape(1 50) gain(0.5)
new sample kick_vintage time(1/2) play(0.85)
new sample snare_ac time(1/2 1/4)
new sample clap_909 time(7/16) play(0.7)
set all fx(reverb 0.3 1.2)
```

Go ahead and make the beat more interesting by playing around with different rhythms and values for the various functions `time`, `play`, `shape` and `gain`. You can also replace the sounds with other soundfiles that you like.

## 2. Bass & Melody

Coming soon...

## 3. More Rhythms, Lists & Effects

Coming soon...

## 4. Final Result

Coming soon...
