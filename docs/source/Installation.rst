************
Installation
************

This is meant to be a rudimentary guide for getting you started with LIMA. Instead of building it directly on Windows, 
we recommend using Windows Subsystem for Linux or a virtual machine if you do not have a Linux Server available. These come
cheap from most Linux hosting companies.

Requirements: 1 Gb of memory and 1 Gb of disk should be plenty.

See https://www.fluffos.info/build.html for more options for building FluffOS.

Windows Subsystem for Linux (WSL)
---------------------------------

1. Open a Powershell or command prompt on Windows and do ``wsl --list --online`` to see which distributions are available for your Windows system.

![WSL Distributions](images/wsl_step1.png)

2. Install Windows Subsystem for Linux 

    ``wsl --install ubuntu``

3. Update apt, and install packages needed for compiling FluffOS

    |   ``sudo apt-update`` 
    |   ``sudo apt install build-essential bison libmysqlclient-dev libpcre3-dev libpq-dev libsqlite3-dev libssl-dev libz-dev libjemalloc-dev libicu-dev``

4. Clone LIMA sources from github. It's available from two sources:

    |   Stable: https://github.com/fluffos/lima
    |   Development: https://github.com/tsathoqqua/lima
    |
    |   ``git clone https://github.com/tsathoqqua/lima.git --recurse-submodules``

5. Use --recurse-submodules to checkout the fluffos driver submodule. You can decide not to and use another driver if you want. LIMA comes with a build script after checking out the files:

    |    ``cd lima``   
    |    ``./build.sh``

6. If you get complaints about missing dependencies here, try to install them via ``apt install`` or use ``apt search`` to find them.
    |    ``sudo apt install libssl3``
    |    (Just an example)

7. After build has completed, try:

    ``./run.sh``

8. You might see a few warnings, but should be able to visit http://localhost:7878/ in your favourite browser via the built-in websockets. This can be reconfigured to use more classical ports in ``config.lima`` in the root directory of LIMA.

Ubuntu
------

Same as above, except you can skip step 1.

