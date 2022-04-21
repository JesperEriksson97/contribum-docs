Adding Subscribers
========================

**Description**

After a Contact has been synced we need to add it as a subscriber to Contribum for it to continuously be synced on any changes.
This is done through a batch job and is located in the ``BatchAddSubscribers`` class.
The ``BatchAddSubscribers`` job is scheduled to run at 21:30 daily but the time is adjuastable.


BatchAddSubscribers
-------------------

``BatchAddSubscribers`` is the batch responsible for adding subscribers.

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




