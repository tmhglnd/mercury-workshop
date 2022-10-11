
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

### Bass

For the second part we will work on a bass synthesizer and a synthesizer that creates a melodic part. The synth generates a specific type of waveform in a rhythmical pattern and has a so called envelope that starts and stops the sound over a period of time. First we will work on the bass synthesizer by adding a `new synth` of type `saw` that plays every quarter note `time(1/4)` a default `note(0 0)`.

```java
new synth saw note(0 0) time(1/4)
```

we can make it play a bit longer by adding the shape function.

```java
new synth saw note(0 0) time(1/8) shape(1 500)
```

It is quite harsh in terms of color/timber. So what we can do is add a filter to the synth that removes high frequencies above a certain value. We use the `fx()` function for this and as an argument we use `filter`. The type of filter will be `low` which mains only output the low frequencies (lowpass).

```java
new synth saw note(0 0) time(1/8) shape(1 500) fx(filter low 800)
```

Now lets create a list as a rhythm for the bass to play. We add the list as an argument to the `play()` function and set the time to `1/16` to play the list at 4x the speed.

```java
list bassRhythm [1 0 0 1 0 0 1 0]
new synth saw note(0 0) time(1/8) shape(1 500) fx(filter low 800) play(bassRhythm)
```

Now lets create a list of notes for the bass to play. We add the list as an argument to the `note()` function. This list will be the progression of the musical piece. We want to duplicate the values of the list for every note the bass plays, so we will use a list function to transform the list and repeat every note 6 times. We also make sure the part resets after `4` bars in `play()`.

```java
list progression [0 7 3 10]
list bassNotes repeat(progression 6)
new synth saw note(bassNotes 0) time(1/8) shape(1 500) fx(filter low 800) play(bassRhythm 4)
```

### Melody

Now we will work on the melody that plays together with the bass. The melody will be another synth that plays in a higher register (higher notes). First we start by creating another synth that plays every quarter note off-beat but in an octave higher, together with a filter with a higher cutoff frequency. We'll add a `shape()` that holds the note for a period of time and then releases using 3 arguments.

```java
new synth saw note(0 1) time(1/4 1/8) fx(filter low 2000) shape(10 250 10)
```

Now we can add a melody as a list that will be played. Let's make a melody using the progression as well.

```java
list melody repeat(progression 4)
new synth saw note(melody 1) time(1/4 1/8) fx(filter low 2000) shape(10 250 10)
```

To make the sound more interesting we can let the notes `slide()` from one to the other (called portamento).

```java
list melody repeat(progression 4)
new synth saw note(melody 1) time(1/4 1/8) fx(filter low 2000) shape(10 250 10) slide(50)
```

We can set the scale for the whole piece to change the height of all the notes. For example we can `set scale minor e` but then adjust the octave offsets in the notes to drop the tuning in the bass and lead synth.

```java
set scale minor e
```
```java
new synth saw note(bassNotes -1) time(1/8) shape(1 100) fx(filter low 800) play(bassRhythm 4)
```
```java
new synth saw note(melody 0) time(1/8) fx(filter low 2000) shape(1 400) slide(100)
```

To finish we can adjust the synth sound more by creating a so called `super()` synth. This is a synth that plays multiple synths (voices) at the same time in unison with a little detuning creating an interesting effect. For example try:

```java
new synth saw note(melody 1) time(1/4 1/8) fx(filter low 2000) shape(10 250 10) slide(50) super(0.032 3)
```

This means 3 voices with a small detuning of 0.12. But instead of `(0.032 3)` you can also try other values like `(0.32 5)`. The final result should look something like this:

```java
set tempo 125

list hatBeat [1 0.9 1 0.3]
new sample hat_909 time(1/16) play(hatBeat 1) speed(0.4) shape(1 50) gain(0.5)
new sample kick_vintage time(1/2) play(0.85)
new sample snare_ac time(1/2 1/4)
new sample clap_909 time(7/16) play(0.7)
set all fx(reverb 0.3 1.2)

set scale minor e

list bassRhythm [1 0 0 1 0 0 1 0]
list progression [0 7 3 10]
list bassNotes repeat(progression 6)
new synth saw note(bassNotes 0) time(1/16) shape(1 500) fx(filter low 800) play(bassRhythm 4)

list melody repeat(progression 4)
new synth saw note(melody 1) time(1/4 1/8) fx(filter low 2000) shape(10 250 10) slide(50) super(0.032 3)
```

