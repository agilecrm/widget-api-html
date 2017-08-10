AGILE CRM HTML WIDGETS
=================

This API allows you to develop HTML widgets for AgileCRM.   

Widgets are small applications that can be built by end users and embedded in a contacts/companies page in AgileCRM. They are HTML/JavaScript segments that are executed whenever a contact/company page is loaded. Every contacts/company page contains a HTML segment allocated for custom widget.

Custom widgets have access to contact/company details and/or external resources in order to display meaningful data. These widgets add extra functionality to the AgileCRM contact/companies page.

*Custom* tab, located at `https://<your_domain>.agilecrm.com/#add-widget/` provides the option to add a custom widget. Upon clicking the dropdown, you see that there are two ways to upload a widget - HTML and URL. You have to select `HTML` option from drop down menu to use methods in this doc.

HTML/JavaScript segment are embedded on the contact/company page. It is executed at the AgileCRM server whenever a contact/company page loads. It can access external resource in the form of jsonp object.    

####Notes:   
1. AgileCRM supports HTML5 specifications. Please write a widget which is compatible with HTML5.  
 
2. jQuery library (version 1.10.2) is already loaded and ready for use if needed. Do not import any other version jquery library with the widget as it can result into compatibility issues.   

3.  Bootstrap css & javascript library (version v3.0.0) is already loaded and ready for use if needed. Do not import any other version Bootstrap library with the widget as it can result into compatibility issues.

4. As modern browsers have started to block [mixed content](https://blog.mozilla.org/security/2013/05/16/mixed-content-blocking-in-firefox-aurora/), all resources must be accessed using secure (https) connection.

***Access:*** Global widget property, widget property wrt to contact, and contact details.
   
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

**III. CONTACT PROPERTY:** These functions have access to contact object and are used to modify the properties of the contact on which the widget is loaded. 

      1. agile_crm_get_contact()
      2. agile_crm_get_contact_property(propertyName)
      3. agile_crm_get_contact_properties_list(propertyName)
      4. agile_crm_get_contact_property_by_subtype(propertyName, subtype)
      5. agile_crm_save_contact_property(propertyName, subtype, value, type)
      6. agile_crm_update_contact(propertyName, value)
      7. agile_crm_update_contact_properties(propertiesArray)
      8. agile_crm_delete_contact_property_by_subtype(propertyName, subtype, value)

**IV. COMPANY PROPERTY:** These functions have access to company object and are used to modify the properties of the company on which the widget is loaded. 

      1. agile_crm_get_company()
      2. agile_crm_get_company_property(propertyName)
      3. agile_crm_get_company_properties_list(propertyName)
      4. agile_crm_get_company_property_by_subtype(propertyName, subtype)
      5. agile_crm_save_company_property(propertyName, subtype, value, type)
      6. agile_crm_update_company(propertyName, value)
      7. agile_crm_update_company_properties(propertiesArray)
      8. agile_crm_delete_company_property_by_subtype(propertyName, subtype, value)

Looking at the functions in detail:

####I. GLOBAL WIDGET PROPERTIES

*`widgetName` represents the name of widget which is mentioned while adding widget on the ***Custom Widget*** (`https://<your_domain>.agilecrm.com/#Custom-widget`) page.

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
            "id": 4521191813414912,
            "type": "PERSON",
            "star_value": 0,
            "lead_score": 0,
            "klout_score": "",
            "tags": ["glod_plan"],
            "tagsWithTime": [{
                "tag": "glod_plan",
                "createdTime": 1502259152466,
                "availableCount": 0,
                "entity_type": "tag"
            }],
            "properties": [{
                "type": "CUSTOM",
                "name": "Date of Birth",
                "value": "599077800"
            }, {
                "type": "CUSTOM",
                "name": "level",
                "value": "Glod"
            }, {
                "type": "SYSTEM",
                "name": "first_name",
                "value": "Sid"
            }, {
                "type": "SYSTEM",
                "name": "last_name",
                "value": "Bravo"
            }, {
                "type": "SYSTEM",
                "name": "company",
                "value": "Agile CRM"
            }, {
                "type": "SYSTEM",
                "name": "title",
                "value": "Mr"
            }, {
                "type": "SYSTEM",
                "name": "email",
                "subtype": "",
                "value": "sid.bravo@gmail.com"
            }, {
                "type": "SYSTEM",
                "name": "email",
                "subtype": "work",
                "value": "bravo.sid@gmail.com"
            }, {
                "type": "SYSTEM",
                "name": "email",
                "subtype": "home",
                "value": "sid.b@ymail.com"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "home",
                "value": "+19908164425"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "work",
                "value": "+18099128809"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "mobile",
                "value": "+18919198785"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "",
                "value": "+19030528764"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "",
                "value": "+19603784425"
            }, {
                "type": "SYSTEM",
                "name": "website",
                "subtype": "URL",
                "value": "http://www.sidbravo.com"
            }, {
                "type": "SYSTEM",
                "name": "website",
                "subtype": "",
                "value": "http://www.ymail.com"
            }, {
                "type": "SYSTEM",
                "name": "website",
                "subtype": "YOUTUBE",
                "value": "http://www.youtube.com/knowbravo"
            }, {
                "type": "SYSTEM",
                "name": "address",
                "subtype": "work",
                "value": "{\"zip\":\"415\",\"address\":\"1st block, cherry towers\",\"state\":\"Texas\",\"countryname\":\"United States\",\"country\":\"US\",\"city\":\"Dallas\"}"
            }],
            "contact_company_id": "4644337115725824"
        }


