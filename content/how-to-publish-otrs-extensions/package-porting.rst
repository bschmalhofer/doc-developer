Package Porting
===============

With every new minor or major version of OTRS, you need to port your packages and make sure they still work with the OTRS API.

This section lists changes that you need to examine when porting your package from OTRS 7 to 8.


Front End Messages
------------------

In an effort to improve consistency and enforce new UX guidelines, a new and common front end messages API has been introduced in OTRS 8. Developers should strive to use only this way of informing users of application changes. A front end component has been created for this feature, and it's included by default in every application.

The API has been re-used from a previous integration, albeit with some changes. By emitting an event on the global event bus it's still possible to trigger display of a message on the user screen. For example, in order to show a toast style message, you can just emit an event like so:

.. code-block:: js

    this.$bus.$emit('showToastMessage', {
        id: 'aSampleToastMessage'
        heading: 'This is a %s heading',
        headingPlaceholders: [ 'toast' ],
        text: 'This is a %s message text.',
        textPlaceholders: [ 'toast' ],
        variant: 'warning',
    });

If your use-case is to prevent user from interacting with the app until they make a choice or acknowledge a message, you can trigger display of a blocking modal message. The interface is similar, please note the different name of the event:

.. code-block:: js

    this.$bus.$emit('showModalMessage', {
        id: 'aSampleModalMessage'
        heading: 'This is a %s heading',
        headingPlaceholders: [ 'modal' ],
        text: 'This is a %s message text.',
        textPlaceholders: [ 'modal' ],
        buttonBehavior: 'yesNo',
    });

Both message styles provide numerous configuration options, so make sure to consult the `component documentation in the design system <https://doc.otrs.com/doc/api/otrs/8.0/frontend/dist/designsystem/#/documentation/components/common/common-messages>`_ for complete API description.


Renamed Message Events
~~~~~~~~~~~~~~~~~~~~~~

Front end message component improvements meant that some event names had to be renamed, it will be expected from you to port existing packages that use them to conform to the new format. Please find a table below containing all affected events.

+-------------------------+-------------------------+
| Old name                | New name                |
+=========================+=========================+
| ``showNotification``    | ``showToastMessage``    |
+-------------------------+-------------------------+
| ``clearNotification``   | ``hideToastMessage``    |
+-------------------------+-------------------------+


Styling Improvements
--------------------

In OTRS 7, new front end stack has been introduced. However, since only one major front end application was shipped (for the external interface), there was no need to make sure that components living in the shared namespace have support for different styling. With OTRS 8, this was bound to change, and now OTRS supports app-specific styling in components.


Application Specific Styling in Shared Components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Usage is quite simple: whenever you need to apply an app-specific style, wrap your SCSS code in the following mixin:

.. code-block:: none

   @include Mixin__StylesForApp('External') {
       .ElementName {
           &__SubElement {
               color: $Color__External__Primary;

               &:hover {
                   color: $Color__External__Highlight;
               }
           }
       }
   }

Usage of the mixin guarantees your block will only be loaded where appropriate (in this example in the external interface app). In the block you are free to use application specific global variables and functions, without any restrictions. Everything that is available within the app will be allowed.

.. note::

   Design system will load appropriate style depending on current choice. For example, there will be a drop-down menu shown above the component example, allowing the user to switch the styles. All you have to do is make sure your component lives in the shared namespace and implements the mixin above, as needed.

In order to provide this mechanism, it was required to refactor all the styles to conform to the BEM specification. This now applies to any variable names, mixins, functions, etc. By looking at their names, now it will be perfectly clear where they are coming from, and the chance for any possible collisions is decreased to the minimum.


Renamed SCSS Literals
~~~~~~~~~~~~~~~~~~~~~

Since some SCSS literals had to be renamed, it will be expected from you to port existing packages that use them to conform to the new format. Please find a table below containing all affected literals.

