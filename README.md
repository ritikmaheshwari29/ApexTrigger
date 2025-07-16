# ApexTrigger

1) **Understanding Trigger Events**

    Trigger.new ---> This is a list which gives us access to the new version of the record user is trying to insert
    Trigger.old----> This is a list which gives us access to the old version of the record user is trying to update
    Trigger.newMap-> This is a map which gives us access to the new version of the record in map format
        				map<Id, cobject__c>
    Trigger.oldMap-> This is a map which gives us access to the old version of the record in map format
        				map<Id, cobject__c>


3) **Before Insert Trigger Event & Trigger Context Variables**

````
trigger beforeAccountInsert on Account (before insert) {
    System.debug('🚀');
    
    for(Account a: Trigger.new){
        
        a.Name = a.Name+ 'Inc - Updated';
        System.debug('🚀' + a.Name);
    }
````
