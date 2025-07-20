# ApexTrigger

# Trigger Context Variables : https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_context_variables.htm

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


5) **Other Trigger Context Variables**

```
trigger triggerContext on Account (before insert, after update) {
    
    if(Trigger.isInsert){
        for(Account a: Trigger.new){
            
            System.debug('Before Insert 🚀 ' +a);
        }
        
    }
    
    
    if(Trigger.isUpdate && Trigger.isAfter){
        for(Account a: Trigger.new){
            
            System.debug('After Insert Trigger 🚀 ' +a);
        }
        
    }

}

```

6) **Making Web Service Callouts From Triggers**

trigger makeAWSCalloutFromTriggers on Account (before insert) {
    for(Account a: Trigger.new){
        
        //we can pass in parameters
        WSController.pushRecordToAnotherPlatform();
        
    }

}

------------------------------------------
public class WSController {
    
    @future(callout=true)
    public static void pushRecordToAnotherPlatform(){
        //code to make a callout
        
    }

}
   
---------------------------------------------------------------------------------------------

WHy do we have to write Test Classes?


---------------------------------

Creating a basic Apex Test Class
```
@isTest
public class AccountController_Test {
    
    testMethod static void saveTest(){
        //method 1
    }
    
    //method 2
    
    @isTest
    private static void saveTestMethod(){
        
    }
    

}
```

```
public class AccountController {
    public void save(String accountName, String accountNumber, String rating){
        
        Account a = new Account();
		a.Name = accountName;
        a.AccountNumber = accountNumber;
        a.Rating = rating;
        
        insert a;
        System.debug('🚀 '+ a);
        
        if(a.Rating !=null){
            contact c = new Contact();
            c.LastName = 'LastName';
            
            insert c;
        } else{
            Case caseRecord = new Case();
            caseRecord.Description = 'Test';
            insert caseRecord;
        }
    }

}
`````
**CLICK--> RunTest**

`````
@isTest
public class AccountController_Test {
    
    testMethod static void saveTest(){
        //method 1
        AccountController accCon = new AccountController();
        accCon.save('Testing Account', 'ABCD1234', 'Warm');
        
         accCon.save('New Testing Account', 'ABCD12345', null);
    }
    

}`````
