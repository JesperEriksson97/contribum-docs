Syncing Subscribers
========================

**Description**

Whenever some data changes about a Contact in Contribum, we want to be able to update it in Salesforce aswell.
This is done through the ``BatchSyncSubscribers`` job. This is a batch job which goes through 4 major steps:

1. Execute method enqueues a future method.
2. A generate-output job against Contribum is ran.
3. Data is being fetched from the generate-output job.
4. Contacts in Saleforce are updated.


BatchSyncSubscribers
-------------------

``BatchSyncSubscribers`` is the batch responsible for continuously syncing subscribers. However the batch is merely a execute function.
The logic for the sync operation is located in the ``ContribumActions.syncSubscribedContacts``.

Generate Output
---------------

The ``ContribumActions.syncSubscribedContacts`` is called with a ID parameter called ``JobID``. This ``JobID`` refers to the generated-output job id and will most of the time be set to null.
In the batch, we call this function with ``JobID`` set to null. However, it is possible to call this function with an already existing ``JobID``. This can be useful if you want to rerun a earlier made sync manually.

If JobId is equal to null we will create a new generate-output job id by calling the ``/customer-register/C9999/generate-output`` endpoint with an empty body. This will return a new ``JobID``.

Fetch Generated Output Data
---------------------------




