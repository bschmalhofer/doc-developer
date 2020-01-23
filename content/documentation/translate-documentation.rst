Translating the Documentation
=============================

All translations of the OTRS GUI, the public extension modules and the documentations are managed using `Weblate <https://translate.otrs.com/>`__.

From OTRS 7 the documentations are available in *reStructuredText* format, that replaced the old *DocBook* format. Be careful not to break the structure while translating the documentation.

Here are some examples.

Emphases
   Emphasized texts are between two asterisks. The text should be translated. This is usually used for screen names, titles, buttons and labels. Please check the user interface translations to find and use the same wording in the documentation.

   Example original sentence:

   .. code-block:: none

      Use the *Ticket Zoom* screen to see the ticket details.

   Example translation into Hungarian:

   .. code-block:: none

      Használja a *Jegynagyítás* képernyőt a jegy részleteinek megtekintéséhez.

Strong
   Strong texts are between two double asterisks. The text should be translated. This is usually used for important information.

   Example original sentence:

   .. code-block:: none

      **Don't continue** the update if you get an error message.

   Example translation into Hungarian:

   .. code-block:: none

      **Ne folytassa** a frissítést, ha hibaüzenetet kap.

Literal texts
   Literal texts are between two double back-tick characters. This is usually used for variable names, configuration names and file paths, and **must not** be translated, otherwise it will break the structure.

   Example original sentence:

   .. code-block:: none

      Activate ``group`` in system configuration ``ExamplePermission###100``.

   Example translation into Hungarian:

   .. code-block:: none

      Aktiválja a ``group`` értéket az ``ExamplePermission###100`` rendszerbeállításban.

Internal links
   Internal links point to other pages or headings of the pages. ``:doc:`page-name``` is used for referring to a page and ``:ref:`Heading Title``` is used for referring to a heading. There is a custom tag ``:sysconfig:`System Configuration Name``` for referring to a system configuration. The texts *page-name*, *Heading Title* and *System Configuration Name* **must not** be translated, otherwise it will break the structure.

   Example original sentence:

   .. code-block:: none

      See page :doc:`queues` to add a queue, especially section :ref:`Queue Settings`.

   Example translation into Hungarian:

   .. code-block:: none

      Nézze meg a :doc:`queues` oldalt, különösen a :ref:`Queue Settings` szakaszt.

External links
   External links consist of a visible text and an URL in form *\`visible text <https://example.com>\`__*. The *visible text* should be translated.

   Example original sentence:

   .. code-block:: none

      See `OTRS website <https://otrs.com/>`__ for more information.

   Example translation into Hungarian:

   .. code-block:: none

      Nézze meg az `OTRS weboldalát <https://otrs.com/>`__ a további információkért.
