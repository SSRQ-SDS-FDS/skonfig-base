cdist-type__pyvenv(7)
=====================

NAME
----
cdist-type__pyvenv - Create or remove python virtual environment


DESCRIPTION
-----------
This cdist type allows you to create or remove python virtual
environment using pyvenv on python3 -m venv.
It assumes pyvenv is already installed. Concrete package depends
on concrete OS and/or OS version/distribution.
Ensure this for e.g. in your init manifest as in the following example:

.. code-block:: sh

   case ${__target_host:?}
   in
      (localhost)
         __package python3-venv --state present
         require=__package/python3-venv \
         __pyvenv /home/darko/testenv \
            --pyvenv pyvenv-3.4 \
            --owner darko --group darko --mode 740 \
            --state present
         require=__pyvenv/home/darko/testenv \
         __package_pip docopt \
            --pip /home/darko/testenv/bin/pip \
            --runas darko \
            --state present
         ;;
   esac


OPTIONAL PARAMETERS
-------------------
group
   Group to :strong:`chgrp`\ (1) to.
interpreter
   Name or path of the interpreter to use for creating the venv.

   Defaults to: ``python3``
mode
   Unix permissions, suitable for chmod
owner
   User to :strong:`chown`\ (1) to
state
   One of:

   present
      the pyvenv exists
   absent
      the pyvenv does not exists

   Defaults to: ``present``
venvparams
   Specific parameters to pass to pyvenv invocation


EXAMPLES
--------

.. code-block:: sh

   __pyvenv /home/services/djangoenv

   # Use specific interpreter name
   __pyvenv /home/foo/fooenv --interpreter python3.10

   # Use specific interpreter path
   __pyvenv /home/foo/fooenv --interpreter /opt/python/3.4/bin/python3

   # Create python virtualenv for user foo.
   __pyvenv /home/foo/fooenv --group foo --owner foo

   # Create python virtualenv with specific parameters.
   __pyvenv /home/services/djangoenv \
      --venvparams '--copies --system-site-packages'


AUTHORS
-------
* Darko Poljak <darko.poljak--@--gmail.com>


COPYING
-------
Copyright \(C) 2016 Darko Poljak.
You can redistribute it and/or modify it under the terms of the GNU General
Public License as published by the Free Software Foundation, either version 3 of
the License, or (at your option) any later version.
