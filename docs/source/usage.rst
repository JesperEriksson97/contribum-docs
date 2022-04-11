Usage
=====

.. _Setup:

Installation 0.27.0
------------

To use Contribum Managed Package, first install it using this link:

Installation Link (Production): https://login.salesforce.com/packaging/installPackage.apexp?p0=04t08000000l10nAAA

Installation Link (Sandbox): https://test.salesforce.com/packaging/installPackage.apexp?p0=04t08000000l10nAAA


1. Go to the above link and enter the credentials for your organization.
2. Enter the package password (provided by package manager).
3. Choose the appropriate User access (most commonly All Users).
4. Check the ``I acknowledge that I’m installing a Non-Salesforce Application that is not authorized for distribution as part of Salesforce’s AppExchange Partner Program.`` checkbox.
5. Press Install.

Salesforce Setup
----------------
.. role:: raw-html(raw)
    :format: html
**Named Credential Setup**

1. Enter the ``Setup`` page of the organization where you want to install the Contribum package on.
2. From the Home page, search for ``Named Credential`` in the Quick Find searchbar.
3. Click the ``New Named Credential``
4. Fill in the form as follows:
:raw-html:`<br />`
``Label``: Contribum
:raw-html:`<br />`
``Name``: Contribum
:raw-html:`<br />`
``URL``: https://www.omnispekt.com
:raw-html:`<br />`
``Certificate``: Leave blank
:raw-html:`<br />`
``Identity Type``: Named Principal
:raw-html:`<br />`
``Authentication Protocol``: Password Authentication
:raw-html:`<br />`
``Username``: axenon
:raw-html:`<br />`
``Password``: Password provided by Package Manager
:raw-html:`<br />`
``Generate Authorization Header``: Check
:raw-html:`<br />`
``Allow Merge Fields in HTTP Header``: Check
:raw-html:`<br />`
``Allow Merge Fields in HTTP Body``: Check
:raw-html:`<br />`
``Outbound Network Connection``: Leave Blank
:raw-html:`<br />`
5. Press ``Save``

.. note::
   These are just the standard set up. If other requirements are needed by the organization feel free to change settings however you like.

**Custom Metadata Setup**

1. Enter the ``Setup`` page of the organization where you want to install the Contribum package on.
2. From the Home page, search for ``Custom Metadata Types`` in the Quick Find searchbar.
3. Click the pre-installed custom metadata type called ``Contribum``. If not, contact the Package Manager at Axenon.
4. Click on ``Manage Contribum``.
5. Click on ``Setup``.
6. Click on ``Edit``.
7. Fill in the organization data. The Issue numbers should be provided by Contribum. The rest of the fields are up to you.



