
# Style: `Techno`

This `Techno` style code uses the famous 909 drum synthesizer with a very uptempo beat combined with a chopped drumloop and a fast bass synthesizer that has a modulating filter resulting in a acid techno type of sound. Techno style rhythms typically have a BPM (Beats Per Minute) somewhere between 130 - 160, but please don't hesitate to experiment with different tempos. While working on the various steps to program the music I like to encourage you to try many different values instead of the ones suggested in the tutorial.

## 1. Rhythm & Samples

We start by creating a hi-hat rhythm of 4th notes (1 bar divided into 4 segments: `1/4`), but we will offset the notes by `1/8` and we will also set the tempo at `140`

```java
set tempo 140

new sample hat_909 time(1/4)
```

We will now include extra percussion sounds such as the kick (or also called bassdrum) and snare (or snaredrum). We will play the kick four times in a measure (1 bar divide into 4 segments: `1/4`) and we will play the snare twice per measure, but we will play it on the 2nd and 4th count, so we need an offset of a `1/4`-segment of a bar. We will also lower the volume of the hihat a little bit.

```java
new sample hats time(1/4 1/8) gain(0.5)

new sample kick_909 time(1/4) 
new sample snare_909 time(1/2 1/4)
```

Since the 909 snare sounds quite high we can stretch the sound to a tone of our liking by using the playback speed function. But because this also lengthens the sound we will use the shape function to give it a shorter fade-out.

```java
new sample snare_909 time(1/2 1/4) speed(0.7) shape(1 100)
```

Now we can use a list `[hat_909 hat_909_open]` to create an alternating pattern for the hihat. We add the name of the list as the samplename argument after `sample`.

```java
list hats [hat_909 hat_909_open]
new sample hats time(1/16) play(hatBeat 1) gain(0.5)
```

We can make some of the sounds less predictable by adding a probability to their play function. The value between 0-1 determines how likely it is the sound will be played. 

```java
new sample kick_909 time(1/4) play(0.9)
```

Laste we can add a `loop` to play a longer soundfile that is synchronized with the tempo of our track. In this case we will add a breakbeat called `amen`. The length of this loop is exactly 1 bar, so we will set the time to `1`. We will lower the gain a bit so it sits more in the background. The final result will look something like this:

```java
new loop amen time(1) gain(0.6)
```

Lastly we can play around with offsetting the clap to create different patterns. So the final result will look something like this:

```java
set tempo 140

list hats [hat_909 hat_909_open]
new sample hats time(1/4 1/8) gain(0.5)
new sample kick_909 time(1/4) play(0.9)
new sample snare_909 time(1/2 1/4) speed(0.7) shape(1 100)

new loop amen time(1) gain(0.6)
```

Go ahead and make the beat more interesting by playing around with different rhythms and values for the sounds. You can also replace the sounds with other soundfiles that you like.

## 2. Bass & Melody

Coming soon...

## 3. More Rhythms, Lists & Effects

Coming soon...

## 4. Final Result

Coming soon...