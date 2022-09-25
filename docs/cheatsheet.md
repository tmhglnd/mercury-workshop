# Cheatsheet

A list of most common functions with a small example.

## sample

Play a sound (see the [sound database](https://github.com/tmhglnd/mercury/tree/master/mercury_ide/media#readme))

```js
new sample kick_909
```

## synth

Play a synth with a specified waveform (saw, sine, square, triangle)

```js 
new synth saw
```

## time

Set the timing (how many times is a sound played in a measure)

```js
// plays the hat_909 8 times per measure
new sample hat_909 time(1/8)

// plays the snare_909 2 times per measure with an offset of 1/4
new sample snare_909 time(1/2 1/4)
```

## tempo

Change the tempo of all the entire song. In Beats Per Minute (BPM).

```js
set tempo 115

new sample bamboo_g time(1/3)
new sample bamboo_f time(1/4)
```

## play

Create a rhythm in a list for the sound to be played (or not). 1 = play, 0 = don't play. Use a decimal value for a percentage of probability that the sound will be played.

```js
list violinRhythm [1 0 0 1 0]

new sample violin_c time(1/8) play(violinRhythm)

list pianoRhythm [0.9 0.1 0.5]
new sample piano_c time(1/8) play(pianoRhythm)
```

## shape

Change the length that the `sample` or `synth` will play (in milliseconds). This functions as a fade-in and fade-out.

```js
// the choire_01 will play for 104 ms, with 2 ms fade-in en 2 ms fade-out
new sample choir_01 time(1/8) shape(2 100 2)

// the saw will play for 501 ms, with 1 ms fade-in en 500 ms fade-out
new synth saw time(1/4) shape(1 500)
```

## speed

Change the playbackrate (speed) of the `sample`, this stretches or shrinks the sound and therefore it changes the pitch because it is played slower or faster.

```js
// the choir_o is played 4 times slower 
new sample choir_o time(2) speed(0.25)

// the snare_909 is played 2 times faster
new sample snare_909 time(1) speed(2)
```

## note

Change the pitch of the `synth` and also change the octave (second argument), allowing to create melodies

```js 
list melody [0 3 7 3 9 3 0]
new synth triangle note(melody 2) time(1/16)
```

## fx

Add sound effects to the sample or synth like a reverb or distortion

```js 
// the harp sound has a distortion and reverb effect
new sample harp_down time(2) fx(reverb 0.5 2) fx(drive 40)
```

## rhythm lists

Create lists for rhythms with functions like flipping a coin, euclidean rhythm and more.

```js 
list tRhythm1 coin(8)
new sample tabla_hi time(1/16) play(tRhythm1)

list tRhythm2 euclidean(16 7)
new sample tabla_mid time(1/16) play(tRhythm2)
```

Try functions like: `coin()`, `euclidean()`, `hexBeat()`, `clave()`

## note lists

Create lists for notes with functions like spread, choose, lookup and more.

```js 
list scale [0 2 3 5 7 9 11 12]
list notes choose(7 scale)

new synth triangle time(1/16) note(notes 1)
```

Try other functions like: `sine()`, `cosine()`, `lookup()`, `drunk()`

## lists in general

Basically any parameter can also be a list of values that will be iterated over. For example in `fx()`

```js
list drives [1 5 10]
new sample kick_909 time(1/4) fx(drive drives)
```