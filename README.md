AGILE CRM WIDGETS
=================

This API allows you to develop widgets for Agile CRM.   

Widgets are small application which can be built by end users and embedded in the contact’s page in Agile CRM. It is a HTML/JavaScript segment which is executed whenever a contact page is loaded. Every contact page has some html segment allocated for the widget.

These widgets have access to contact details and/or external resources in order to return some meaningful collection of data. These widgets add extra functionality to the AgileCRM contacts page.

*Custom* tab, located at `https://<your_domain>.agilecrm.com/#add-widget/` provides option to add widget. There are two methods to upload a widget.

HTML/JavaScript segment are simply embedded in the contact page html. It is executed at the AgileCRM server whenever a contact page loads. It can access external resource in the form of jsonp object.    
***Access:*** Global widget property, widget property wrt to contact and contact details.

####Notes:   
1. AgileCRM support HTML5 specifications. Please write widget which is compatible with HTML5.  
 
2. jQuery library (version 1.7.2) is already loaded. Feel free to use it. And don't import any other version jquery library in widget as it can result into compatibility issue.   

3. As modern browsers are blocking [mixed content](https://blog.mozilla.org/tanvi/2013/04/10/mixed-content-blocking-enabled-in-firefox-23/), all resources must be accessed using secure (https) connection.
   

HTML/SCRIPT
---
This section contain the description of javascript functions which end user can use while writing the widgets using *HTML* option.

These functions can be broadly categorized in the following three groups based on their domain.

**I. GLOBAL WIDGET PROPERTIES:** These functions have access to global widget properties.

      1. agile_crm_get_widget(widgetName)
      2. agile_crm_save_widget_prefs(widgetName, preferences)
      3. agile_crm_get_widget_prefs(widgetName)
      4. agile_crm_delete_widget_prefs(widgetName)

**II. LOCAL WIDGET PROPERTY WRT. CONTACT:** These functions have access to local properties of widgets wrt. to contact. These are contact specific widget property. Anything accessible here is local to contact page on which the widget loads.

      1. agile_crm_save_widget_property_to_contact(propertyName, propertyValue)
      2. agile_crm_get_widget_property_from_contact(propertyName)
      3. agile_crm_delete_widget_property_from_contact(propertyName)

**III. CONTACT PROPERTY:** These functions have access to the properties of the contacts. 

      1. agile_crm_get_contact()
      2. agile_crm_get_contact_property(propertyName)
      3. agile_crm_get_contact_properties_list(propertyName)
      4. agile_crm_get_contact_property_by_subtype(propertyName, subtype)
      5. agile_crm_save_contact_property(propertyName, subtype, value, type)
      6. agile_crm_update_contact(propertyName, value)
      7. agile_crm_update_contact_properties(propertiesArray)
      8. agile_crm_delete_contact_property_by_subtype(propertyName, subtype, value)

####I. GLOBAL WIDGET PROPERTIES

*`widgetName` represents the name of widget which is mentioned while adding widget on the ***Add Widget*** (`https://<your_domain>.agilecrm.com/#add-widget`) page.

**a) agile_crm_get_widget(widgetName)**   
**Arguments:**  widget name    
**Response:**  JSON Object   

   This fetches the widget object with the name of `widgetName` in the form of json object. JSON object contains the name, description, url, logo url, script attached to the widget and all other details related to the widget.

      Syntax:
            agile_crm_get_widget(widgetName)
              where,
                widgetName = Widget name
                
      Eg.   var widgetDetails = agile_crm_get_widget(widgetName);

**b) agile_crm_save_widget_prefs(widgetName, pref)**   
**Arguments:**  widget name, preference  (stringified json object)    
**Response:**  nil   

   This function can be used to save global widget preference. `pref` should be a stringified json object. In case, if the preference already exists it will overwrite it.
   
      Syntax:
            agile_crm_save_widget_prefs(widgetName, pref)
              where,
                widgetName = Widget name
                pref = Stringified json object.

      Eg.   var jPref = {"user_name":"foo", "password":"bar"};
            var pref = JSON.stringify(jPref);
            agile_crm_save_widget_prefs(widgetName, pref);

