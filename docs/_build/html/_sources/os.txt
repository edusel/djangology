.. role:: console(code)
    :language: console

********************************
Setting up your Operating System
********************************

.. NOTE::

    The instructions below cover setup for OSX El Capitan 10.11 and above.

Take Me to the Terminal
=======================

There are many amazing and expensive Interactive Development Environments (IDE's)
out there for Python. IDE's are great for professionals because they auto-magically
take care of a lot of things. Since this tutorial is about the basics, we're going
to be working in Terminal most of the time.

So without further ado, launch Terminal:

1.      Launch Spotlight Search by pressing :kbd:`cmd+space`.
2.      Type 'Terminal' and press :kbd:`enter`.

Woo hoo! Welcome to your ``$HOME`` directory. You should see something like this:

.. code-block:: console

    <YOUR_COMPUTER>:~ <USER_ID>$

Install X-Code Command Line Tools
=================================

X-Code is the IDE that is used to develop applications for developing OSX and IOS
applications. Do we care about that? Not for the scope of this tutorial. However,
we do need the compiler tool-chain to help us install other development related
software. Thankfully, we can install the stand alone X-Code Command Line tool:

.. code-block:: console

    $ xcode-select --install

Package Managers
================

Most of the tools we will use in our development environment will not be found
in the Apple Store or as an executable download. These tools are usually maintained
at the binary level, so that developers can easily install the latest update or
work collaborately to add features.

Homebrew
--------

`Homebrew <http://brew.sh>`_ will be used to install OSX binary packages for the most
common elements in our development keychain (Python, PostgreSQL, Git, etc.).

Install Homebrew from the most recent master version on GitHub using the following
Ruby script:

.. code-block:: console

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

.. NOTE::

    Install the following packages via :console:`$ brew install <package>`
        * atom
        * git
        * postgresql
        * python
        * python3
        * trash

    Helpful Brew commands:
        * help
        * install
        * list
        * search

pip
---

pip is the python package installer. It comes in both the python and python3 brew packages. Later
in this tutorial, we'll edit the ``.bashrc`` script to control pip's behavior in the global
environment.

.. NOTE::

    Install the following packages via :console:`$ pip install <package>`
        * flake8
        * flake8-docstrings
        * pep8
        * virtualenv
        * virtualenvwrapper

    Helpful pip commands:
        * help
        * install
        * list
        * search

Create a .bash_profile
======================

The bash_profile will allow us to override terminals default settings when we
launch a new terminal window. We'll use this to streamline some common tasks and
(hopefully) help us avoid some mistakes.

Use the console command ``touch`` to create a bash profile in your ``$HOME`` directory:

.. code-block:: console

    $ touch ~/.bash_profile

Open the new ``.bash_profile`` in edit mode using the ``open -e`` command:

.. code-block:: console

    $ open -e ~/.bash_profile

The code below will establish two things happen every time a new terminal window is
initiated [1]_:

1.      An architecture flag will be set to establish all compilation can assume 64 bit
        (OSX is 64-bit)
2.      Check to see if a ``.bashrc`` script exists, and if so, ``source`` it.

.. code-block:: bash

    # Set architecture flags
    export ARCHFLAGS="-arch x86_64"

    # Load .bashrc if it exists
    test -f ~/.bashrc && source ~/.bashrc

.. NOTE::
    In order to separate our development applications from OSX applications, I
    recommend adding ``/usr/local/bin`` to the ``$PATH`` in your ``.bash_profile`` as
    well.  You can ensure that your path is set correctly using the ``echo`` command
    from the terminal prompt:

    .. code-block:: console

        $ echo $PATH
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

    If your ``echo`` return doesn't start with ``/usr/local/bin:``, add the following
    lines to your ``.bash_profile``:

    .. code-block:: bash

        # Ensure user defined binaries take precedence
        # export PATH=/usr/local/bin:$PATH

Save the file (:kbd:`cmd+s`) and quit TextEdit (:kbd:`cmd+q`)

We need to reinitiate the terminal window with our fancy new ``.bash_profile``.
The console command ``source`` takes care of that.

.. code-block:: console

    $ source ~/.bash_profile

Did we actually change anything? Check by using the ``echo`` command to ask the
console to return the value of ``ARCHFLAGS``.

.. code-block:: console

    $ echo $ARCHFLAGS
    -arch x86_64

Create a .bashrc script
=======================

The bash_rc script is used to set global and environmental variables that we want
every time a terminal window is launched. We'll use it to safe guard against some
basic problems.

Set pip to only execute in a virtual environment
================================================
Generally speaking, packages should be installed on an as-needed basis for each
project. This will prevent unintended dependencies not contained in your virtual
environment.

Use the console command ``touch`` to create a bashrc script in your ``$HOME`` directory:

.. code-block:: console

    $ touch ~/.bashrc

Open the new ``.bashrc`` in edit mode using the ``open -e`` command:

.. code-block:: console

    $ open -e ~/.bashrc

Create an environment variable to ensure a virtual environment is active before pip can run.

.. code-block:: bash

    # pip should only run if there is a virtualenv currently active
    export PIP_REQUIRE_VIRTUALENV=TRUE

Create two global variables for when global pip packages need to be installed.

.. code-block:: bash

    # use gpip to temporarily turn off pip restrictions
    gpip(){
       PIP_REQUIRE_VIRTUALENV="" pip "$@"
    }
    gpip3(){
       PIP_REQUIRE_VIRTUALENV="" pip3 "$@"
    }

.. [1] `<https://hackercodex.com/guide/mac-osx-mavericks-10.9-configuration/>`_