####IV. COMPANY PROPERTY   
####**a) agile_crm_get_company()**   
**Parameters:**  nil    
**Response:**  JSON Object

   This function returns the object in the form of a JSON object. More details about the structure of company object is provided at the [bottom](#company-structure) of this page.

            var jsonData = agile_crm_get_company();

####**b) agile_crm_get_company_property(propertyName)**   
**Parameters:**  propertyName    
**Response:** String   

   This function returns the property associated with `propertyName`, if present, otherwise returns `undefined`.

              var propertyName = "name";
              var propertyValue = agile_crm_get_company_property(propertyName);  // returns the first name of company.

####**c) agile_crm_get_company_properties_list(propertyName)**   
**Parameters:**  propertyName    
**Response:** Array [JSON Object]    

   This function fetches all the value associated with `propertyName` in a array. If there's no property present, it will return an empty array.

              var propertyName = "email";
              var propertyValue = agile_crm_get_company_properties_list(propertyName); // returns list of all emails associated to current company.

####**d) agile_crm_get_company_property_by_subtype(propertyName, subtype)**   
**Parameters:**  propertyName, subtype   
**Response:**  String   


   This will return the current company property with key `propertyName` and possess a subtype of (`subtype`). If there's no result, it will return `undefined` object.

              var propertyName = "email";
              var subtype = "primary";
              var result = agile_crm_get_company_property_by_subtype(propertyName, subtype);
         
####**e) agile_crm_save_company_property(propertyName, subtype, value, type)**   
**Parameters:**  propertyName, subtype, value, type    
**Response:**  nil   

   This function will save the property with all of its details.

              var propertyName = "email";
              var subtype = "primary";
              var value = "foo@bar.com";
              var type = "SYSTEM";  // because email is system defined property.
              agile_crm_save_company_property(propertyName, subtype, value, type);

####**f) agile_crm_update_company(propertyName, value)**   
**Parameters:**  propertyName, value    
**Response:**  nil   

   This function is used to update current company property and assign it the value `value`.

              var propertyName = "email";
              var value = "foo@bar.com";
              agile_crm_update_company(propertyName, value);

####**g) agile_crm_update_company_properties(propertiesArray)**   
**Parameters:**  propertiesArray    
**Response:**  nil   

   This function is used to update multiple properties with a single function call.

              var pref = [
                            {"name": "name", "value":"foo"},
                            {"name": "email", "value":"foo@bar.com", "type":"SYSTEM", "subtype":"primary"}
                         ];
              agile_crm_update_company_properties(pref);

####**h) agile_crm_delete_company_property_by_subtype(propertyName, subtype, value)**   
**Parameters:**  propertyName, subtype, value    
**Response:**  nil   

   This function can be used to remove the property mentioned by the arguments.

              var propertyName = "email";
              var subtype = "primary";
              var value = "foo@bar.com";
              agile_crm_delete_company_property_by_subtype(propertyName, subtype, value);



Company Structure
---
        {
            "id": 4644337115725824,
            "type": "COMPANY",
            "star_value": 5,
            "lead_score": 150,
            "klout_score": "",
            "tags": ["glod_plan"],
            "tagsWithTime": [{
                "tag": "glod_plan",
                "createdTime": 1502262765637,
                "availableCount": 0,
                "entity_type": "tag"
            }],
            "properties": [{
                "type": "CUSTOM",
                "name": "Established Date",
                "value": "1293301800"
            }, {
                "type": "CUSTOM",
                "name": "Level",
                "value": "High"
            }, {
                "type": "SYSTEM",
                "name": "name",
                "value": "Agile CRM"
            }, {
                "type": "SYSTEM",
                "name": "url",
                "value": "https://www.agilecrm.com"
            }, {
                "type": "SYSTEM",
                "name": "email",
                "subtype": "primary",
                "value": "sales@agilecrm.com"
            }, {
                "type": "SYSTEM",
                "name": "email",
                "subtype": "alternate",
                "value": "info@agilecrm.com"
            }, {
                "type": "SYSTEM",
                "name": "email",
                "subtype": "",
                "value": "support@agilecrm.com"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "primary",
                "value": "+1-800-980-0729"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "alternate",
                "value": "+44 14428 00729"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "other",
                "value": "+61 285990729"
            }, {
                "type": "SYSTEM",
                "name": "phone",
                "subtype": "",
                "value": "+91 9492227799"
            }, {
                "type": "SYSTEM",
                "name": "website",
                "subtype": "URL",
                "value": "http://www.agilecrm.com"
            }, {
                "type": "SYSTEM",
                "name": "website",
                "subtype": "TWITTER",
                "value": "agilecrm"
            }, {
                "type": "SYSTEM",
                "name": "address",
                "subtype": "work",
                "value": "{\"zip\":\"73301\",\"address\":\"white blocks, 7th avenu\",\"state\":\"Texas\",\"countryname\":\"United States\",\"country\":\"US\",\"city\":\"Dallas\"}"
            }, {
                "type": "SYSTEM",
                "name": "name_lower",
                "value": "agile crm"
            }]
        }