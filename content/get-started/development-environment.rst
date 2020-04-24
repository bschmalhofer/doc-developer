Development Environment
=======================

To facilitate the writing of OTRS expansion modules, the creation of a development environment is necessary. The source code of OTRS and additional public modules can be found on `GitHub <http://otrs.github.io>`__.


Obtain the Source Code
----------------------

First of all a directory must be created in which the modules can be stored. Then switch to the new directory using the command line and clone the Git repository by using the following command:

.. code-block:: bash

   shell> git clone git@github.com:OTRS/otrs.git -b master

For a specific branch like OTRS 6:

.. code-block:: bash

   shell> git clone git@github.com:OTRS/otrs.git -b rel-6_0

Please configure the OTRS system according to the `installation instructions`_.

.. _`installation instructions`: https://doc.otrs.com/doc/manual/installation/8.0/en/index.html


Useful Tools
------------

Clone the `module-tools <https://github.com/OTRS/module-tools>`__ module too, for your development environment. It contains a number of useful tools:

.. code-block:: bash

   shell> git clone git@github.com:OTRS/module-tools.git

`OTRSCodePolicy <https://github.com/OTRS/otrscodepolicy>`__ is a code quality checker that enforces the use of common coding standards also for the OTRS development team. It is highly recommended to use it if you plan to make contributions. You can use it as a standalone test script or even register it as a git commit hook that runs every time that you create a commit. Please see `the module documentation <https://github.com/OTRS/otrscodepolicy/blob/master/doc/en/OTRSCodePolicy.xml>`__ for details.

.. code-block:: bash

   shell> git clone git@github.com:OTRS/otrscodepolicy.git


Linking Expansion Modules
-------------------------

A clear separation between OTRS and the modules is necessary for proper developing. Particularly when using a git clone, a clear separation is crucial. In order to facilitate the OTRS access the files, links must be created. This is done by a script in the directory module tools repository.

Example: linking the *Calendar* module:

.. code-block:: bash

   shell> ~/src/module-tools/link.pl ~/src/Calendar/ ~/src/otrs/

Whenever new files are added, they must be linked as described above.

As soon as the linking is completed, the system configuration must be rebuilt to register the module in OTRS. Additional SQL or Perl code from the module must also be executed.

Example:

.. code-block:: bash

   shell> ~/src/otrs/bin/otrs.Console.pl Maint::Config::Rebuild
   shell> ~/src/module-tools/DatabaseInstall.pl -m Calendar.sopm -a install
   shell> ~/src/module-tools/CodeInstall.pl -m Calendar.sopm -a install

To remove links from OTRS enter the following command:

.. code-block:: bash

   shell> ~/src/module-tools/remove_links.pl ~/src/otrs/
