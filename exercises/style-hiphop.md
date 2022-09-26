
# Style: `Hip-Hop`

This `Hip-Hop` style code uses the classic 808 drum synthesizer combined with an ascending staccato lead melody line together with a bass synthesizer. Hip-hop style rhythms typically have a BPM (Beats Per Minute) somewhere between 75-110, but please don't hesitate to experiment with different tempos. While working on the various steps to program the music I like to encourage you to try many different values instead of the ones suggested in the tutorial.

## 1. Rhythm & Samples

We start by creating a hi-hat rhythm of 16th notes (1 bar divided into 16 segments: `1/16`) and we will also set the `tempo` to `80`

```java
set tempo 80

new sample hat_808 time(1/16)
```

Now we will include extra drum sounds like the kick (or also called bassdrum) and snare (or snaredrum). We will play the kick twice, once at the beginning of every measure and once half way through (1 bar divided into 2 segments: `1/2`) and we will play the snare also twice per measure, but we will play it in between the two kicks, so we need an offset of a quarter of the bar (`1/4`). We will also lower the `gain()` (volume) of the hihat a little bit so it sounds a bit better compared to the other drum sounds.

```java
new sample hat_808 time(1/16) gain(0.7)

new sample kick_808 time(1/2)
new sample snare_808 time(1/2 1/4)
```

Now we can use a `list` `[1 0.05 1 0.2]` to create a more complicated hihat pattern. In this list we will use probability values (between 0-1) to have the hihat play twice as fast sometimes. In order to allow this double speed we will need to set the standard `time()` of the hihat to 32nd notes (`1/32`). We add the name of the list as an argument to the `play()` function and as a second argument we let play reset the pattern every bar by adding a `1`. 

```java
list hatBeat [1 0.05 1 0.2]
new sample hat_808 time(1/32) play(hatBeat 1)
```

We will also create a rhythm for the kick `[1 0 0 1 0 1 0 1]` that can be played in 8th notes (`1/8`)

```java
list kickBeat [1 0 0 1 0 1 0 1]
new sample kick_808 time(1/8) play(kickBeat 1)
```

Lastly we can make the kick pattern more interesting by also introducing some probabilities so that not all the `1`'s are played every time. We can tune the snaredrum to our liking by using the `speed()` function. And by adding an extra `maracas_808` percussion sound we will make a bit more variation to the rhythm as well. So the final result will look something like this:

```java
set tempo 80  
set scale minor d

list hatBeat [1 0.05 1 0.2]
new sample hat_808 time(1/32) play(hatBeat 1) gain(0.7)

list kickBeat [1 0 0 0.7 0 0.9 0 0.2]
new sample kick_808 time(1/8) gain(1) play(kickBeat 1)
new sample snare_808 time(1/2 1/4) speed(1.4)
new sample maracas_808 time(1/2 15/16)
```

Go ahead and make the beat more interesting by playing around with different rhythms and values for the various functions `time`, `play`, `shape` and `gain`. You can also replace the sounds with other soundfiles that you like.

## 2. Bass & Melody

Coming soon...

## 3. More Rhythms, Lists & Effects

Coming soon...

## 4. Final Result

Coming soon...
