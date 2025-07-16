# ApexTrigger

1) **Understanding Trigger Events**

    Trigger.new ---> This is a list which gives us access to the new version of the record user is trying to insert
   
    Trigger.old----> This is a list which gives us access to the old version of the record user is trying to update
   
    Trigger.newMap-> This is a map which gives us access to the new version of the record in map format
        				map<Id, cobject__c>
   
    Trigger.oldMap-> This is a map which gives us access to the old version of the record in map format
        				map<Id, cobject__c>



2) **Before Insert Trigger Event & Trigger Context Variables**

````
trigger beforeAccountInsert on Account (before insert) {
    System.debug('🚀');
    
    for(Account a: Trigger.new){
        
        a.Name = a.Name+ 'Inc - Updated';
        System.debug('🚀' + a.Name);
    }
````


3) **Trigger with After Insert Trigger Event**

```
   trigger afterAccountInsert on Account (after insert) {
    
    System.debug('🚀');
    
    for(Account a: Trigger.new){
        Contact c = new Contact();
        c.AccountId = a.Id;
        c.LastName = 'Upadhyay';
        
        insert c;
    }

}
```


4) **OldMap and NewMap Context Variables**

   ```
   trigger exploreTriggerMaps on Account (before update) {
    
             System.debug(' 🚀 ' +Trigger.oldMap);
    
             System.debug(' 🚀 ' +Trigger.newMap);
       }```
