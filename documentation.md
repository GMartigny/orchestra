# Documentation

Orchestra is a musical textual language designed around the idea that creativity is more important than strictness.

Unlike other notation, Orchestra is not meant to generate a music score but music directly, freeing itself from the scores limitations.

> Once you free your mind about a concept of  
> Harmony and of music being correct  
> You can do whatever you want  
> -- Giorgio Moroder


## Example

    "Bella Ciao"
    $ E_ #F_ G_
    $ 120bpm
    || {- B_ E F A E}/2 - :|
    {- B_ E F}/2 A {G F}/2 A {G F}/2 B B B/2
    {B A B}/2 C C {- C B A}/2 C B -
    {A G}/2 F B G F 2E


## Notes

Notes are written using letters from `A` to `G`. The case is insensitive, so `a` is the same as `A`.

    A B C D

Letters are used for, and only for, notes. Spaces are optional and only used for readability, `AAA` equals to `A A A`.

In fact, you can use any letter from `A` to `Z`. It just means that `H` is equivalent to an octave above `A`.


## Rest

Rests are written using the `-` character.

    - B - D

## Timing

A single note `A` last for 1 beat (equivalent to the Quarter note or Crotchet).
Note can be multiplied and divided by integers almost mathematically.

Any integer directly before a note lengthen (multiply) the note duration.

    2A 2B
> This mean that `1A` is equivalent to `A`.

Adding `/` and an integer right after will shorten (divide) the note duration.

    A/2 B/2 C D -/2 F/2
> This mean that `A/1` is equivalent to `A`.

Both can be used simultaneously for more complex timing.

    3A/2 B/2 C 9-/16 7E/16
> This mean that `A` is equivalent to `2A/2`, `3A/3` ...

Using `0` is **not valid**, neither for multiplying nor dividing, `0A` and `A/0` will produce an error.


## Incidentals

To slightly modify the pitch of a note, you can use incidentals.

 - `#` - Sharp, raises by a semitone
 - `&` - Flat, lowers by a semitone
 - `@` - Natural, cancel any previous accidental to this note

Incidentals are placed in front of a note as many time as you want.

    #A B &&C @D

Two semitones equals one tone, this means that `##A` is the same as `B`.
Due to the fact that `#` and `&` cancel each other, `#&&#A` is equal to `A`.
You can use incidentals in any order.
However, natural has a higher priority, cancelling any other incidental. This means that `##@###A` is the same as `A`.

> Note that, due to the limitation of [12-TET](https://en.wikipedia.org/wiki/12_equal_temperament), there's only a half-step between `B`-`C` and `E`-`F`.  
> Therefore, `#B` is the same as `C` and `&C` is the same as `B` (likewise for `E` and `F`).


## Octave

By default, any note is in the "middle C" (C4) octave. Of course, you can change it.

 - `_` - Lower by one octave
 - `^` - Raise by one octave
 - `~` - Return to the default (C4) octave
 
Octaves are defined after the note as many time as you want.

    A_ B C^^ D~
    
Remember `H` we saw above ? well, `H` is the same as `A^`, `I` is the same as `B^` ...

Much like incidentals, octaves can be change in any order and as many times as you want.
In the same way, `_` and `^` cancel each other, `A_^^_` is equal to `A`.
However, return has a higher priority, so `A__~___` is the same as `A`.


## Recap

Any note can have incidentals, a timing, a pitch and octave modifiers in that order.

![example of #2A/3___](./example.png)

 - `#` any number of incidentals (here 1 semitone higher)
 - `2`, `/3` the note timing (here 2/3 of a beat)
 - `A` the note pitch (here 440Hz)
 - `___` the octave modifier (here 3 octaves lower)


## Grouping

You might want to apply a modifier to multiple notes at the same time to reduce repetition.
Tempo, incidentals and octave modifiers can be applied to a group which will be applied to all notes in that group.

### Neutral

Using braces `{}` just serves to group notes without changing the way they are played.

    A_ B_ C_ D_

Can be simplified to:

    {A B C D}_

Braces can be nested as many times as you want.

    A_ B_ #C_ #D_

Can be simplified to:

    {A B #{C D}}_

### Chords

In order to play multiple notes at the same time, you have to group them inside chevrons `<>`.

    <A B 3C> D
> The length in time of the group will be defined by the longest note inside. Here the chord is 3 beats long.

Chevrons **can't** be nested and will produce an error otherwise, `<A <B C>>` is invalid.


### Slurs or legato

Sometimes, you want to slide from one note to the other without interruption. This can be denoted by grouping notes inside parenthesis `()`.

    (A B C) D

Parenthesis **can't** be nested and will produce an error otherwise, `(A (B C))` is invalid.


## Bars

Classics music scores have bars delimiting each measures.
Orchestra allow you to add pipe `|` to your composition, but this is purely decorative and only serves to improve readability.

    || A B C D | E F G H ||


## Repeat

In order to repeat a passage and reduce duplication, you can use a colon `:` sign.
Encountering this sign repeat back until the nearest double-bar `||` or all the way to the start if none present.
You can use as many repeats as you want to increase the number of times the passage is played.

    A B C D | E F G H | E F G H | E F G H

Can be simplified to:

    A B C D || E F G H ::

Repeats don't have to be followed by a bar, but this could be a good idea for clarity.
More complex loops can be written with nested repeats.

    A B | C D | C D | E F | C D | C D | E F |

Can be simplified to:

    A B || C D :| E F :|


## Global

Some effects can be applied until told otherwise. Often found at the start, they can define the tempo or the key signature.
In Orchestra, you can use a dollar `$` at the start of a line to define globals.
Each globals can be written in the same line or with one line for each.

    $ 120
    $ #F
    $ #D

Can be simplified to:

    $ 120 #F #D


### Tempo

Starting a line with a dollar `$` followed by any integer will denote the temp in BPM. You can add the optional keyword `bpm` for better clarity.

    $ 120bpm


### Key signature

Starting a line with a dollar `$` followed by a modified note will apply these modifiers to every instance of that note.
It can be incidentals, a timing, or octave modifiers or all of them at once.

    $ #3F/2_ &D
> Every `F` note will be sharpened, last for 3/2 of a beat and lowered by 1 octave.  
> Every `D` will be flattened.

These modifiers are absolute. They will completely override previous similar globals.

    $ #A
    A B C
    $ &A
    A B C
> The first global sharpen all `A`. The second will not cancel the first one, but override it and flatten all `A`.

To cancel globals, use the related cancel sign (`@`, `~` or `1`).

    $ @A
> Remove all previous global incidentals on `A`.

If you want to apply the modifiers to all pitch, you can use the wildcard sign `*`.

    $ #*
> All notes will be sharpened.

    $ @1*~
> Reset all notes to default.


## Comments

Comments make great code. To insert contextual information, wrap your text in double-quote `""`.
Orchestra will ignore any text between two double-quote.

    "
    Title: My awesome composition
    Author: Me
    Date: 08/25/2020
    "
    || A B C D ||
    "The End"
