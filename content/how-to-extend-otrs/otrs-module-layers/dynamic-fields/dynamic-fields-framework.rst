Dynamic Fields Framework
========================

Before creating new dynamic fields is necessary to understand its framework and how OTRS screens interact with them, as well as their underlying API.

In OTRS 8 two dynamic fields API are present.

The new API is used in the ``External`` and ``Agent`` interface while the legacy is still heavily used in the ``Admin`` interface and core modules ``$OTRS_HOME/Kernel/System``.


Dynamic Fields Framework (Legacy API)
-------------------------------------

This is the old and well known dynamic fields API.

The following picture shows the architecture of the framework.

.. figure:: images/dfframework.png
   :alt: Dynamic Fields Framework

   Dynamic Fields Framework


Dynamic Field Back End Modules (Legacy API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dynamic Field Back End (Legacy API)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Normally called as ``BackendObject`` in the front end modules is the mediator between the front end modules and each specific dynamic field implementation or driver. It defines a generic middle API for all dynamic field drivers, and each driver has the responsibility to implement the middle API for the specific needs for the field.

The dynamic field back end is the master controller of all the drivers. Each function in this module is responsible to check the required parameters and call the same function in the specific driver according to the dynamic field configuration parameter received.

This module is also responsible to call specific functions on each object type delegate (like ``Ticket`` or ``Article``) e.g. to add a history entry or fire an event.

This module is located in ``$OTRS_HOME/Kernel/System/DynamicFieldLegacy/Backend.pm``.

.. _dynamic-fields-framework-backends-drivers:


Dynamic Field Drivers (Legacy API)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A dynamic field driver is the implementation of the dynamic field. Each driver must implement all the mandatory functions specified in the back end (there are some functions that depends on a behavior and it is
not needed to implement those if the dynamic field does not have that particular behavior).

A driver is responsible to know how to get its own value or values from a web request, or from a profile (like a search profile). It also needs to know the HTML code to render the field in edit or display screens, or how to interact with the stats module, among other functions.

These modules are located in ``$OTRS_HOME/Kernel/System/DynamicFieldLegacy/Driver/*.pm``.

It exists some base drivers like ``Base.pm``, ``BaseText.pm``, ``BaseSelect.pm`` and ``BaseDateTime.pm``, that implements common functions for certain drivers (e.g. driver ``TextArea.pm`` uses ``BaseText.pm`` that also uses ``Base.pm`` then ``TextArea`` only needs to implement the functions that are missing in ``Base.pm`` and ``BateText.pm`` or the ones that are special cases).

The following is the drivers inheritance tree:

- ``Base.pm``

   - ``BaseText.pm``

      - ``Text.pm``
      - ``TextArea.pm``

   - ``BaseSelect.pm``

      - ``Dropdown.pm``
      - ``Multiselect.pm``

   - ``BaseDateTime.pm``

      - ``DateTime.pm``
      - ``Date.pm``

   - ``Checkbox.pm``


Object Type Delegate (Legacy API)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

An object type delegate is responsible to perform specific functions on the object linked to the dynamic field. These functions are triggered by the back end object as they are needed.

These modules are located in ``$OTRS_HOME/Kernel/System/DynamicFieldLegacy/ObjectType/*.pm``.


Dynamic Fields Admin Modules (Legacy API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To manage the dynamic fields (add, edit and list) a series of modules has been already developed. There is one specific master module (``AdminDynamicField.pm``) that shows the list of defined dynamic fields, and from within other modules are called to create new dynamic fields or modify an existing ones.

Normally a dynamic field driver needs its own admin module (admin dialog) to define its properties. This dialog might differ from other drivers. But this is not mandatory, drivers can share admin dialogs, if they can provide needed information for all the drivers that are linked to them, no matter if they are from different type. What is mandatory is that each driver must be linked to an admin dialog (e.g. text and textarea drivers share ``AdminDynamicFieldText.pm`` admin dialog, and date and date/time drivers share ``AdminDynamicFieldDateTime.pm`` admin dialog).

Admin dialogs follow the normal OTRS admin module rules and architecture. But for standardization all configuration common parts to all dynamic fields should have the same look and feel among all admin dialogs.

These modules are located in ``$OTRS_HOME/Kernel/Modules/*.pm``.

.. note::

   Each admin dialog needs its corresponding HTML template file (``.tt``).


Dynamic Fields Core Modules (Legacy API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This modules reads and writes the dynamic fields information from and to the database tables.

``DynamicFieldLegacy.pm``
   This module is responsible to manage the dynamic field definitions. It provides the basic API for add, change, delete, list and get dynamic fields. This module is located in ``$OTRS_HOME/Kernel/System/DynamicFieldLegacy.pm``.

``DynamicFieldLegacy/Value.pm``
   This module is responsible to read and write dynamic field values into the form and into the database. This module is highly used by the drivers and is located in ``$OTRS_HOME/Kernel/System/DynamicFieldLegacy/Value.pm``.


Dynamic Fields Database Tables (Legacy API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are two tables in the database to store the dynamic field information:

``dynamic_field``
   Used by the core module ``DynamicFieldLegacy.pm``, it stores the dynamic field definitions.

``dynamic_field_value``
   Used by the core module ``DynamicFieldLegacy/Value.pm`` to save the dynamic field values for each dynamic field and each object type instance.


Dynamic Fields Configuration Files (Legacy API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The back end module needs a way to know which drivers exists and since the amount of drivers can be easily extended. The easiest way to manage them is to use the system configuration, where the information of dynamic field drivers and object type drivers can be stored and extended.

The master admin module also needs to know this information about the available dynamic field drivers to use the admin dialog linked with, to create or modify the dynamic fields.

Front end modules need to read the system configuration to know which dynamic fields are active for each screen and which ones are also mandatory. For example: ``Ticket::Frontend::AgentTicketPhone###DynamicField`` stores the active, mandatory and inactive dynamic fields for *New Phone Ticket* screen.


Dynamic Fields Framework (New API)
----------------------------------

This is the new dynamic field API, which is slightly different from the legacy API in terms of the field structure.

The following picture shows the architecture of the framework.

.. figure:: images/dfframework-new.png
   :alt: Dynamic Fields Framework

   Dynamic Fields Framework


Dynamic Field Back End Modules (New API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dynamic Field Drivers (New API)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Every dynamic field driver represents a dynamic field, containing data like the related name, configurations and stored values. Each driver should inherit from the base driver ``Driver/Base.pm`` and implement all the mandatory functions specified in it.

A driver provides necessary information about, how it must be rendered in the front end applications (displaying in forms or screens), or how to interacts with modules and APIs, like the statistics back end among other functions.

These modules are located in ``$OTRS_HOME/Kernel/System/DynamicField/Driver/*.pm``.

To provide often needed and common functions for certain driver types, there are global base drivers available (``Base/Text.pm``, ``Base/Select.pm`` and ``Base/DateTime.pm``) from which dynamic field driver modules may inherit. Those base drivers themselves inherits from the mandatory base module.

For example driver ``TextArea.pm`` uses ``Base/Text.pm`` that also uses ``Base.pm`` then ``TextArea`` only needs to implement the functions that are missing in ``Base.pm`` and ``Base/Text.pm`` or the ones that are special cases.

The following is the drivers inheritance tree:

.. code-block:: none

   Base.pm
   |
   |-- Base/Text.pm
       |-- Text.pm
       |-- TextArea.pm
       |
       Base/Select.pm
       |-- Dropdown.pm
       |-- Multiselect.pm
       |
       Base/DateTime.pm
       |-- DateTime.pm
       |-- Date.pm
       |
       Checkbox.pm
       |
       ContactWithData.pm


Roles (New API)
***************

The dynamic field roles provide a simple way to extend the driver functionality.

The ``HasValueType`` roles indicate, which kind of data the driver will store and in which database column has to be used.

The ``Behaviour`` roles extend the drivers with extra funcionality, for example ``SupportsACLs.pm`` indicates that the possible values of the drivers could be restricted by ACL permissions.

These modules are located in ``$OTRS_HOME/Kernel/System/DynamicField/Driver/Role/*.pm``.

Here is a list of the current roles:

.. code-block:: none

   HasValueType
   - Text.pm
   - Int.pm
   - DateTime.pm

   Behavior
   - IsFilterable.pm
   - IsSortable.pm
   - IsHTMLContent.pm
   - SupportsACLs.pm
   - SupportsExternalInterface.pm
   - SupportsLikeOperator.pm
   - SupportsMultipleRecordsPerValue.pm
   - SupportsNotificationEvents.pm
   - SupportsOutputInTicketInformation.pm
   - SupportsStatsSearchField.pm


Object Type Delegation (New API)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

An object type delegation is responsible to perform specific functions on the object linked to a certain dynamic field. These functions are triggered by the driver as they are needed.

These modules are located in ``$OTRS_HOME/Kernel/System/DynamicField/ObjectType/*.pm``.


Dynamic Fields Admin Modules (New API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The management of the dynamic fields is done in the administrator interface, which still uses the legacy API.


Dynamic Fields Core Modules (New API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These modules read and write the dynamic fields information from and to the database tables.

``DynamicField.pm``
   This module is responsible to manage the dynamic field definitions. It provides the basic API for add, change, delete, list and get dynamic fields. It is located in ``$OTRS_HOME/Kernel/System/DynamicField.pm``.

``DynamicField/Value.pm``
   This module is responsible to read and write dynamic field values into the database. It is frequently used by the driver modules and is located in ``$OTRS_HOME/Kernel/System/DynamicField/Value.pm``.


Dynamic Fields Database Tables (New API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are three tables in the database to store the dynamic field information:

``dynamic_field``
   Used by the core module ``DynamicField.pm``, it stores the dynamic field definitions.

``dynamic_field_obj_id_name``
   Used by the core module ``DynamicField.pm`` to save the relationship between objects (with ID and name) and an available object type.

``dynamic_field_value``
   Used by the core module ``DynamicField/Value.pm`` to save the dynamic field values for each dynamic field and each object type instance.


Dynamic Fields Configuration Files (New API)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The master admin, as well as the dynamic field management module needs to know which drivers exists.

The easiest way to manage them is to use the system configuration, where the information of dynamic field drivers, their paths and meta information and object type modules can be stored and extended.

Front end modules need to read the system configuration to know which dynamic fields are active for each screen and which ones are also mandatory.

For example ``Ticket::Frontend::AgentTicketPhone###DynamicField`` stores the active, mandatory and inactive dynamic fields for the *New Phone Ticket* screen.
