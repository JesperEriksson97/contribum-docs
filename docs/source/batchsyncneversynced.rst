Initial Sync of Contacts
========================

.. autosummary::
   :toctree: generated


Initial Sync
------------

**Description**

Whenever a new Contact is created in Salesforce the package will sync this Contact against the Contribum package.
This is done daily at 20:30 (Time is adjustable). The sync will "dress" the Contact with latest data derived from the Swedish population registration register.
The data provided in Salesforce by Contribum is:

1. First Name
2. Last Name
3. Middle Name
4. Personal Number
5. Status
6. Customer Id / Unique Id
7. C/O - Address
8. Street Address
9. Postal Number
10. Address Type
11. City
12. Deceased Date 
13. Proteceted Identity
14. Last Change Date
15. LKF - Code (County Code)

This sync should be done early in the Salesforce - Contribum workflow.

The sync is done through the Salesforce Personal Number field which is set in the Custom Metadata Type (see :doc:`usage`).

BatchSyncNeverSynced
--------------------

``BatchSyncNeverSynced`` is the name of the Batch responsible for initiating the intial sync of every new Contact in Salesforce.

Batch SOQL query::

    SELECT Id, Personal_Number_Field, FirstName, LastName, MailingStreet, MailingCity, MailingPostalCode
    FROM Contact
    WHERE Last_Sync__c = null
    AND Do_Not_Sync__c = false
    AND Wants_To_Subscribe__c = true
    AND Personal_Number_Field != null

For each Contact in the query it will call the ``/pnrws/C999?personnr=personalNumber``.
The response will go through the ``ContribumTransformer`` class. It will update the values based on the Id gathered from the original batch query with values retrieved from Contribum.
Finally, it will create a ``Contribum_Log__c`` Object in Salesforce containing information about:

1. Number(Integer) of records that failed.
2. Number(Integer) of records processed by the batch.
3. Finishing DateTime of batch job.




