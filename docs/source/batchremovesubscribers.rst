Removing Subscribers
========================

**Description**

If, for any reason, a Contact wants to data to be removed from Salesforce we need to remove that Contact from the subscribers register at Contribum.
This is necessary to ensure that the contact is not included in the ``BatchSyncSubscribers`` job (see :doc:`batchsyncsubscribers`).
This is done through a batch job and is located in the ``BatchRemoveSubscribers`` class.
The ``BatchRemoveSubscribers`` job is scheduled to run at 21:00 daily but the time is adjuastable.




BatchAddSubscribers
-------------------

``BatchRemoveSubscribers`` is the batch responsible for removing subscribers.

Batch SOQL query::
    
    SELECT Id, Personal_Number_Field
    FROM Contact
    WHERE Wants_To_Subscribe__c = true
    AND Do_Not_Sync__c = false
    AND Subscription_Is_Active__c = false
    AND axenoncontribum__Last_Sync__c != null
    AND Personal_Number_Field != null

All Contacts found by the query is added to a List and sent to Contribum bulkified.
If the Contribum response is not empty it goes through each Contact processed by the batch following actions are made:

1. ``Subscription_Started__c`` is set to ``System.now()``
2. ``Subscription_Ended__c`` is set to ``null``




