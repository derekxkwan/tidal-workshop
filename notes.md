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

## OTHER CAPABILITIES
- custom OSC message output (link up with `p5.js`)
- midi message output (link up with `Pure Data`)

