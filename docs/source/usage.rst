Usage
=====

.. _installation:

Installation 0.27.0
------------

To use Contribum Managed Package, first install it using this link:
.. _Installation Link (Production): https://login.salesforce.com/packaging/installPackage.apexp?p0=04t08000000l10nAAA
.. _Installation Link (Sandbox): https://test.salesforce.com/packaging/installPackage.apexp?p0=04t08000000l10nAAA

1. Go to the above link and enter the credentials for your organization.
2. Enter the package password (provided by package manager).
3. Choose the appropriate User access (most commonly All Users).
4. Check the ``I acknowledge that I’m installing a Non-Salesforce Application that is not authorized for distribution as part of Salesforce’s AppExchange Partner Program.`` checkbox.
5. Press Install.

Salesforce Setup
----------------

**Named Credential Setup**

1. Enter the ``Setup`` page of the organization where you want to install the Contribum package on.
2. From the Home page, search for ``Named Credential`` in the Quick Find searchbar.
3. Click the ``New Named Credential`` Image 1
4. Fill in the form as the image provides: image:: docs/Screenshot 2022-04-11 at 15.04.15.png

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']

