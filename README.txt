==============================================================================
PRICESYLO - QUICK START
==============================================================================

Thank you for buying PriceSylo. You have two programs: the ENGINE, which
runs on your own computer and talks to the exchanges directly, and the
PANEL, the desktop app you actually look at. There is no cloud behind it -
your market data, your exchange keys and your notes stay on your machine.

Downloads for every platform:  https://github.com/understandingx/pricesylo/releases/v1.0.2
Website, documentation and support:  https://pricesylo.com

This is the short version. The full manual ships inside the download, as
README.md beside the engine.


------------------------------------------------------------------------------
WHAT IS IN THE DOWNLOAD
------------------------------------------------------------------------------

There is one folder per operating system: macOs, Windows and Linux. Open
the one for your computer. Inside each you will find:

  Panel/                     the desktop app installer
  the engine                 one file - see below
  config.json                the engine's settings
  LICENSE                    your licence agreement
  README.md                  the full manual
  SHA256SUMS-pricesylo.txt   checksums, for the optional check at the
                             end of this page

There is one engine for everyone. What your licence allows is decided by
your licence key, not by a different download.

The panel installer, inside Panel/:

  macOS      the file ending in .dmg  (works on Apple Silicon and Intel)
  Windows    the setup file ending in .exe
  Linux      .AppImage, .deb or .rpm - whichever suits your system

The engine:

  macOS      PriceSylo-universal     (Apple Silicon and Intel)
  Windows    PriceSylo-win-x64.exe
  Linux      PriceSylo-linux-x64

The engine is a single file with everything it needs inside it - there is
nothing else to install, not even Node.js. Keep it together with
config.json, LICENSE and README.md in one folder you can write to, for
example Documents/PriceSylo. The engine keeps its settings and everything
it saves in that folder, so a read-only disk will not work.


------------------------------------------------------------------------------
STEP 1 - START THE ENGINE
------------------------------------------------------------------------------

macOS

  1. Put the engine and its files in a folder you can write to.
  2. Double-click PriceSylo-universal. A Terminal window opens and the
     engine starts in it.
  3. The first time, macOS refuses to open it, because the engine is not
     signed by Apple yet. Open System Settings, then Privacy & Security,
     scroll to the bottom and click Open Anyway next to PriceSylo, then
     confirm. You only do this once.
     If macOS opens the file as a document instead of running it, open
     Terminal in that folder and type:
        chmod +x PriceSylo-universal
        ./PriceSylo-universal

Windows

  1. Put the engine and its files in a folder you can write to.
  2. Double-click PriceSylo-win-x64.exe. A console window opens and the
     engine starts in it.
  3. Windows may say "Windows protected your PC". Click More info, then
     Run anyway. This is a signature check, not a virus warning.

Linux

  1. Put the engine and its files in a folder you can write to, and open
     a terminal there.
  2. Make the engine executable and start it:
        chmod +x PriceSylo-linux-x64
        ./PriceSylo-linux-x64

The first time it starts, the engine asks one question:

  This engine will not start without a licence.

    1) Use a license key
    2) Test the product as a demo
       (each run lasts 30 minutes, then the engine stops and clears its
       market data)

  - Choose 1 and paste the licence key that was emailed to you after
    checkout. It looks like LX-XXXXX-XXXXX-XXXXX-XXXXX. Press Enter. The
    key is saved, so later starts go straight in.
  - Choose 2 to try PriceSylo first. The engine remembers that choice and
    starts as the demo each time after that.

The first start can take a minute or more while market data warms up.
That is normal.

THE ENGINE RUNS IN THAT WINDOW. Keep the window open for as long as you
use PriceSylo. Closing the window, or pressing Ctrl+C in it, stops the
engine. It does not start by itself when you log in: next time, start it
the same way.


------------------------------------------------------------------------------
STEP 2 - INSTALL AND OPEN THE PANEL
------------------------------------------------------------------------------

  macOS      open the .dmg and drag PriceSylo into your Applications
             folder, then open it. The panel is signed and notarised by
             Apple, so it opens normally - macOS only asks you once to
             confirm that you want to open an app from the internet.
  Windows    run the .exe setup and follow it. If Windows says "Windows
             protected your PC", click More info, then Run anyway.
  Linux      install the .deb or .rpm with your package manager, or
             mark the .AppImage executable and run it.

Open the panel. It finds the engine on this computer by itself and starts
drawing charts.

If it says the engine is offline, look inside the panel at Help, then the
Engine tab: it shows what the engine is doing right now, and its log.


------------------------------------------------------------------------------
STEP 3 - CONNECT AN EXCHANGE
------------------------------------------------------------------------------

Charts and analysis work with no account anywhere. To see your own
balances and place orders, add your exchange API keys in the panel's
encrypted vault. Keys are entered in the PANEL, never in the engine and
never in config.json, and they stay encrypted on your machine.


------------------------------------------------------------------------------
EVERY DAY
------------------------------------------------------------------------------

  1. Start the engine, as in Step 1, and leave its window open.
  2. Open the panel.

When you are done, close the engine's window to stop it. Only one engine
can run on a computer at a time.


------------------------------------------------------------------------------
IF SOMETHING GOES WRONG
------------------------------------------------------------------------------

The engine will not accept your licence key
  The engine says why - for example the key has expired, or it is already
  in use on all the computers your licence allows. Check the key in your
  purchase email, or contact support.

The engine stops after exactly 30 minutes
  It is running as the demo. To move to your licence key, contact
  support.

The panel says the engine is offline
  Check that the engine's window is still open and running. If you closed
  it, start the engine again (Step 1).

The engine says the address or port 8420 is already in use
  Another copy of the engine is already running. Close the other window
  first.

A VPN or firewall is running
  The panel talks to the engine at 127.0.0.1 port 8420, which is this
  computer talking to itself and never leaves it. The engine does need
  outbound internet to reach the exchanges and to check your licence.

Where to look
  The engine's window shows what it is doing, live. If the engine ever
  crashes, the reason is written to error.log, in the same folder as the
  engine.


------------------------------------------------------------------------------
REMOVING PRICESYLO
------------------------------------------------------------------------------

The engine installs nothing on your computer. To remove it, close its
window and delete the folder you put it in - that removes the engine
together with its settings, saved data and licence record. If you are
moving your licence to another computer, contact support.

Remove the panel the normal way for your system - on macOS, drag
PriceSylo from Applications to the Bin.


------------------------------------------------------------------------------
CHECK YOUR DOWNLOAD (OPTIONAL)
------------------------------------------------------------------------------

Each operating system's folder has a SHA256SUMS-pricesylo.txt listing the
engine, config.json, README.md and LICENSE. In a terminal in that folder,
on macOS or Linux, run:
  shasum -a 256 -c SHA256SUMS-pricesylo.txt
Every line should say OK. If one does not, do not run the file.

On Windows, in PowerShell in that folder, run:
  Get-FileHash PriceSylo-win-x64.exe -Algorithm SHA256
and compare the result with the first line of SHA256SUMS-pricesylo.txt.


------------------------------------------------------------------------------
SUPPORT AND LICENCE
------------------------------------------------------------------------------

Website and documentation:  https://pricesylo.com
Support:                    support@pricesylo.com
Licence terms:              the LICENSE file beside your engine

PriceSylo is published by Laxtic Software Services.
