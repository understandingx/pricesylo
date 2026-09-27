==============================================================================
PRICESYLO - QUICK START
==============================================================================

Thank you for buying PriceSylo. PriceSylo is two programs working
together: the ENGINE, which runs on your own computer and talks to the
exchanges directly, and the PANEL, the desktop app you actually look at.
There is no cloud behind it - your market data, your exchange keys and
your notes stay on your machine.

On macOS and Windows you install ONE app: the engine is inside it, and the
app starts and stops it for you. On Linux the engine and the app are two
separate downloads, as before.

Downloads for every platform:  https://github.com/understandingx/pricesylo/releases/downloads-20260927-160513
Website, documentation and support:  https://pricesylo.com


------------------------------------------------------------------------------
WHAT IS IN THE DOWNLOAD
------------------------------------------------------------------------------

There is one folder per operating system: macOs, Windows and Linux. Open
the one for your computer.

  macOs      PriceSylo-App-<version>-mac-arm64.dmg   Apple Silicon (M1 and later)
             PriceSylo-App-<version>-mac-x64.dmg     Intel Macs
             SHA256SUMS.txt                          checksums (optional check)
             Not sure which Mac you have? Apple menu > About This Mac: "Chip
             Apple M..." is Apple Silicon; "Processor Intel..." is Intel.

  Windows    PriceSylo-App-<version>-win-x64-setup.exe
             SHA256SUMS.txt

  Linux      Panel/                     the desktop app: .AppImage, .deb or .rpm
             PriceSylo-linux-x64        the engine, one file
             config.json                the engine's settings
             LICENSE                    your licence agreement
             README.md                  the full engine manual
             SHA256SUMS-pricesylo.txt   checksums (optional check)

There is one engine for everyone. What your licence allows is decided by
your licence key, not by a different download.


------------------------------------------------------------------------------
macOS AND WINDOWS - INSTALL AND OPEN
------------------------------------------------------------------------------

  macOS      open the .dmg for your Mac and drag PriceSylo into your
             Applications folder, then open it. macOS asks you once to
             confirm that you want to open an app from the internet. If it
             says the developer cannot be verified, open System Settings,
             then Privacy & Security, scroll down and click Open Anyway.
  Windows    run the setup .exe and follow it. If Windows says "Windows
             protected your PC", click More info, then Run anyway. This is a
             signature check, not a virus warning.

Open PriceSylo. It starts its engine by itself - there is nothing else to
run and no window to keep open. The first start can take a minute or more
while market data warms up. That is normal.

The first time, PriceSylo asks for your licence:

  - Choose "Enter licence key". The app opens Help > Engine: paste the key
    that was emailed to you after checkout (it looks like
    LX-XXXXX-XXXXX-XXXXX-XXXXX) and activate it. The engine starts as soon
    as the key is accepted, and later starts go straight in.
  - Or choose "Try the 30-minute demo" to try PriceSylo first. Each demo
    run lasts 30 minutes, then the engine stops and clears its market data.

When you quit PriceSylo, its engine stops with it. If you already run an
engine of your own on this computer (for example the Linux-style engine on a
server setup), the app uses that one instead and leaves it running.

The engine keeps its settings and everything it saves in your user folder:

  macOS      ~/Library/Application Support/PriceSylo
  Windows    %LOCALAPPDATA%\PriceSylo


------------------------------------------------------------------------------
LINUX - STEP 1, START THE ENGINE
------------------------------------------------------------------------------

The engine is a single file with everything it needs inside it - there is
nothing else to install, not even Node.js. Keep it together with
config.json, LICENSE and README.md in one folder you can write to, for
example ~/PriceSylo. The engine keeps its settings and everything it saves
in that folder, so a read-only disk will not work.

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
    starts as the demo each time after that. To be asked again and use a
    licence key instead, close the engine, delete the file
    .engine-state/license/run-mode.json in its folder (a hidden folder), and
    start it again.

The first start can take a minute or more while market data warms up.
That is normal.

