# TIDALCYCLES INTRO
[https://tidalcycles.org/](https://tidalcycles.org/)

## WHAT IS LIVE CODING
- evaluating code snippets realtime

## WHY LIVE CODING
- tool made to express musical (or visual) ideas quickly (closer to improvisation)
  - does a lot for you (but at the same restricts musical scope)
  - even more so than more general purpose computer music software (Max/MSP, Pure Data, CSound, bare-bones SuperCollider)

## WHAT IS TIDALCYCLES
- a way of generating music through code live created by Alex McLean (yaxu)
- operates on around the basis of "cycles" (musical equivalent to bar/measure)

## COMPONENTS
- Haskell DSL (sends OSC messages to SuperCollider aka `Tidal`)
- SuperCollider + Quark (sound generation aka `SuperDirt`)
- editor plugin (Atom, Vim, Emacs, VS Code, Sublime Text)

## INSTALLATION
###  EVERYTHING (Git, SuperCollider, Haskell, Atom)
#### LINUX 
- =(

#### MAC
- `curl https://tidalcycles.org/tidal-bootstrap.sh -sSf | sh`

#### WIN
- [https://tidalcycles.org/index.php/Windows_choco_install](https://tidalcycles.org/index.php/Windows_choco_install)

### BY STEPS
#### HASKELL
- `cabal update`
- `cabal install tidal`

##### LINUX/MAC/WIN
- [https://www.haskell.org/platform/](https://www.haskell.org/platform/)

##### LINUX ALT.
- `sudo apt install ghc cabal-install`

##### MAC ALT.
- `brew install ghc cabal-install`

#### EDITOR PLUGINS
- [https://tidalcycles.org/index.php/List_of_tidal_editors](https://tidalcycles.org/index.php/List_of_tidal_editors)

#### SUPERCOLLIDER
- [https://supercollider.github.io/](https://supercollider.github.io/)
- `Quarks.gui` -> Check `SuperDirt`
- Note for Linux users, require use of `JACK`

## GET UP AND RUNNING
- Start SuperCollider
- `SuperDirt.start`
- open up a `.tidal` file in your favorite editor
- type in `p 1 $ sound "bd sn"` which will start a nifty bass drum/snare drum loop once executed
- type in `hush` to hush everything once executed

## EDITOR COMMANDS
### EMACS
- start Tidal: `C-c C-s`
- run line: `C-c C-c`
- run multiple lines: `C-c C-e`

### ATOM
- start Tidal: `Packages > TidalCycles > Boot TidalCycles`
- run line: `shift+enter`
- run multiple lines: `(cmd/ctrl)+enter`
