# A CodinGame Haskell Environment

Today, one of the reasons why CodinGame sticks to an old version of Haskell (GHC 8.4.3)
is that the [Haskell Platform](https://www.haskell.org/platform/) stopped providing anything outside the core package.
In turn, the reason why Haskell Platform made this choice is the lack of consensus on which packages must be part of the "platform".
A divise question considering that [Hackage](https://hackage.haskell.org/) could be a wild place with many packages doing the same thing with different approachs.

A leaner way to develop in Haskell is using [Stack](https://docs.haskellstack.org/en/stable/README/).
I won't present it in details here, suffice to say it's a popular tool aiming at reproducible builds
(by managing both the project dependencies and the build tools such as GHC itself).
It is based on [Stackage](https://www.stackage.org/) which is a curated list of packages from Hackage providing a set of versions known to works well with each other.
For instance, the LTS 17.9 from Stackage is the latest stable set of package using GHC 8.10.4.
Stack is not the only tool around however and using Cabal and/or NixOS are legitimate approachs as well.
In fact Stack, Cabal, NixOS and others are not exclusive and can often be combined.
For instance, Stack understands Cabal files (they use the same internal cabal library from GHC after all) and offers a direct support for Nix containerization if needed.

So, what would it look like to use Stack at CodinGame to unlock newer versions of GHC and fine tune the available packages?

First, Stack needs to be installed:

```bash
curl -sSL https://get.haskellstack.org/ | sh
```

If already installed, calling `stack upgrade` will update it if needed.
The former Haskell Platform, or any other tool, is not needed (Stack downloads GHC versions when needed to achieve reproducible builds),
but having it around shouldn't interfere either (Stack being some kind of sandbox in this regard).

This GitHub project could then be used as the Stack project for any player submission:

```
git clone https://github.com/Chatanga/HaskellCodinGamePlayer.git
stack build
stack exec player
```

The first time this project is build could take some times since Stack will download and compile any dependencies
(the Haskell part at least, wrapped native libraries, if any, need to be installed separately, hence the need of things such as NixOS).
Of course, that's just the first time.

The last command launchs the program in a properly configured environnement (a bit like `chroot` for instance).
In the `package.yaml` file (`stack.yaml` simply specifies the LTS version used here) are listed the packages made available for a Haskell submission:

	- ghc
	- array
	- base
	- bytestring
	- containers
	- deepseq
	- hashable
	- integer-logarithms
	- mtl
	- parsec
	- pretty
	- text
	- time
	- transformers
	- unordered-containers
	- vector

That a pretty decent set of package to code a great bot I think.
Of course, other packages could be added if wanted ('linear' would be a great addition for some contests).
