.. include:: /Includes.rst.txt
.. index::
   Top-level objects; plugin
   plugin
.. _plugin:

======
plugin
======

This is used for extensions in TYPO3 set up as frontend plugins.
Typically you can set configuration properties of the plugin here. Say
you have an extension with the key "myext" and it has a frontend
plugin named "tx\_myext\_pi1" then you would find the TypoScript
configuration at the position :typoscript:`plugin.tx_myext_pi1` in the object
tree!

Most plugins are :ref:`cobj-user` objects
which means that they have at least 1 or 2 reserved properties.
Furthermore this table outlines some other default properties.
Generally system properties are prefixed with an underscore:

Properties
----------

.. container:: ts-properties

   =============================================== ======================================== ====================== =======
   Property                                        Data Type                                :ref:`stdwrap`         Default
   =============================================== ======================================== ====================== =======
   `userFunc`_                                     (array of keys)
   `setup-plugin-css-default-style`_               :ref:`data-type-string`                  :ref:`stdwrap`
   `setup-plugin-css-page-style`_                  :ref:`data-type-string`
   `setup-plugin-default-pi-vars-pivar-key`_       :ref:`data-type-string`                  :ref:`stdwrap`
   `setup-plugin-local-lang-lang-key-label-key`_   :ref:`data-type-string`
   =============================================== ======================================== ====================== =======

.. ### BEGIN~OF~TABLE ###

.. _setup-plugin-userfunc:

userFunc
--------

.. container:: table-row

   Property
         userFunc

   Data type
         (array of keys)

   Description
         Property setting up the :ref:`cobj-user` object of the plugin.



.. _setup-plugin-css-default-style:

\_CSS\_DEFAULT\_STYLE
---------------------

.. container:: table-row

   Property
         \_CSS\_DEFAULT\_STYLE

   Data type
         :ref:`data-type-string` / :ref:`stdwrap`

   Description
         Use this to have some default CSS styles inserted in the header
         section of the document. :typoscript:`_CSS_DEFAULT_STYLE` outputs a set of
         default styles, just because an extension is installed. Most likely
         this will provide an acceptable default display from the plugin, but
         should ideally be cleared and moved to an external stylesheet.

         This value is read by the frontend :php:`RequestHandler` script when
         collecting the CSS of the document to be rendered.

         This is e.g. used by *frontend* and *indexed_search*. Their
         default styles can be removed with:

         .. code-block:: typoscript
            :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

            plugin.tx_frontend._CSS_DEFAULT_STYLE >
            plugin.tx_indexedsearch._CSS_DEFAULT_STYLE >

         However, you will then have to define according styles yourself.



.. _setup-plugin-css-page-style:

\_CSS\_PAGE\_STYLE
------------------

.. container:: table-row

   Property
         \_CSS\_PAGE\_STYLE

   Data type
         :ref:`data-type-string`

   Description
         `_CSS_PAGE_STYLE` is included only on the affected pages. Depending
         on your configuration it will be written in an external file and included
         on the page or directly added as inline CSS block. Compression
         for page specific CSS also depends on the global :typoscript:`config.compressCss`
         setting.

         This setting can be used to only include the CSS when the plugin of a
         certain extension is included on that page.

         This value is read by the frontend :php:`RequestHandler` when
         collecting the CSS of the document to be rendered.



.. _setup-plugin-default-pi-vars-pivar-key:

\_DEFAULT\_PI\_VARS.[piVar-key]
-------------------------------

.. container:: table-row

   Property
         \_DEFAULT\_PI\_VARS.[piVar-key]

   Data type
         :ref:`data-type-string` / :ref:`stdwrap`

   Description
         Allows you to set default values of the piVars array which most
         plugins are using (and should use) for data exchange with themselves.

         This works only if the plugin calls :php:`$this->pi_setPiVarDefaults()`.

         The values have :ref:`stdWrap`, which also works recursively for multilevel
         keys.

   Example
         .. code-block:: typoscript
            :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

            plugin.tt_news._DEFAULT_PI_VARS {
                year.stdWrap.data = date:Y
            }

         This sets the key "year" to the current year.



.. _setup-plugin-local-lang-lang-key-label-key:

\_LOCAL\_LANG.[lang-key].[label-key]
------------------------------------

.. container:: table-row

   Property
         \_LOCAL\_LANG.[lang-key].[label-key]

   Data type
         :ref:`data-type-string`

   Description
         Can be used to override the default language labels for the plugin. The 'lang-key' setup part is 'default' for the default language of the website or the 2-letter (ISO 639-1) code for the language. 'label-key' is the 'trans-unit id' xml value in the XLF language file which resides in the path :file:`Resources/Private/Language` of the extension or in the :file:`typo3conf/l10n/[lang-key]` (:file:`var/labels/[lang-key]` in composer mode) subfolder of the TYPO3 root folder. And on the right side of the equation sign '=' you put the new value string for the language key which you want to override.

   Example

         .. code-block:: typoscript
            :caption: EXT:site_package/Configuration/TypoScript/setup.typoscript

            plugin.tx_myext_pi1._LOCAL_LANG.de.list_mode_1 = Der erste Modus

         All variables, which are used inside an extension with
         :php:`$this->pi_getLL('list_mode_1', 'Text, if no entry in locallang.xlf', true);`
         can that way be overwritten with TypoScript. The :file:`locallang.xlf` file in
         the plugin folder in the file system can be used to get an overview of
         the entries the extension uses.

.. ###### END~OF~TABLE ######
