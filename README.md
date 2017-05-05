# JohnIsWrongAboutFlow_TestCase
I wrote bad `flow` code somehow, and because I'm new I'm blaming the tool.  Please explain why I'm wrong

## ... the fuck?
So, I'm doing it wrong, as hard as I can.  Awesome.

Brand new to `flow`.  Lovely work.

I wrote a bunch of types inline, and all was well.  Then, eventually there were too many, so I decided to break them out 
into their own space.  Time to `import type {foo} from './bar';`, yes?

Cue `Infinity` hours of trying to find a variant of that, or relevant filenames or paths, which works.  In turn, ask IRC a 
bunch.  Day and a half of nope later, and a kind stranger points out that I forgot to validate `flow`'s claim of Windows 
support.  He tries it on his mac, and...

I seem to have stumbled across a pretty significant difference in behavior between my mac and my windows machine.  It's not 
clear whether I've just installed on the windows machine incorrectly, whether this is undefined behavior because my reduced 
test case is wrong, whether I found a bug, or what.

Pro tip: it's always because the noob's code is wrong.  ***Always***.  (And yet he proceeds.)

### wrong

The code in this repo, on a Windows MinGW64 console, like you get from `git bash`:

```
User@DESKTOP MINGW64 ~/Local Settings/lxss/home/john/projects/JohnIsWrongAboutFlow_TestCase (master)
$ flow --color=never
source.js:4
  4: import type { Suit } from './types';
                               ^^^^^^^^^ ./types. Required module not found
```

### wrong

The code in this repo, from a Windows cmd prompt:

```
C:\Users\User\Local Settings\lxss\home\john\projects\JohnIsWrongAboutFlow_TestCase>flow
source.js:4
  4: import type { Suit } from './types';
                               ^^^^^^^^^ ./types. Required module not found
```

### ... right?

The code in this repo, on a mac:

```
John-Haugeland:JohnIsWrongAboutFlow_TestCase johnhaugeland$ flow
source.js:9
  9: ex("Harts");
        ^^^^^^^ string. This type is incompatible with the expected param type of
  6: function ex(blah: Suit) { console.log('suits'); }
                       ^^^^ string enum
```

## wat
And, I mean, it's a trivially simple test case, same-dir, empty standard `.flowconfig`

180% confuse and le rising

### types.js
```javascript
// @flow
export type Suit = "Diamonds" | "Clubs" | "Hearts" | "Spades";
```

### source.js
```javascript
// @flow

import type { Suit } from './types';

function ex(arg: Suit) { console.log(`suits ${arg}`); }

ex("Hearts");
ex("Harts");
```

### .flowconfig
```
[ignore]

[include]

[libs]

[options]
```

![](http://i2.kym-cdn.com/photos/images/original/000/275/025/866.jpg)

And, I mean.  It seems like that should work identically on all supported platforms, 

... so ...

## Honesty
I got help with this test case, which is why it doesn't look like the rest of my code, but that person is being modest and 
doesn't want credit.  Thank you, (you know who you are.)  It's difficult to produce reductions this small.
