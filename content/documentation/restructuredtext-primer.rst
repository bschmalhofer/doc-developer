reStructuredText Primer
=======================

The documentation format name is **reStructuredText** (one word, this is the correct spelling). This is an easy to read documentation format using plain text and small inline markers.

This short tutorial will guide you through to create or update documentations. To give a full featured tutorial about how to use the *reStructuredText* format is beyond the scope of this document, and many tutorials (e. g. `Sphinx reStructuredText primer <http://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`__ and `reStructuredText user documentation <http://docutils.sourceforge.net/rst.html>`__) and `on-line editors <http://rst.ninjs.org>`__ are available on the internet.

The following examples shows the most commonly used documentation elements.


Headings
--------

To use heading in the documentation, you have to underline the titles with special characters. The underline must start from the first letter of the title and end at the last letter of the title. The hierarchy of the special characters are the following: =, -, ~, ^, .

The following example shows the usage of the headings:

.. code-block:: rst

   Chapter title
   =============

   This is the heading 1 title. It has numbering like 1.

   Section title
   -------------

   This is the heading 2 title. It has numbering like 1.1.

   Subsection title
   ~~~~~~~~~~~~~~~~

   This is the heading 3 title. It has numbering like 1.1.1.

   Subsubsection title
   ^^^^^^^^^^^^^^^^^^^

   This is the heading 4 title. It has numbering like 1.1.1.1.

   Subsubsubsection title
   ......................

   This is the heading 5 title. It has numbering like 1.1.1.1.1. Please don't use this level of heading.


Paragraphs
----------

For writing paragraphs, you have to start sentences at the beginning of the line. To create a new paragraph, just leave a blank line between the paragraphs. Example:

.. code-block:: rst

   Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed dictum imperdiet enim. Curabitur
   nisi diam, lobortis facilisis quam ut, porttitor consequat lectus. Nam elementum, ipsum id
   feugiat vestibulum, dolor ante dictum quam, ac bibendum ipsum felis in orci.

   Vestibulum maximus egestas orci, eget consequat nibh imperdiet eget. Suspendisse sagittis tempus
   sapien, sit amet tincidunt tortor efficitur et. Etiam ac lacus sem. Sed ut magna imperdiet,
   viverra quam vitae, consequat mauris.


Inline Markups
--------------

The standard inline markup is quite simple:

- One asterisk: \*text\* for *emphasis* (italics).
- Two asterisks: \*\*text\*\* for **strong emphasis** (boldface).
- Grave accents: \`\`text\`\` for ``literal texts`` (code samples).

If asterisks or grave accents appear in running text which could be confused with inline markup delimiters, they have to be escaped with a backslash, like \\*.


Lists
-----

To create unordered lists, start a line with asterisk (\*) or dash (-). To create ordered list, start a line with numbers or hash mark (#). If you need nested lists, leave a blank line between the list items and use indentation with 3 spaces. Example:

.. code-block:: rst

   * This is a bulleted list.
   * It has two items, the second
     item uses two lines.

   1. This is a numbered list.
   2. It has two items too.

   #. This is a numbered list.
   #. It has two items too.

Nested list example:

.. code-block:: rst

   - this is
   - a list
   
     - with a nested list
     - and some subitems

   - and here the parent list continues


Literal Blocks
--------------

Literal blocks are texts that should be displayed as verbatim. To create literal blocks, do the following:

1. Type 2 colons (::) in a new line.
2. Leave a blank line.
3. Write the text with indentation of 3 spaces.

Use literal blocks for code snippets, terminal outputs, configuration files, etc. Example:

.. code-block:: none

   ::

      $Self->{DatabaseHost} = '127.0.0.1';
      $Self->{Database} = 'otrs';
      $Self->{DatabaseUser} = 'otrs';

If the language of the code snippet is known, you can specify it for syntax highlighting:

.. code-block:: perl

   .. code-block:: perl

      $Self->{DatabaseHost} = '127.0.0.1';
      $Self->{Database} = 'otrs';
      $Self->{DatabaseUser} = 'otrs';

.. code-block:: xml

   .. code-block:: xml

      <Setting Name="FAQ::Agent::StateTypes" Required="1" Valid="1">
          <Description Translatable="1">List of state types which can be used in the agent interface.</Description>
          <Navigation>Core::FAQ</Navigation>
          <Value>
              <Array>
                  <Item>internal</Item>
                  <Item>external</Item>
                  <Item>public</Item>
              </Array>
          </Value>
      </Setting>


Tables
------

To create grid tables, you have to draw the table. Example:

.. code-block:: rst

   +------------------------+------------+----------+----------+
   | Header row, column 1   | Header 2   | Header 3 | Header 4 |
   | (header rows optional) |            |          |          |
   +========================+============+==========+==========+
   | body row 1, column 1   | column 2   | column 3 | column 4 |
   +------------------------+------------+----------+----------+
   | body row 2             | ...        | ...      |          |
   +------------------------+------------+----------+----------+


Hyperlinks
----------

Hyperlinks can be used inline or referenced. For inline use, encapsulate the text of the link and the URL with grave accents and two trailing underscore characters.

.. code-block:: rst

   Visit `OTRS website <https://otrs.com>`__ for more information.

The link above will display as: `OTRS website <https://otrs.com>`__.

To create referenced links, you have to separate the text and the link. Example:

.. code-block:: rst

   The documentations are available in the `OTRS documentation portal`_.

   .. _OTRS documentation portal: https://doc.otrs.com/


Images
------

To insert an image into the documentation:

1. Put the image in the ``images`` folder.
2. Create a reference to the image with:

   .. code-block:: rst

      .. figure:: images/admin-general-catalog-management-class.png
         :alt: Admin General Catalog

         Admin General Catalog


Colored Boxes
-------------

These boxes have special meanings and will be highlighted as default.

Warning box:

.. code-block:: rst

   .. warning::

      This is a warning box.

.. warning::

   This is a warning box.

Note box:

.. code-block:: rst

   .. note::

      This is a note box.

.. note::

   This is a note box.

See also box:

.. code-block:: rst

   .. seealso::

      This is a see also box.

.. seealso::

   This is a see also box.