**c) agile_crm_get_widget_prefs(widgetName)**   
**Arguments:**  widget name   
**Response:** JSON string    

   This function is used to get the global widget preference. If there's some preference present then it returns its stringified json representation otherwise `undefined` object.

      Syntax: 
            agile_crm_get_widget_prefs(widgetName)
              where,
                widgetName = Widget name

      Eg.   var widgetPrefs = agile_crm_get_widget_prefs(widgetName);

**d) agile_crm_delete_widget_prefs(widgetName)**   
**Arguments:**  widget name   
**Response:**  nil    

   This function is used to delete the complete preference of the widget and resets it to original setting.

      Syntax:
            agile_crm_delete_widget_prefs(widgetName)
              where,
                widgetName = Widget name
    
      Eg.   var prefs = agile_crm_delete_widget_prefs(widgetName)


####II. LOCAL WIDGET PROPERTY WRT. CONTACT   

**a) agile_crm_save_widget_property_to_contact(propertyName, propertyValue)**    
**Arguments:**  property name, value    
**Response:**  nil   

   It is used to save the property of a widget with respect to the contact pages on which widget loads. 

      Syntax:
            agile_crm_save_widget_property_to_contact(propertyName, propertyValue)
              where,
                propertyName = Property name which has to be modified.
                propertyValue = value to be assigned to propertyName

      Eg.   var propertyName = "contact_widget_id";
            var propertyValue = "12345";
            agile_crm_save_widget_property_to_contact(propertyName, propertyValue);

**b) agile_crm_get_widget_property_from_contact(propertyName)**   
**Arguments:**  property name    
**Response:**  String   

   Call to this function fetches the value associated to `propertyName`, if present, otherwise `undefined`.

      Syntax:
            agile_crm_get_widget_property_from_contact(propertyName)
              where,
                propertyName = Name of property which is to be accessed.

      Eg.   var propertyName = "custom_widget_id";
            var propertyValue = agile_crm_get_widget_property_from_contact(propertyName);

**c) agile_crm_delete_widget_property_from_contact(propertyName)**   
**Arguments:**  property name    
**Response:** nil    

   This function can be used to delete the property key value pair from the contact-widget property field.

      Syntax:
            agile_crm_delete_widget_property_from_contact(propertyName)
              where,
                propertyName = Name of property which is to be accessed.

      Eg.   var propertyName = "custom_widget_id";
            agile_crm_delete_widget_property_from_contact(propertyName);

####III. CONTACT PROPERTY   
**a) agile_crm_get_contact()**   
**Arguments:**  NIL    
**Response:**  JSON Object   

   This function returns the object in the form of json object. More details about the structure of contact object is provided in URL section above.

        Syntax: 
            agile_crm_get_contact()

        Eg. var jsonData = agile_crm_get_contact();

**b) agile_crm_get_contact_property(propertyName)**   
**Arguments:**  property name    
**Response:** String   

   This function returns the property associated with `propertyName`, if it exists, otherwise returns `undefined`.

        Syntax:
              agile_crm_get_contact_property(propertyName)
                where,
                  propertyName = Name of property which is to be accessed.

        Eg.   var propertyName = "first_name";
              var propertyValue = agile_crm_get_contact_property(propertyName);  // returns the first name of contact.

**c) agile_crm_get_contact_properties_list(propertyName)**   
**Arguments:**  property name    
**Response:** Array [JSON Object]    

   This function fetches all the value associated to `propertyName` in a array. If there's no such property existed, it will return an empty array.

        Syntax:
              agile_crm_get_contact_properties_list(propertyName)
                where,
                  propertyName = Name of property which is to be accessed.

        Eg.   var propertyName = "email";
              var propertyValue = agile_crm_get_contact_properties_list(propertyName); // returns list of all emails associated to current contact.

