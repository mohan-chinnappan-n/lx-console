### Notes on Salesforce Ligtning Console JavaScript API

#### Lightning Console App User Interface

![Lightning Console App User Interface](https://developer.salesforce.com/docs/resources/img/en-us/208.0?doc_id=help%2Fimages%2Fconsole_lex.png&folder=api_console)


To select objects, use the item menu in the navigation bar (1). Records selected from the table list view or split view open as workspace tabs (2). When you click related records from the workspace tab, those records open as subtabs (3). To keep you efficient and productive, split view lets you work with a list view while still working on other records (4). You can close and open split view whenever you want—click the arrow on the split view pane (5). You can also click anywhere in the vertical divider between split view and record page. You can view and update a record using the details area (6) and the feed (7). Keep in mind that page layouts can be different for each type of record. Finally, utilities let you access common processes and tools, like History and Notes (8).

In this example, there are two workspace tabs open—Acme and Global Media. Under the Global Media workspace tab, there are three subtabs open—the contact record for Jon Amos, a related case, and a related opportunity. In the History utility, you can easily access your recently opened records.


#### APIs

1. force:workspaceAPIAccess,  this component gives access to Workspace API
2. force:utilityBarAPIAccess, this component gives access to utility bar API

##### How to get access these APIs:

Component using both the libs 

```xml
<aura:component implements="flexipage:availableForAllPageTypes" access="global"> 

<!-- APIs -->
<force:workspaceAPIAccess aura:id="workspace" /> 
<force:utilityBarAPIAccess aura:id="utilitybar" />

<lightning:button label="Open Utility" onclick="{! c.openUtilityBar }"/> 
<lightning:button label="Open Tab" onclick="{! c.openTab }" />

 </aura:component>
```

Controller:

```javascript
({
    openUtilityBar : function(component, event, helper) {
        // get the utilityAPI component
        var utilityAPI = component.find("utilitybar");
        utilityAPI.openUtility();
    },
    
    openTab : function(component) {
        // get the workspaceAPI component
        var workspaceAPI = component.find("workspace"); 
        workspaceAPI.openTab({
            url: '#/sObject/001R0000003HgssIAC/view', 
            focus: true,
            });
    }, 
})
```


#### Utility Bar

![Utility Bar](https://developer.salesforce.com/docs/resources/img/en-us/208.0?doc_id=dev_guides%2Fapi_console%2Fimages%2Fapi_console_utility_bar.png&folder=api_console)


1. The utility bar
2. The Chatter Feed utility
3. A utility label
4. A utility icon
5. The panel header
6. The panel header label
7. The panel header icon

-  single-column Lightning page
-  has ready-to-use utilities, such as Recent Items, History, and Notes
-  utility bar API (force:utilityBarAPIAccess) has 10 methods 


##### Example: API : openUtility()
```xml
<aura:component implements="flexipage:availableForAllPageTypes" access="global" >
    <force:utilityBarAPIAccess aura:id="utilitybar" />
    <lightning:button label="Open Utility" onclick="{! c.openUtility }"/>
</aura:component>
```

```javascript
({
    openUtility : function(component, event, helper) {
        var utilityAPI = component.find("utilitybar");
        utilityAPI.openUtility();
    }
})
```

#### Questions:
What is **flexipage:availableForAllPageTypes**?

If your component implements **flexipage:availableForAllPageTypes**  it can be accessed in the Lightning **App Builder**.




#### Event Driven Framework

Allows you to create handlers to respond to interface events as they occur. 

The Lightning Console JavaScript API provides several events specific to Lightning console apps.

Events are fired from JavaScript **controller** actions. Events can contain attributes (event parameters) that can be set (**setParams()**) before the event is fired and read (**getParam()**) when the event is handled. 


#### Example : Handling event - tab closing

Component:

```xml
<aura:component implements="flexipage:availableForAllPageTypes" access="global" >
    <force:workspaceAPIAccess aura:id="workspace" />	
    <aura:handler event="force:tabClosed" action="{! c.onTabClosed }"/> 
    
</aura:component>

```

Controller:

```javascript


    onTabClosed : function(component, event, helper) {
        // you can get use: event.getParams() to get Event parameters
        // eg. var tabid = event.getParam('tabId');

        confirm("Do you really want to close this tab?");
    }


```

#### Example : Firing an event 


```javascript


navHome : function (component, event, helper) {
    var homeEvent = $A.get("e.force:navigateToObjectHome");
    
    // setting params for the event
    homeEvent.setParams({
        "scope": "myNamespace__myObject__c"
    });
    
    // fire the event!
    homeEvent.fire();
}

```

#### References

1. [Salesforce Console Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.api_console.meta/api_console/sforce_api_console.htm)

