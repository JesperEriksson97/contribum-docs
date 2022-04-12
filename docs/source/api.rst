Contribum API
=============

.. autosummary::
   :toctree: generated

Contribum Managed Package
-------------------------

The managed package handles integration between Contribum and Salesforce.
It allows Contacts in Salesforce to be updated with up-to-date data about following:

1. First Name
2. Last Name
3. Middle Name
4. Personal Number
5. Status
        a. N = Standard
        b. A = Deceased
        c. U = Emigrated
        d. G = Changed personal number / old personal number
        e. X = Unknown personal number
6. Customer Id / Unique Id
7. C/O - Address
8. Street Address
9. Postal Number
10. Address Type
         a. F = Population register address
         b. S = Special address
         c. U = Abroad address
         d. 0 = No address
11. City
12. Deceased Date (YYYYMMDD)
13. Proteceted Identity
         a. 1 = Protected Identity
         b. 0 = Standard
         c. empty string = Uknown Individual
14. Last Change Date (YYYYMMDD)
15. LKF - Code (County Code)


Contribum Managed Package Workflow
----------------------------------

The integration to Contribum is composed by four main stages

1. Initial Sync of Contacts
2. Creating / Removing Subscribers
3. Generating Data
4. Continousouly Updating Contacts

Initial Sync of Contacts
------------------------

Whenever a Contact is created in Salesfoce it's set up to be synced with Contribum.
This is done through the Salesforce field called ``wants_to_subscribe`` being set to ``true``.

Once a day Salesforce rounds up all the new Contacts that has been created during the day and Syncs them with Contribum.
This is done through the ``BatchSyncNeverSynced`` job in Salesforce. It calls the Contribum API endpoint for each created Contact: ``/pnrws/C9999/?personnr=PERSONAL_NUMBER`` where C9999 is refering to the Issue number and the personnr query parameter is populated with the Contacts personal number.
For more information about the BatchSyncNeverSynced job, please see: LINK_TO_BATCHSYNCNEVERSYNCED_PAGE.

.. note::
   Note that personal number wont always be populated on a Contact. This is required for the initial sync. If the field is not populated it will be populated within a month by monthly fetch jobs that populates the personal number through other fields such as Mobile, Street Address, Last Name, First Name, etc. Read more about this under the Fetch section.


Creating Subscribers
--------------------

To be able to continuously sync our Salesforce Contacts we need to register them against the Contribum API. This is done daily through the ``BatchAddSubscribers`` job in Salesforce. Whenever a Contact is synced, we refer to it as a ``Subscriber``.
It calls the Contribum API endpoint for each synced Contact that wants to be subscribers: ``/customer-register/C9999/update`` where C9999 is refering to the Issue number and the personnr query parameter is populated with the Contacts personal number.
The endpoint requires a body consisting of two attributes per Contact that we want to sync::


[
   {
      "personnr": "contact_personal_number_1",
      "kundnr": "salesforce_id_1",
   },
   {
      "personnr": "contact_personal_number_2",
      "kundnr": "salesforce_id_2",
   }
]

1. ``personnr``` refers to the Contacts Personal Number.
2. ``kundnr``` refers to Customer ID or Customer Unique Identifer and is set to the Contacts unique Salesforce ID.

The answer we get back from Contribum looks like::


{
  "new": 1,
  "ignored": 1,
  "updated": 2
}


Where

1. ``new`` refers to all new subscribers registered successfully.
2. ``updated`` refers to all added subscribers that already exists at Contribum.
3. ``ignored`` if duplicates in ``kundnnr`` is found.

For more information about the BatchAddSubscribers job, please see: LINK_TO_BatchAddSubscribers_PAGE.

Removing Subscribers
--------------------

Whenever we want to remove a Subscriber from the Contribum API register, we do that daily through the ``BatchRemoveSubscribers`` job in Salesforce.
It calls the Contribum API endpoint: ``/customer-register/C9999/delete`` for each Contact that has a active subscription set in Salesforce and wants to be removed from the Contribum API register where C9999 is refering to the Issue number and the personnr query parameter is populated with the Contacts personal number.
The endpoint demands a body containing a list of customer ids (``kundnr`` set when we create the subscriber). Example of a delete body::


["0037a00001dU8DIAA0", "0037a00001dU8DJAA0", "0037a00001dU8DKAA0", "0037a00001dU8DLAA0"]


The request body will look something like::


{
  "deleted": 2,
  "ignored": 1
}


Where

1. ``deleted`` refers to amount of successfully deleted subscribers
2. ``ignored`` if no match on the ID.

For more information about the BatchRemoveSubscribers job, please see: LINK_TO_BatchAddSubscribers_PAGE.
   
   