THE ENGINE RUNS IN THAT TERMINAL. Keep it open for as long as you use
PriceSylo. Closing it, or pressing Ctrl+C in it, stops the engine. It does
not start by itself when you log in: next time, start it the same way.


------------------------------------------------------------------------------
LINUX - STEP 2, INSTALL AND OPEN THE PANEL
------------------------------------------------------------------------------

Install the .deb or .rpm from Panel/ with your package manager, or mark the
.AppImage executable and run it. Open the panel. It finds the engine on
this computer by itself and starts drawing charts.

If it says the engine is offline, look inside the panel at Help, then the
Engine tab: it shows what the engine is doing right now, and its log.


------------------------------------------------------------------------------
CONNECT AN EXCHANGE
------------------------------------------------------------------------------

Charts and analysis work with no account anywhere. To see your own
balances and place orders, add your exchange API keys in the panel's
encrypted vault. Keys are entered in the PANEL, never in the engine and
never in config.json, and they stay encrypted on your machine.


------------------------------------------------------------------------------
IF SOMETHING GOES WRONG
------------------------------------------------------------------------------

The engine will not accept your licence key
  Help > Engine says why - for example the key has expired, or it is already
  in use on all the computers your licence allows. Check the key in your
  purchase email, or contact support.

The engine stops after exactly 30 minutes
  It is running as the demo. On macOS and Windows, choose "Enter licence
  key" when PriceSylo asks, or activate your key in Help > Engine. On Linux,
  close the engine, delete .engine-state/license/run-mode.json in its folder
  (a hidden folder), start it again and choose 1.

The panel says the engine is offline
  macOS and Windows: Help > Engine shows what the engine did and its log;
  quitting and reopening PriceSylo starts it again. Linux: check that the
  engine's terminal is still open and running; if you closed it, start the
  engine again (Step 1).

The engine says the address or port 8420 is already in use
  Another program, or another copy of the engine, is using that port. Close
  the other copy first.

A VPN or firewall is running
  The panel talks to the engine at 127.0.0.1 port 8420, which is this
  computer talking to itself and never leaves it. The engine does need
  outbound internet to reach the exchanges and to check your licence.

Where to look
  Help > Engine in the panel shows the engine's status and its log. On macOS
  and Windows the logs are in the engine folder listed above, under logs/;
  on Linux the engine's terminal shows what it is doing, live. If the engine
  ever crashes, the reason is written to error.log in its folder.


------------------------------------------------------------------------------
REMOVING PRICESYLO
------------------------------------------------------------------------------

macOS      drag PriceSylo from Applications to the Bin. To also remove the
           engine's settings, saved data and licence record, delete
           ~/Library/Application Support/PriceSylo.
Windows    uninstall PriceSylo from Settings > Apps. To also remove the
           engine's data, delete %LOCALAPPDATA%\PriceSylo.
Linux      the engine installs nothing: close it and delete the folder you
           put it in. Remove the panel the normal way for your system.

If you are moving your licence to another computer, contact support.


------------------------------------------------------------------------------
CHECK YOUR DOWNLOAD (OPTIONAL)
------------------------------------------------------------------------------

macOS and Windows: each folder has a SHA256SUMS.txt listing its
installers. Linux: SHA256SUMS-pricesylo.txt lists the engine, config.json,
README.md and LICENSE.

In a terminal in that folder, on macOS or Linux, run:
  shasum -a 256 -c SHA256SUMS.txt              (macOS)
  shasum -a 256 -c SHA256SUMS-pricesylo.txt    (Linux)
Every line should say OK. If one does not, do not run the file.

On Windows, in PowerShell in that folder, run:
  Get-FileHash PriceSylo-App-<version>-win-x64-setup.exe -Algorithm SHA256
and compare the result with the line for it in SHA256SUMS.txt.


------------------------------------------------------------------------------
SUPPORT AND LICENCE
------------------------------------------------------------------------------

Website and documentation:  https://pricesylo.com
Support:                    support@pricesylo.com
Licence terms:              https://pricesylo.com, and the LICENSE file
                            beside the Linux engine

PriceSylo is published by Laxtic Software Services.