**d) agile_crm_get_contact_property_by_subtype(propertyName, subtype)**   
**Arguments:**  property name, property subtype    
**Response:**  String   


   This will return the current contact property with key `propertyName` and possess a subtype of `subtype`. If there's no result, then it will return `undefined` object.

        Syntax:
              agile_crm_get_contact_property_by_subtype(propertyName, subtype)
                where,
                  propertyName = Name of property which is to be accessed.
                  subtype = Subtype of the property

        Eg.   var propertyName = "email";
              var subtype = "work";
              var result = agile_crm_get_contact_property_by_subtype(propertyName, subtype);
         
**e) agile_crm_save_contact_property(propertyName, subtype, value, type)**   
**Arguments:**  property name, property subtype, property value, property type    
**Response:**  nil   

   This function will save the property with the complete details about it.

        Syntax:
              agile_crm_save_contact_property(propertyName, subtype, value, type);
                where,
                  propertyName = Name of property which is to be accessed.
                  subtype = Subtype of the property
                  value = value to be stored
                  type = "SYSTEM" (for the system defined property) or "CUSTOM" (for new property)

        Eg.   var propertyName = "email";
              var subtype = "work";
              var value = "foo@bar.com";
              var type = "SYSTEM";  // because email is system defined property.
              agile_crm_save_contact_property(propertyName, subtype, value, type);

**f) agile_crm_update_contact(propertyName, value)**   
**Arguments:**  property name, value    
**Response:**  nil   

   This function can also be used to update current contact property and assigns it the value `value`.

        Syntax:
              agile_crm_update_contact(propertyName, value)
                where,
                  propertyName = Name of property which is to be accessed.
                  value = value to be stored

        Eg.   var propertyName = "email";
              var value = "foo@bar.com";
              agile_crm_update_contact(propertyName, value);

**g) agile_crm_update_contact_properties(propertiesArray)**   
**Arguments:**  properties array    
**Response:**  nil   

   This function can be used to update multiple properties with single function call.

      Syntax:
              agile_crm_update_contact_properties(propertiesArray);
                where,
                  propertiesArray = Array of json object, where each object represents one property.

      Eg.     var pref = [
                            {"name": "first_name", "value":"foo"}, 
                            {"name":"last_name", "value": "bar"},
                            {"name": "email", "value":"foo@bar.com", "type":"SYSTEM", "subtype":"home"}
                         ];
              agile_crm_update_contact_properties(pref);

**h) agile_crm_delete_contact_property_by_subtype(propertyName, subtype, value)**   
**Arguments:**  property name, property subtype, property value    
**Response:**  nil   

   This function can be used to remove the property mentioned by the arguments.

        Syntax:
              agile_crm_delete_contact_property_by_subtype(propertyName, subtype, value);
                where,
                  propertyName = Name of property which is to be accessed.
                  subtype = Subtype of the property
                  value = value to be stored

        Eg.   var propertyName = "email";
              var subtype = "work";
                var value = "foo@bar.com";
                agile_crm_delete_contact_property_by_subtype(propertyName, subtype, value);



Contact Structure
---
        {
            "id": 1435,`
            "type": "PERSON",
            "created_time": 1375424495,
            "updated_time": 1380003638,
            "viewed": {
                "viewed_time": 1380003638330,
                "viewer_id": 1
            },
            "star_value": 0,
            "lead_score": 0,
            "tags": [
                "lead"
            ],
            "tagsWithTime": [
                {
                    "tag": "lead",
                    "createdTime": 1378131285793,
                    "entity_type": "tag"
                }
            ],
            "properties": [
                {
                    "type": "SYSTEM",
                    "name": "last_name",
                    "subtype": null,
                    "value": "test"
                },
                {
                    "type": "SYSTEM",
                    "name": "title",
                    "subtype": null,
                    "value": "Software Developer at Agile CRM"
                },
                {
                    "type": "SYSTEM",
                    "name": "email",
                    "subtype": "",
                    "value": "test@agilecrm.com"
                },
                {
                    "type": "CUSTOM",
                    "name": "image",
                    "subtype": null,
                    "value": "https://www.agilecrm.com/img/agile-crm.png"
                }
            ],
            "widget_properties": null,
            "owner": {
                "id": 1450,
                "email": "tejaswi@agilecrm.com",
                "is_admin": false
            }
        }

