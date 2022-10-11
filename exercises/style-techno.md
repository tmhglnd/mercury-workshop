
# Style: `Techno`

This `Techno` style code uses the famous 909 drum synthesizer with a very uptempo beat combined with a chopped drumloop and a fast bass synthesizer that has a modulating filter resulting in a acid techno type of sound. Techno style rhythms typically have a BPM (Beats Per Minute) somewhere between 130 - 160, but please don't hesitate to experiment with different tempos. While working on the various steps to program the music I like to encourage you to try many different values instead of the ones suggested in the tutorial.

## 1. Rhythm & Samples

We start by creating a hi-hat rhythm of 4th notes (1 bar divided into 4 segments: `1/4`), but we will offset the notes by `1/8` and we will also set the `tempo` at `140`

```java
set tempo 140

new sample hat_909 time(1/4)
```

We will now include extra percussion sounds such as the kick (or also called bassdrum) and snare (or snaredrum). We will play the kick four times in a measure (1 bar divide into 4 segments: `1/4`) and we will play the snare twice per measure, but we will play it on the 2nd and 4th count, so we need an offset of a quarter of the bar (`1/4`). We will also lower the `gain()` (volume) of the hihat a little bit so it sounds a bit better compared to the other drum sounds.

```java
new sample hats time(1/4 1/8) gain(0.5)

new sample kick_909 time(1/4) 
new sample snare_909 time(1/2 1/4)
```

Since the 909 snare sounds quite high we can stretch the sound to a tone of our liking by using the playback `speed()` function. But because this also lengthens the sound we will use the `shape()` function to give it a shorter fade-out.

```java
new sample snare_909 time(1/2 1/4) speed(0.7) shape(1 100)
```

Now we can use a `list` `[hat_909 hat_909_open]` to create an alternating sound for the hihat. We add the name of the list as the samplename argument after `sample`.

```java
list hats [hat_909 hat_909_open]
new sample hats time(1/16) gain(0.5)
```

We can make some of the sounds less predictable by adding a probability to their `play()` function. The value between 0-1 determines how likely it is the sound will be played. 

```java
new sample kick_909 time(1/4) play(0.9)
```

We can add a `loop` to play a longer soundfile that is synchronized with the tempo of our track. In this case we will add a breakbeat called `amen`. The length of this loop is exactly `1 bar`, so we will set the `time()` to `1/1` or simply `1`. We will lower the `gain()` a bit so it sits more in the background. The final result will look something like this:

```java
new loop amen time(1) gain(0.6)
```

Lastly we can play around with offsetting the clap to create different composite rhythms (a rhythm that is a result of all the sounds together). So the final result will look something like this:

```java
set tempo 140

list hats [hat_909 hat_909_open]
new sample hats time(1/4 1/8) gain(0.5)
new sample kick_909 time(1/4) play(0.9)
new sample snare_909 time(1/2 1/4) speed(0.7) shape(1 100)

new loop amen time(1) gain(0.6)
```

Go ahead and make the beat more interesting by playing around with different rhythms and values for the various functions `time`, `play`, `shape` and `gain`. You can also replace the sounds with other soundfiles that you like.

## 2. Bass & Melody

### Bass

For the second part we will work on a bass synthesizer creates both a bass and melodic part. The synth generates a specific type of waveform in a rhythmical pattern and has a so called envelope that starts and stops the sound over a period of time. First we will work on the bass synthesizer by adding a `new synth` of type `saw` that plays every sixteenth note `time(1/16)` a default `note(0 0)`.

```java
new synth saw note(0 0) time(1/16)
```

we can make it play a bit more staccato by adding the shape function and giving it an short attack, a hold and then a short release.

```java
new synth saw note(0 0) time(1/8) shape(1 80 1)
```

It is quite harsh in terms of color/timber. So what we can do is add a filter to the synth that removes high frequencies above a certain value. We use the `fx()` function for this and as an argument we use `filter`. The type of filter will be `low` which mains only output the low frequencies (lowpass).

