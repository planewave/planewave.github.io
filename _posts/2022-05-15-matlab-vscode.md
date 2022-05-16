---
title: 'Matlab Development in Visual Studio Code'
date: 2022-05-15 20:00:00
categories: [Development Tools]
tags: [matlab, vscode]
math: false
# mermaid: true
---

I almost use Matlab daily, and I see it is evolving in every release.
However, I always have some complaints about its outdated GUI:
The heavy toolbar reminds me of the MS office 2007.
I know it can be hidden, but these useful buttons will be hidden too.
In addition, there is no dark theme, making it difficult to work at night.
In this post, I will show you some alternative ways to use Matlab.

## Matlab IDE

The *Matlab* we are talking about has multiple meanings: the programming language with an interpreter, and the Matlab desktop with a graphical user interface (GUI).
If you are familiar with other programming languages, you will know that such an integrated development environment (IDE) is not always necessary, and there may be many IDEs available.

To start Matlab without the desktop (Linux and macOS), you can use the command line:

```bash
matlab -nodesktop
```

Then you will enter the Matlab interactive terminal.
For more startup options, for example `-nojvm`, see the [Matlab documentation](https://www.mathworks.com/help/matlab/startup-and-shutdown.html).

My point is that the Matlab desktop is not necessary to run Matlab scripts, and we can use a third-party IDE to run Matlab scripts.
As for 2022, the [VSCode](https://code.visualstudio.com/) is de facto the most popular IDE for coding in multiple languages.

## VSCode extensions for Matlab

To make Matlab work in VSCode, you need to install some extensions.
There is a one-click solution called [Matlab Extension Pack](https://marketplace.visualstudio.com/items?itemName=bat67.matlab-extension-pack).

![Matlab Extension Pack](/assets/img/posts/Matlab_extension.png)

It includes 6 extensions, some of the important ones are:

* MATLAB for Visual Studio Code: basic language support for MATLAB to VSCode. You will need to set up the linter (`mlint`) to make the most of it.
* Matlab Code Run: run Matlab scripts in VSCode. Open the command palette (under "View" or with shortcut ctr+shift+p) and find the "Run Matlab File" command.
* Matlab Interactive Terminal for VSCode. This extension allows you to start an interactive Matlab session in VSCode like other terminals. MATLAB Engine API for Python, installations instruction is available [here](https://www.mathworks.com/help/matlab/matlab_external/install-the-matlab-engine-for-python.html). I had some struggles with this extension installation on macOS, hope you can pull it out.
  
These are unofficial extensions and many critical functions are missing.
There is no debug support, and no variable window (workspace), so it is not meant for serious work.
I heard rumors that Matlab is going to remake its GUI in a future release, and the Matlab online has already shipped with a dark theme (but still the old GUI layout).
I hope they can have a VSCode-like GUI layout, and get rid of the JVM if possible.

![Matlab online](/assets/img/posts/matlab_online.png)

All right, that is all for this post. Thank you for reading.