## 3. More Rhythms, Lists & Effects

### List methods

So now that we've made both rhythmical and melodical instruments we can start to work with algorithmic processes (or methods/functions) to let the computer aid us in generating lists that we can use to manipulate parameters in the instruments.

We can start by generating a rhythm for the bass synthesizer without having to type all the numbers ourselves. For instance we can use the `euclid()` method that allows us to generate euclidean rhythms. This is a rhythm where a `x`-amount of hits is evenly distributed over a `y`-amount of steps.

```java
list bassRhythm euclid(8 3)
```

At any point you can always see what the output is from a list by using `print` or `view`. `print` will output the result of the list in the console below the texteditor. `view` will give you a visual representation of the values as grayscale colors behind the code editor. This can be useful for longer lists to get an overall idea of what the list looks like at a quick glance.

```java
list melody euclid(8 3)
print melody
view melody
```

We can now extend the rhythm by for example creating a copy of itself and reserving it. Then appending the result to the original list. This creates a so called palindrome and therefore the function is called `palin()`.

```java
list bassRhythm palin(euclid(8 3))
```

Now that we know we have 6 hits in the rhythm for the bass we can create a new bassline that uses the progression as a startingpoint, but uses other notes to create a relative offset (for example alternating between root and octave). The `clone()` method can take any list as an input and make multiple copies of the list and append it. The intersting thing is that clone can also add a value to the copied list so it can offset all the values. We will use `duplicate()` to append 2 identitical copies to the `[0 12]` list.

```java
list bassRhythm palin(euclid(8 3))
list bassNotes duplicate([0 12] 3)
list bassNotes clone(bassNotes progression)
print bassNotes
```

This will result in a list that looks like this: `[0 12 0 12 0 12 7 19 7 19 7 19 3 15 3 15 3 15 10 22 10 22 10 22]`

### FX

We have already seen that we can adjust the color of the synths by using the `filter`. The filter is one effect that we can use with the `fx()` function. But we can make the sound more interesting and alive by adding various other effects such as reverb (room emulation), delay and saturation (distortion/overdrive). You can apply values to the functions by taste and you can even modulate the parameters of these effects with a (generated) list.

For example we can add a `reverb` to the lead synthesizer to give it the effect of being in a room/hall. We give the arguments `0.5` and `3`. The `0.5` will mix half of the original sound and half of the reverberation sound. The `3` is the reverb time in seconds.

```java
new synth saw note(melody 2) time(1/8) fx(filter low 2000) shape(1 400) slide(100) super(0.12 3) fx(reverb 0.5 3)
```

We can also add a distortion (`drive`) to the bass synthesizer like so.

```java
new synth saw note(bassNotes 0) time(1/16) shape(1 500) fx(filter low 800) play(bassRhythm 4) fx(drive 2)
```

If we remove the `shape()` from the bass by adding a value of `-1` to the shape function it will continuously play the sound of the synth. We can then use a Low Frequency Oscillator (`lfo`) effect to let the amplitude be modulated in different timing intervals by a different type of waveform like a `sine`.

```java
new synth saw note(bassNotes 0) time(1/16) shape(-1) fx(filter low 800) play(bassRhythm 4) fx(drive 2) fx(lfo 3/16 square)
```

## 4. Final Result

When you are done your code may look something like this. But of course it is completely fine if it is something completely different as well!

```java
set tempo 125

list hatBeat [1 0.9 1 0.3]
new sample hat_909 time(1/16) play(hatBeat 1) speed(0.4) shape(1 50) gain(0.5)
new sample kick_vintage time(1/2) play(0.85)
new sample snare_ac time(1/2 1/4)
new sample clap_909 time(7/16) play(0.7)
set all fx(reverb 0.3 1.2)

set scale minor e

list bassRhythm palin(euclid(8 3))
list progression [0 7 3 10]
list bassNotes duplicate([0 12] 3)
list bassNotes clone(bassNotes progression)
new synth saw note(bassNotes 0) time(1/16) shape(-1) fx(filter low 800) play(bassRhythm 4) fx(drive 2) fx(lfo 3/16 square)

list melody repeat(progression 4)
new synth saw note(melody 1) time(1/4 1/8) fx(filter low 2000) shape(10 250 10) slide(50) super(0.032 3) fx(reverb 0.4 3)
```