```java
new synth saw note(0 0) time(1/8) shape(1 100) fx(filter low 900)
```

Now lets create a list of notes for the bass to play. We add the list as an argument to the `note()` function. This list will be the progression of the musical piece. We want to duplicate the values of the list for every note the bass plays so it fills a bar, so we will use a list function to transforms the list and repeats every note 16 times. We also make sure the part resets after `4` bars in `play()`.

```java
list progression [0 2 3 2]
list bassNotes repeat(progression 16)
new synth saw note(bassNotes 0) time(1/16) shape(1 80 1) fx(filter low 900)
```

### Melody

Now we will work on the melody that is played by the same synthesizer. The melody will be alternating from the bass synth in a higher register (higher notes). We start by creating a list that temporarily replaces the current `bassNotes`.

```java
list bassNotes [24 0 0 24 0 0 19 0]
// list bassNotes repeat(progression 16)
new synth saw note(bassNotes 0) time(1/16) shape(1 80 1) fx(filter low 900)
```

Now we want to duplicate this pattern for the progression length by offsetting every repition by the amount of the progression. So for example in the `3` of the progression the list becomes `[27 3 3 27 3 3 22 3]`. We can do this by using the `clone()` function. This clones a list and adds a value to it as an offset.

```java
list progression [0 2 3 2]
list bassNotes [24 0 0 24 0 0 19 13]
list bassNotes clone(bassNotes progression)
new synth saw note(bassNotes 0) time(1/16) shape(1 80 1) fx(filter low 900)
```

To make the sound more interesting we can also create a list of different cutoff frequencies for the filter to modulate that over time.

```java
list cutoffs [300 700 1500]
new synth saw note(bassNotes 0) time(1/16) shape(1 80 1) fx(filter low cutoffs)
```

If we set the scale for the whole piece we don't have to worry about generating values that are maybe not part of a musical scale. For example we can `set scale minor c`.

```java
set scale minor c
```

To finish we can adjust the synth sound more by creating a so called `super()` synth. This is a synth that plays multiple synths (voices) at the same time in unison with a little detuning creating an interesting effect. For example try:

```java
new synth saw note(bassNotes 0) time(1/16) shape(1 80 1) fx(filter low cutoffs) super(0.213 3)
```

This means 3 voices with a small detuning of 0.12. But instead of `(0.213 3)` you can also try other values like `(0.32 5)`. The final result should look something like this:

```java
set tempo 140

list hats [hat_909 hat_909_open]
new sample hats time(1/4 1/8) gain(0.5)
new sample kick_909 time(1/4) play(0.9)
new sample snare_909 time(1/2 1/4) speed(0.7) shape(1 100)

new loop amen time(1) gain(0.6)

list progression [0 2 3 2]
list bassNotes [24 0 0 24 0 0 19 13]
list bassNotes clone(bassNotes progression)
list cutoffs [300 700 1500]
new synth saw note(bassNotes 0) time(1/16) shape(1 80 1) fx(filter low cutoffs) super(0.213 3)
```

## 3. More Rhythms, Lists & Effects

Coming soon...

## 4. Final Result

```java
set tempo 140
set scale minor c

new sample kick_909 time(1/4) play(0.9) fx(squash 1.1)
list hats choose(4 [hat_909 hat_909_open])
new sample hats time(1/4 1/8) gain(0.5)
new sample snare_909 time(1/2 1/4) speed(0.7)

list slices choose(16 spreadF(16))
new loop amen time(1/16) fx(filter high 300 0.4) start(slices) gain(0.6)

list bassRhythm palin(euclid(8 3))
list highNotes spread(5 12 36)
list bassLine spray(highNotes bassRhythm)
list cutoffs shuffle(mtof(spread(32 60 108)))

new synth saw note(bassLine) time(1/16) super(0.213 3) name(bass)
    set bass fx(filter low cutoffs 0.6) fx(reverb 0.2 2)
```