
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

### Bass

For the second part we will work on a bass synthesizer and a synthesizer that creates a melodic part. The synth generates a specific type of waveform in a rhythmical pattern and has a so called envelope that starts and stops the sound over a period of time. First we will work on the bass synthesizer by adding a `new synth` of type `saw` that plays every eight note `time(1/8)` a default `note(0 0)`.

```java
new synth saw note(0 0) time(1/8)
```

we can make it play a bit shorter by adding the shape function

```java
new synth saw note(0 0) time(1/8) shape(1 100)
```

It is quite harsh in terms of color/timber. So what we can do is add a filter to the synth that removes high frequencies above a certain value. We use the `fx()` function for this and as an argument we use `filter`. The type of filter will be `low` which mains only output the low frequencies (lowpass).

```java
new synth saw note(0 0) time(1/8) shape(1 100) fx(filter low 500)
```

Now lets create a list of notes for the bass to play. We add the list as an argument to the `note()` function. This list will be the progression of the musical piece. We want to duplicate the values of the list for every note the bass plays, so we will use a list function to transform the list and repeat every note 16 times. We also make sure the part resets after `4` bars in `play()`.

```java
list progression [0 2]
list bassNotes repeat(progression 16)
new synth saw note(bassNotes 0) time(1/8) shape(1 100) fx(filter low 500)
```

### Melody

Now we will work on the melody that plays together with the bass. The melody will be another synth that plays in a higher register (higher notes). First we start by creating another synth that plays every eight note but in an octave higher, together with a filter with a higher cutoff frequency. We'll add a `shape()` that plays the note a bit longer

```java
new synth saw note(0 2) time(1/8) fx(filter low 2000) shape(1 400)
```

Now we can add a melody as a list that will be played. Let's make a descending melody.

```java
list melody [3 3 3 3 2 2 2 2 0 0 0 0 0 0 0 0]
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400)
```

To make the sound more interesting we can let the notes `slide()` from one to the other (called portamento).

```java
list melody [3 3 3 3 2 2 2 2 0 0 0 0 0 0 0 0]
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400) slide(100)
```

If we set the scale for the whole piece we don't have to worry about putting in values that are maybe not part of a musical scale. For example we can `set scale minor d` and then add some random values to our melody without really thinking about the exact number too much.

```java
set scale minor d

list melody [3 3 3 10 2 2 2 9 0 0 0 6 4 0 0 5]
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400) slide(100)
```

To finish we can adjust the synth sound more by creating a so called `super()` synth. This is a synth that plays multiple synths (voices) at the same time in unison with a little detuning creating an interesting effect. For example try:

```java
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400) slide(100) super(0.12 3)
```

This means 3 voices with a small detuning of 0.12. But instead of `(0.12 3)` you can also try other values like `(0.32 5)`. The final result should look something like this:

```java
set tempo 80  
set scale minor d

list hatBeat [1 0.05 1 0.2]
new sample hat_808 time(1/32) play(hatBeat 1) gain(0.7)

list kickBeat [1 0 0 0.7 0 0.9 0 0.2]
new sample kick_808 time(1/8) gain(1) play(kickBeat 1)
new sample snare_808 time(1/2 1/4) speed(1.4)
new sample maracas_808 time(1/2 15/16)

set scale minor d

list progression [0 2]
list bassNotes repeat(progression 16)
new synth saw note(bassNotes 0) time(1/8) shape(1 100) fx(filter low 500)

list melody [3 3 3 10 2 2 2 9 0 0 0 6 4 0 0 5]
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400) slide(100) super(0.12 3)
```

## 3. More Rhythms, Lists & Effects

### List methods
So now that we've made both rhythmical and melodical instruments we can start to work with algorithmic processes (or methods/functions) to let the computer aid us in generating lists that we can use to manipulate parameters in the instruments.

For example we can start by generating a melody for the lead synthesizer with the `spread()` method. The spread method generates a list of `n`-amount of items and spreads the list from the starting to ending value. For example `spread(4 5 0)` will create a list of `4` items, starting at `5` and ending at `0`.

```java
list melody spread(4 5 0)
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400) slide(100) super(0.12 3)
```

At any point you can always see what the output is from a list by using `print` or `view`. `print` will output the result of the list in the console below the texteditor. `view` will give you a visual representation of the values as grayscale colors behind the code editor. This can be useful for longer lists to get an overall idea of what the list looks like at a quick glance.

```java
list melody spread(4 5 0)
print melody
view melody
```

We can now repeat these notes 4 times (or any other value) by applying the `repeat()` function to the melody.

```java
list melody repeat(spread(4 5 0) 4)
print melody
```

This will result in a list that looks like this: `[3 3 3 3 2 2 2 2 1 1 1 1 0 0 0 0]`

### FX

We have already seen that we can adjust the color of the synths by using the `filter`. The filter is one effect that we can use with the `fx()` function. But we can make the sound more interesting and alive by adding various other effects such as reverb (room emulation), delay and saturation (distortion/overdrive). You can apply values to the functions by taste and you can even modulate the parameters of these effects with a (generated) list.

For example we can add a `reverb` to the lead synthesizer to give it the effect of being in a bigger room/hall. We give the arguments `0.5` and `3`. The `0.5` will mix half of the original sound and half of the reverberation sound. The `3` is the reverb time in seconds.

```java
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400) slide(100) super(0.12 3) fx(reverb 0.5 3)
```

We can also add a distortion (`drive`) to the bass synthesizer like so.

```java
new synth saw note(bassNotes 0) time(1/8) shape(1 100) fx(filter low 500) fx(drive 15)
```

If we remove the `shape()` from the bass by adding a value of `-1` to the shape function it will continuously play the sound of the synth. We can then use a Low Frequency Oscillator (`lfo`) effect to let the amplitude be modulated in a specific timing interval by a different type of waveform like a `sine`.

```java
new synth saw note(bassNotes 0) time(1/8) shape(-1) fx(filter low 500) fx(drive 15) fx(lfo 1/8 sine)
```

## 4. Final Result

```java
set tempo 80  
set scale minor d

list hatBeat [1 0.05 1 0.2]
new sample hat_808 time(1/32) play(hatBeat 1) gain(0.7)

list kickBeat [1 0 0 0.7 0 0.9 0 0.2]
new sample kick_808 time(1/8) gain(1) play(kickBeat 1)
new sample snare_808 time(1/2 1/4) speed(1.4)
new sample maracas_808 time(1/2 15/16)

set scale minor d

list progression [0 2]
list bassNotes repeat(progression 16)
new synth saw note(bassNotes 0) time(1/8) shape(-1) fx(filter low 500) fx(drive 15) fx(lfo 1/8 sine)

list melody repeat(spread(4 5 0) 4)
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400) slide(100) super(0.12 3) fx(reverb 0.5 3)
```
