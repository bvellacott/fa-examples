# fa-examples
Contains examples of using the force adapter

# To setup the examples in a salesforce developer org follow these steps

1. go to https://developer.salesforce.com/signup
2. Sign up for a developer org
3. Once you are signed up and logged in go to click your username at the top right of the page and go to 'My Settings'
4. In the quick find field at the left of the page write 'reset my security token' and click the link that appears
5. You will get an email in to the address you specified when you created the org with the security token. Save the token.
6. Go to https://githubsfdeploy.herokuapp.com/app/githubdeploy/bvellacott/fa-examples and follow the login process until you land on a page with a 'Deploy' button on it. Click deploy and the heroku tool will deploy the examples into your org from github.
7. Login/Go to your org and go to the page [org hostname i.e. https://c.eu5.visual.force.com]/apex/CrmDynamic and you should be on an example page.

# Example pages

There are two example pages:
 - [salesforce hostname]/apex/CrmDynamic
 - [salesforce hostname]/apex/CrmStatic

They're trying to be a very simplified version of a crm and are really the same page, but with one difference. CrmDynamic generates the ember models dynamically on page load from the salesforce sobject schema and the ember app needs to be initialised in a callback. CrmStatic uses a static ember model definition when initialised and the ember app doesn't need to be initialised in a callback. Also CrmDynamic contains a model definitions download button which produces the static definition that CrmStatic uses. You can utilise the same functionality from the browser console with the method: 
  Papu.SF.downloadEmberModels(typeFilter)
where the type filter is a function which takes an sobject definition (i.e. salesforce object) and returns true or false based on whether or not you want to create an ember model definition for the sobject (i.e. use it in your ember app).
A type filter could be for example:
  var typeFilter = function(obj) { return obj.name === 'Account' || obj.name === 'User'; }
This would create ember models for the Account and User objects and no other salesforce objects could then be used in your app. If you don't pass in a typeFilter, ember models will be created for all sobjects.

# Custom objects

In salesforce, when you define your own sobject an '__c' extension is added to the end of the object name. Unfortunately this doesn't work with ember since ember doesn't seem to like underscores. Therefore when the force adapter creates an ember model for your custom sobject it modifies the extension to 'ccc'. Thus if you have created a custom sobject called Customer__c, you will have to reference it by the name Customerccc in your ember app.

# Soap primitive types

Salesforce provides a rainbow of primitive types for field values and theyre definitions can be seen at:
https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/field_types.htm. However, out of the box, ember provides only a few. Here you can see the conversion map that the salesforce adapter uses to translate between these:
                   salesforce | ember
		                     'id' : 'string'
		                'boolean' : 'boolean
		                 'string' : 'string'
		               'datetime' : 'date'
		               'currency' : 'number'
		                   'date' : 'date'
		                  'email' : 'string'
		                    'int' : 'number'
		                 'double' : 'number'
		                'percent' : 'number'
		               'location' : 'string'
		                  'phone' : 'string'
		               'picklist' : 'string'
		          'multipicklist' : 'string'
		               'textarea' : 'string'
		        'encryptedstring' : 'string'
		                    'url' : 'string'
		                'address' : 'string'
		             'calculated' : 'string'
		               'combobox' : 'string'
 'datacategorygroupreference' : 'string'
		        'encryptedstring' : 'string'
		         'junctionidlist' : 'string'
		           'masterrecord' : 'string'

You can easily find this list in the forceAdapter.resource file and modify it if you have your own primitive types defined for ember.
