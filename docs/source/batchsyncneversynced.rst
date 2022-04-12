Initial Sync of Contacts
========================

.. _Setup:

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

The sync is done via Personal Number which is set in the Custom Metadata Type (see :doc:`usage`).




