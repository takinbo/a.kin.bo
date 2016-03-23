---
title: Compiling cpp-ethereum on old MacBook Pros
description: You need qt 5.5 to compile cpp-ethereum on OSX
date: 2016-03-23 05:16:00 +0100
author: takinbo
---

I happen to still use a 2010 Macbook Pro for all my work and recently I've been researching dapps on the [Ethereum](https://ethereum.org/) blockchain. In attempting to have the full experience, I've installed [go-ethereum](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Mac) and attempted (without success) to install [cpp-ethereum](http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/index.html) as well.

While the precompiled OSX release should work, it crashes everytime I open it on my old MBP. The error is somewhat due to an illegal instruction - my guess is that, the release was compiled on more recent hardware with optimizations that use a more recent instruction set than that on mine. My last option was to compile it.

While the [instructions](http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/building-from-source/osx.html) for compiling cpp-ethereum are pretty straight forward, what it doesn't tell you is that it would not work with the most recent version of qt5 (5.6 as of this writing); what you need is version 5.5. So rather than installing the most recent version of qt5, you should do the following instead:

``` bash
brew install https://raw.githubusercontent.com/Homebrew/homebrew/f802822b0fa35ad362aebd0101ccf83a638bed37/Library/Formula/qt5.rb
```

Then you can proceed with the remaining steps to compile the package.
