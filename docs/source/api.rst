Contribum API
=============

.. autosummary::
   :toctree: generated

Contribum Managed Package
-------------------------

The managed package handles integration between Contribum and Salesforce.
It allows Contacts in Salesforce to be updated with up-to-date data about

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
-------------------------

The integration to Contribum is composed by four main stages

1. Initial Sync of Contacts
2. Creating / Removing Subscribers
3. Generating Data
4. Continousouly Updating Contacts

Initial Sync of Contacts
-------------------------

Whenever a Contact is created in Salesfoce it's set up to be synced with Contribum.
This is done through the Salesforce field called ``wants_to_subscribe`` being set to ``true```.

Once a day Salesforce rounds up all the new Contacts that has been created during the day and Syncs them with Contribum.
This is done through the ``BatchSyncNeverSynced``` job in Salesforce. It calls the Contribum API endpoint for each created Contact: ``/pnrws/C9999/?personnr=PERSONAL_NUMBER`` where C9999 is refering to the Issue number and the personnr query parameter is populated with the Contacts personal number.

.. note::
   Note that personal number wont always be populated on a Contact. This is required for the initial sync. If the field is not populated it will be populated within a month by monthly fetch jobs that populates the personal number through other fields such as Mobile, Street Address, Last Name, First Name, etc. Read more about this under the Fetch section.

The endpoint requires a body consisting of two attributes per Contact that we want to sync

.. code-block:: json
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

