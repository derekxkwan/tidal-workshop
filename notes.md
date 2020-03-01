# TIDALCYCLES NOTES
- You've just executed `p 1 $ sound "bd sn"`, what just happened?
- `p 1` specifies the **p**attern named 1
  - can be named anything, even strings (try replacing `p 1` with `p "jamz"`)
- `sound "bd sn"` specifies the pattern will be comprised of the **sound**s `bd` and `sn` with `bd` coming at the beginning of the cycle and `sn` coming halfway through
  - `s` is shorthand for `sound` (try replacing `sound "bd sn"` with `s "bd sn"`
  - what happens when we change `"bd sn"` to `"bd sn cp"`?
- `hush`ing individual patterns: `silence`
  - `p 1 $ silence`

## MINI NOTATION
- grouping `[]` (`.` shorthand)
  - `s "bd [sn ho]"`
  - equivalent: `s "bd . sn ho"`
- rests `~`
  - `s "bd [~ ho]"`
- hold `_`
  - `s "bd [_ ho]"`
- fast (repeat) `*` (think of `sn` as its own little cycle)
  - `s "bd [sn*2 ho]"`
  - equivalent: `s "bd [sn sn . ho]"`
- replicate `!`
  - `s "bd [sn ! . ho]"`
  - `s "[bd !] [sn!4 . ho]"`
	- equivalent: `s "[bd bd]  [sn sn sn sn . ho]"`
 - 	- equivalent: `s "[bd bd]  [sn ! ! ! . ho]"`
- slow `/` (think of `bd` as its own little cycle)
  - `s "bd/2"`
- alternate `<>`
  - `s "<bd cp> sn"`
- chance `?` (50%)
  - `s "bd? sn"`
- layering `,`
  - `s "bd [sn, cp]"`  

## TACKING ON EFFECTS PARAMETERS
- things that affect the output of the pattern
- `#`
- let's try tacking on a low-pass filter
  - `p 1 $ s "bd sn" # lpf 250`
- let's try out reducing the volume
  - `p 1 $ s "bd sn" # gain 0.5`
- now let's try out reducing the volume and distorting the heck out of it
  - can tack on multiple parameters
  - `p 1 $ s "bd sn" # gain 0.5 # shape 0.75`
- in general, these parameters specified with `#` goes to the **right** of the pattern string

## MODIFYING PATTERNS
- things that affect/define the structure of the pattern
- `$`
  - in `p 1 $ s "bd sn"`, `s "bd sn"` defines the pattern structure
- let's drop things out of the pattern by chance
  - `p 1 $ degradeBy 0.25 $ s "bd sn"`
- let's make the pattern go twice as fast (`*`)
  - `p 1 $ fast 2 $ s "bd sn"`
- let's add a copy of the pattern that goes twice as fast and has things drop out randomly
  - `p 1 $ superimpose (fast 2 . degradeBy 0.25) $ s "bd sn"`
  - note that in the parentheses, we string together `$` type things with `.`
  - `#` requires the `#` and parentheses and the `.` (`superimpose (fast 2 . (# lpf 100))`)
- in general, these **functions** go to the **left** of the pattern string

## NOTES ON PATTERN STRINGS
- don't just have to be centered around `s`
- let's make a pattern around the volume of snare drum hits
  - `p 1 $ gain "1 0.8 ! !" # s "sn"`
  - note that since `gain` is defining the pattern structure now, `s` essentially becomes a parameter
- what happens if we get a bit more complicated with the second pattern string?
  - `p 1 $ gain "1 0.8 ! !" # s "bd sn"`
  - the `gain` bit with `$` **defines** the pattern structure, the `s` bit with `#` fits itself around it
  - what happens if we try `p 1 $ gain "1 0.8 ! !" $ s "bd sn"`?
- other fun stuff: `note` (defined in semitones), `speed` (defined in proportions)
  - `p 1 $ speed "2 1 1 1" # s "hh"`

## MODIFYING EFFECTS PARAMETERS
- `|*`, `|+`, `|-`
- let's take the `speed` example from before but we want to base these speeds on a `0.75` speed `hh`
  - `p 1 $ speed "2 1 1 1" |* speed 0.75 # s "hh"`
  - order is a bit more flexible: `p 1 $ speed "2 1 1 1" # s "hh" |* speed 0.75`
- `|` points in the direction of where our pattern structure is coming from (from the left)
  - also `*|`, `|*|` but more confusing and don't really use those...
- like `#`, needs to be nestled within parentheses with functions involving parentheses
  - `p 1 $ superimpose (fast 2 . (|* speed 2)) $ speed "2 1 1 1" # s "hh"`

## SAMPLE MANIPULATION
- use `legato 1` or `cut 1` for making sure sample segments don't bleed over (the int in `cut` groups together things to cut)
- the parameters `begin` and `end` (helpful when reversing a sample ie `# speed (-1)`) control the start/end points of a sample and range from 0 to 1
- `slice` can also be used as a function to slice up samples
  - `p 1 $ slice 32 "0 3 10 20" $ s "bev"`
- `sustain` can be used to cut sample short
  - great for glitchy sorta drummy sounds, esp combined with `note` or `gain`
  - `p 1 $ note "0 [~ 12*3] 7*2 [~ 17 . ~ 12]" # s "bd" # sustain 0.005`
- `chop`, `gap`, `striate` granulate (chop into very small bits) samples in various ways
- custom sound files (`.wav` or `aiff.`) can be loaded with `~dirt.loadSoundFiles("/path/to/folder/*");`
  - sounds are specified in pattern strings by the subfolders that contain them
	- `/path/to/folder/neat/sound.wav`is specified by `neat` (`s " bd neat hh neat"`)
 - individual sound files in the example `neat` folder are picked out by `:` (not sure of ordering, 0 indexed)
   - `s "neat neat:1 neat:2 neat:3"` 

## SYNTHS
- synthesizers can also be specified with `s`!
- some synths: `supermandolin` (karplus), `superpiano`, `supersaw`, `superfork`, `superchip`,...
  - each has their various parameters
- pitches are specified by `note` (`n` for short), are numbered chromatically, and centered around C4
  - `n` can also take note names (`c`, `c4` to specify octave, `cs4` where s is sharp, `cf4` where `f` is flat)  
- `scale (scale name) (pattern string)` can be passed to n (where the numbers refer to scale degrees)
  - `p 1 $ n (scale "major" "0 2 4 6") $ s "superpiano"`
  - there are many many scales built in: `major`, `minor`, `lydian`, `dorian`, `mixolydian`, etc., ...
- chords can be direcly passed into `n` and are tacked onto note names with `'`
  - ie `c'maj`, `c4'sus4`, `df3'min7`,...
  - there are many chords: `maj`,`min`,`dom7`, `maj9`, `7f5`, etc., ...
  - chords can also be `arp`eggiated!
	- `p 1 $ n (arp "c4'maj7 a3'min7")`
	- possible arpgegiations include `up`, `down`, `updown`, `converge`, `thumbup`, etc.,...

## ASSORTED EFFECTS
- delay with feedback: `delayfb`, `delay` (for amount of delay signal), `delaytime` (units in seconds, helpful to use with `# lock 1` to lock to cycle units)
- - `delayfb`, `delay` go from 0 to 1
- reverb: `room` (initial reflections), `size` (late reflections), `dry`(amount of clean signal to go through)
- distortion: `shape (0-1)`
- bitcrushing: `crush (int)` where int is number of bits 
- sample rate reduction: `coarse (int)`where int is amount of rate reduction
- filters: `lpf`, `bpf`, `hpf`for low-pass/bandpass/hi-pass, `vowel` for vowel formant filters
- NOTE: effects like delay and reverb (and `leslie`) are "global" effects, they affect all synths within a given group
  - specify groups with `orbit (int)`(0 default)

## CHANCE FUNCTIONS
- execute things by chance!
- `sometimesBy (f)` to execute things by chance on a event-by-event basis
  - `p 1 $ sometimesBy 0.25 (|+ note 12) $ note "0 7 12" # s "superpiano"`
- `someCyclesBy (f)` to execute things by change on a cycle-by-cycle basis

## CONTINUOUS FUNCTIONS
- functions that don't have discrete steps (and so using them to structure patterns makes things go really fast)
  - can be made discrete with `segment (int)` which discretizes the function into given steps
- usually used as arguments for parameters (`# pan (rand)`)
- `rand` for float randomness 0-1 (pair with `range (lo) (hi)` to map to a specific range)
- `irand (int)` for random ints from 0 to n-1
- `saw` for ramp 0-1 over cycle (`isaw` for the other way around)
- `square` for switching from 0 to 1 midway through cycle
- `sine`/`cosine` for their respective ranges
- `tri` for linearly ramping from 0 to 1 to 0 over a given cycle
- NOTE: can use `slow` and `fast` to change the rates of these functions!

## OTHER RANDOMNESS
- all continuous
- `choose [a]` to choose elements from array
- `wchoose [a]` to choose by given weights in the format `(choice, weight)`
-`cycleChoose [a]` makes a choice per cycle and thus **not continuous**!

## SOME TRANSITIONS
- used for not changing patterns immediately
- take the form `(transition) (pattern name) (number of cycles)`
- transitions start when executed but have forms followed by `'` that aligns their execution to the next cycle start
- `xfadeIn`/`xfadeIn'` for crossfades, `jumpIn`/`jumpIn'` for immediate switches `clutchIn` for pattern xfades (not gain-based)...

## RANDOM HASKELL NOTES
- `$` doubles as putting everything to the right in parentheses
- negative numbers require parentheses
  - `# speed -1` doesn't work but `# speed (-1)` does!

## OTHER CAPABILITIES
- custom OSC message output (link up with `p5.js`)
- midi message output (link up with `Pure Data`)

