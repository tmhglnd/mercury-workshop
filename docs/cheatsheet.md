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

Set the timing (how many times is a sound played in a measure) and offset

```js
// plays the hat_909 8 times per measure
new sample hat_909 time(1/8)

// plays the snare_909 2 times per measure with an offset of 1/4
new sample snare_909 time(1/2 1/4)
```

## tempo

Change the tempo of the entire song in Beats Per Minute (BPM).

```js
set tempo 115

new sample bamboo_g time(1/3)
new sample bamboo_f time(1/4)
```

## scale

Set the scale to map notes to for the entire song. For example `major` or `minor`.

```js
set scale minor d
```

## play

Create a rhythm in a list for the sample or synth. 1 = play, 0 = don't play. Use a decimal value for a percentage of chance that the sound will be played.

```js
list violinRhythm [1 0 0 1 0]

new sample violin_c time(1/8) play(violinRhythm)

list pianoRhythm [0.9 0.1 0.5]
new sample piano_c time(1/8) play(pianoRhythm)
```

## shape

Change the length that the `sample` or `synth` will play (in milliseconds or division). This functions as a fade-in and fade-out.

```js
// the choire_01 will play for 104 ms, with 2 ms fade-in en 2 ms fade-out
new sample choir_01 time(1/8) shape(2 100 2)

// the saw will play for 501 ms, with 1 ms fade-in en 500 ms fade-out
new synth saw time(1/4) shape(1 500)

// the square will fade in over a quarter-note and fade-out over a quarter note
// the length depends on the tempo
new synth square time(1/1) shape(1/4 1/4)
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

Change the pitch of the `sample` or `synth` and also change the octave (second argument), allowing to create melodies. Combine with `tune()` in the sample if your sound is tuned to a different note than a `c`.

```js 
list melody [0 3 7 3 9 3 0]
new synth triangle note(melody 2) time(1/16)

new sample piano_f note(melody 1) time(1/4) tune(f3)
```

## fx

Add sound effects to the sample or synth like a reverb or distortion

```js 
// the harp sound has a distortion and reverb effect
new sample harp_down time(2) fx(drive 15) fx(reverb 0.5 2) 
```

Try fx like: `delay`, `chorus`, `filter`, `triggerFilter`, `degrade`. See the documentation or tutorials for further explanation.

## rhythm lists

Create lists for rhythms with functions like coin, euclidean rhythm and more.

```js 
list tRhythm1 coin(8) // random 1 or 0
new sample tabla_hi time(1/16) play(tRhythm1)

list tRhythm2 euclidean(16 7) // evenly distribute 7 1's over 16 0's
new sample tabla_mid time(1/16) play(tRhythm2)

list tRhythm3 clave(8) // generate a clave rhythm
new sample hat_808 time(1/16) play(tRhythm3)

list tRhythm4 hex('89ac') // generate a rhythm from hexadecimal values
new sample maracas_808 time(1/16) play(tRhythm4)
```

Try functions like: `coin()`, `euclidean()`, `hexBeat()`, `clave()`. See the documentation or tutorials for further explanation.

## note lists

Create lists for notes with functions like spread, choose, lookup and more. 

```js 
list scale [0 2 3 5 7 9 11 12]
list notes choose(7 scale) // choose randomly 7 values from the list

new synth triangle time(1/16) note(notes 1)

list notes2 spread(5 0 12) // 5 numbers starting at 0 up to 12 evenly spaced
new synth sine time(1/16) note(notes2 1)

list notes3 repeat([0 3 2] 8) // all notes from the list repeated 8 times
new synth sine time(1/16) note(notes3 2)
```

Try other functions like: `sine()`, `cosine()`, `lookup()`, `drunk()`. See the documentation or tutorials for further explanation.

## lists in general

Basically any parameter can also be a list of values that will be iterated over. For example in `fx()`

```js
list drives [1 5 10]
new sample kick_909 time(1/4) fx(drive drives)
```

## combining list functions

By combining list functions you can generate more complex structures.

```js
set scale dorian c
list notes add(repeat(clone(palin(spread(4 0 12)) 0 5 3 2) 4) random(4))
print notes // use print to see the output from this list combination

new synth saw note(notes 1) time(1/16) shape(1 1/16)
```

## generating chords

To create chords we use the polyphonic instruments `polySample` or `polySynth`. These are able to play maximum of 8 sounds at the same time. With `makeChords` we can generate a 2d list of notes that will be played at the same time.

```js
set scale major c
list chords makeChords(I IV VI V)
print chords

new synth sine note(chords 2) time(1/2) shape(1/4 1/4)
```