+-----------+--------------+------------------------------+---------------------------------------------+
| Namespace | Origin       | Old name                     | New name                                    |
+===========+==============+==============================+=============================================+
| Shared    | *_colors*    | ``$base``                    | ``$Color__Base``                            |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$base``                    | ``$Color__Base``                            |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$white``                   | ``$Color__White``                           |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$alert``                   | ``$Color__Alert``                           |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$warning``                 | ``$Color__Warning``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$success``                 | ``$Color__Success``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$shadow``                  | ``$Color__Shadow``                          |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black100``                | ``$Color__Black100``                        |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black90``                 | ``$Color__Black90``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black80``                 | ``$Color__Black80``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black70``                 | ``$Color__Black70``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black50``                 | ``$Color__Black50``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black30``                 | ``$Color__Black30``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black20``                 | ``$Color__Black20``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black10``                 | ``$Color__Black10``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_colors*    | ``$black4``                  | ``$Color__Black4``                          |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_functions* | ``calculateRem``             | ``Function__CalculateRem``                  |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``border-radius``            | ``Mixin__BorderRadius``                     |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``list-reset``               | ``Mixin__ListReset``                        |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``FontSize``                 | ``Mixin__FontSize``                         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``MarginBottom--Responsive`` | ``Mixin__MarginBottom--Responsive``         |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``shadow``                   | ``Mixin__Shadow``                           |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``placeholder``              | ``Mixin__Placeholder``                      |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``float-label-container``    | ``Mixin__FloatLabel__Container``            |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``float-label``              | ``Mixin__FloatLabel``                       |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``float-label-input``        | ``Mixin__FloatLabel__Input``                |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``float-label-scaled``       | ``Mixin__FloatLabel--Scaled``               |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``linearGradient``           | ``Mixin__LinearGradient``                   |
+-----------+--------------+------------------------------+---------------------------------------------+
| Shared    | *_mixins*    | ``linearGradientoverlay``    | ``Mixin__LinearGradient--Overlay``          |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_variables* | ``$container-max-width``     | ``$Variable__External__ContainerMaxWidth``  |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_variables* | ``$spacing-small``           | ``$Variable__External__Spacing--Small``     |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_variables* | ``$spacing-medium``          | ``$Variable__External__Spacing--Medium``    |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_variables* | ``$font-small``              | ``$Variable__External__FontSize--Small``    |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_variables* | ``$font-smaller``            | ``$Variable__External__FontSize--Smaller``  |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_variables* | ``$font-smallest``           | ``$Variable__External__FontSize--Smallest`` |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$primary``                 | ``$Color__External__Primary``               |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$primary-darker``          | ``$Color__External__Primary--Darker``       |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$primary-dark``            | ``$Color__External__Primary--Dark``         |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$primary-lighter``         | ``$Color__External__Primary--Lighter``      |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$primary-light``           | ``$Color__External__Primary--Light``        |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$highlight``               | ``$Color__External__Highlight``             |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$highlight-darker``        | ``$Color__External__Highlight--Darker``     |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$highlight-dark``          | ``$Color__External__Highlight--Dark``       |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$highlight-lighter``       | ``$Color__External__Highlight--Lighter``    |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$highlight-light``         | ``$Color__External__Highlight--Light``      |
+-----------+--------------+------------------------------+---------------------------------------------+
| External  | *_colors*    | ``$gray-light``              | ``$Color__External__Gray--Light``           |
+-----------+--------------+------------------------------+---------------------------------------------+


Encode API Changed
--------------------

The legacy method ``Convert2CharsetInternal()`` was dropped. Please replace any usages of this with ``Convert()`` and a ``To => 'utf-8'`` parameter like this:

.. code-block:: Perl

    $EncodeObject->Convert2CharsetInternal(
        Text  => $BodyStrg,
        From  => $Self->GetCharset(),
        Check => 1,
    );

Replace this by:

.. code-block:: Perl

    $EncodeObject->Convert(
        Text  => $BodyStrg,
        From  => $Self->GetCharset(),
        To    => 'utf-8',
        Check => 1,
    );