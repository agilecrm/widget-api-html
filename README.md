AGILE CRM HTML WIDGETS
=================

This API allows you to develop HTML widgets for AgileCRM.   

Widgets are small application that can be built by end users and embedded in a contact’s page in AgileCRM. They are HTML/JavaScript segments that are executed whenever a contact page is loaded. Every contact page contains an HTML segment allocated for custom widget.

Custom widgets have access to contact details and/or external resources in order to display meaningful data. These widgets add extra functionality to the AgileCRM contacts page.

*Custom* tab, located at `https://<your_domain>.agilecrm.com/#add-widget/` provides the option to add a custom widget. Upon clicking the dropdown, you see that there are two ways to upload a widget - HTML and URL. You have to select `HTML` option from drop down menu to use methods in this doc.

HTML/JavaScript segment are embedded on the contact page. It is executed at the AgileCRM server whenever a contact page loads. It can access external resource in the form of jsonp object.    
***Access:*** Global widget property, widget property wrt to contact, and contact details.

####Notes:   
1. AgileCRM supports HTML5 specifications. Please write a widget which is compatible with HTML5.  
 
2. jQuery library (version 1.7.2) is already loaded and ready for use if needed. Do not import any other version jquery library with the widget as it can result into compatibility issues.   

3. As modern browsers have started to block [mixed content](https://blog.mozilla.org/security/2013/05/16/mixed-content-blocking-in-firefox-aurora/), all resources must be accessed using secure (https) connection.
   
Functions can be broadly categorized in the following three groups based on their domain.

**I. GLOBAL WIDGET PROPERTIES:** These functions have access to global widget properties.

      1. agile_crm_get_widget(widgetName)
      2. agile_crm_save_widget_prefs(widgetName, preferences)
      3. agile_crm_get_widget_prefs(widgetName)
      4. agile_crm_delete_widget_prefs(widgetName)

**II. LOCAL WIDGET PROPERTY WRT. CONTACT:** These functions have access to local properties of widgets wrt. to a contact. These are contact specific widget properties. Anything accessible here is local to the contact page on which the widget loads.

      1. agile_crm_save_widget_property_to_contact(propertyName, propertyValue)
      2. agile_crm_get_widget_property_from_contact(propertyName)
      3. agile_crm_delete_widget_property_from_contact(propertyName)

**III. CONTACT PROPERTY:** These functions have access to and are used to modify the properties of the contact on which the widget is loaded. 

      1. agile_crm_get_contact()
      2. agile_crm_get_contact_property(propertyName)
      3. agile_crm_get_contact_properties_list(propertyName)
      4. agile_crm_get_contact_property_by_subtype(propertyName, subtype)
      5. agile_crm_save_contact_property(propertyName, subtype, value, type)
      6. agile_crm_update_contact(propertyName, value)
      7. agile_crm_update_contact_properties(propertiesArray)
      8. agile_crm_delete_contact_property_by_subtype(propertyName, subtype, value)

Looking at the functions in detail: 

####I. GLOBAL WIDGET PROPERTIES

*`widgetName` represents the name of widget which is mentioned while adding widget on the ***Add Widget*** (`https://<your_domain>.agilecrm.com/#add-widget`) page.

####**a) agile_crm_get_widget(widgetName)**   
**Parameters:**  widgetName   
**Response:**  JSON Object   

   This fetches the widget object with the name of `widgetName` in the form of JSON object. JSON object contains the name, description, logo url, script attached to the widget and all other details related to the widget.

            var widgetDetails = agile_crm_get_widget(widgetName);
   
####**b) agile_crm_save_widget_prefs(widgetName, pref)**   
**Parameters:**  widgetName, pref   
**Response:**  nil   

   This function can be used to save global widget preference. `pref` should be a stringified JSON object. If the preference already exists it will be overwritten.
   
            var jPref = {"user_name":"foo", "password":"bar"};
            var pref = JSON.stringify(jPref);
            agile_crm_save_widget_prefs(widgetName, pref);

####**c) agile_crm_get_widget_prefs(widgetName)**   
**Parameters:**  widgetName    
**Response:** JSON string    

   This function is used to get the global widget preference. If there's some preference set, it returns its stringified JSON representation. Otherwise, it returns `undefined` object.

            var widgetPrefs = agile_crm_get_widget_prefs(widgetName);

####**d) agile_crm_delete_widget_prefs(widgetName)**   
**Parameters:**  widgetName    
**Response:**  nil    

   This function is used to delete preference of a widget and fully resets it.
    
            var prefs = agile_crm_delete_widget_prefs(widgetName)


####II. LOCAL WIDGET PROPERTY WRT. CONTACT   

####**a) agile_crm_save_widget_property_to_contact(propertyName, propertyValue)**    
**Parameters:**  propertyName, propertyValue    
**Response:**  nil   

   It is used to save the property of a widget with respect to the contact pages on which widget loads. 

            var propertyName = "contact_widget_id";
            var propertyValue = "12345";
            agile_crm_save_widget_property_to_contact(propertyName, propertyValue);

####**b) agile_crm_get_widget_property_from_contact(propertyName)**   
**Parameters:**  propertyName    
**Response:**  String   

   A call to this function fetches the value associated with `propertyName` if present, otherwise returns `undefined`.

            var propertyName = "custom_widget_id";
            var propertyValue = agile_crm_get_widget_property_from_contact(propertyName);

####**c) agile_crm_delete_widget_property_from_contact(propertyName)**   
**Parameters:**  propertyName    
**Response:** nil    

   This function can be used to delete the property key-value pair from the contact-widget property field.

            var propertyName = "custom_widget_id";
            agile_crm_delete_widget_property_from_contact(propertyName);

####III. CONTACT PROPERTY   
####**a) agile_crm_get_contact()**   
**Parameters:**  nil    
**Response:**  JSON Object   

   This function returns the object in the form of a JSON object. More details about the structure of contact object is provided at the [bottom](#contact-structure) of this page.

            var jsonData = agile_crm_get_contact();

####**b) agile_crm_get_contact_property(propertyName)**   
**Parameters:**  propertyName    
**Response:** String   

   This function returns the property associated with `propertyName`, if present, otherwise returns `undefined`.

              var propertyName = "first_name";
              var propertyValue = agile_crm_get_contact_property(propertyName);  // returns the first name of contact.

####**c) agile_crm_get_contact_properties_list(propertyName)**   
**Parameters:**  propertyName    
**Response:** Array [JSON Object]    

   This function fetches all the value associated with `propertyName` in a array. If there's no property present, it will return an empty array.

              var propertyName = "email";
              var propertyValue = agile_crm_get_contact_properties_list(propertyName); // returns list of all emails associated to current contact.

####**d) agile_crm_get_contact_property_by_subtype(propertyName, subtype)**   
**Parameters:**  propertyName, subtype   
**Response:**  String   


   This will return the current contact property with key `propertyName` and possess a subtype of (`subtype`). If there's no result, it will return `undefined` object.

              var propertyName = "email";
              var subtype = "work";
              var result = agile_crm_get_contact_property_by_subtype(propertyName, subtype);
         
####**e) agile_crm_save_contact_property(propertyName, subtype, value, type)**   
**Parameters:**  propertyName, subtype, value, type    
**Response:**  nil   

   This function will save the property with all of its details.

              var propertyName = "email";
              var subtype = "work";
              var value = "foo@bar.com";
              var type = "SYSTEM";  // because email is system defined property.
              agile_crm_save_contact_property(propertyName, subtype, value, type);

####**f) agile_crm_update_contact(propertyName, value)**   
**Parameters:**  propertyName, value    
**Response:**  nil   

   This function is used to update current contact property and assign it the value `value`.

              var propertyName = "email";
              var value = "foo@bar.com";
              agile_crm_update_contact(propertyName, value);

####**g) agile_crm_update_contact_properties(propertiesArray)**   
**Parameters:**  propertiesArray    
**Response:**  nil   

   This function is used to update multiple properties with a single function call.

              var pref = [
                            {"name": "first_name", "value":"foo"}, 
                            {"name":"last_name", "value": "bar"},
                            {"name": "email", "value":"foo@bar.com", "type":"SYSTEM", "subtype":"home"}
                         ];
              agile_crm_update_contact_properties(pref);

####**h) agile_crm_delete_contact_property_by_subtype(propertyName, subtype, value)**   
**Parameters:**  propertyName, subtype, value    
**Response:**  nil   

   This function can be used to remove the property mentioned by the arguments.

              var propertyName = "email";
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
                "email": "user@agilecrm.com",
                "is_admin": false
            }
        }

