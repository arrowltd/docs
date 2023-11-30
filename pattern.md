## Validate with a list

![telegram-cloud-photo-size-5-6055620501579083144-x](telegram-cloud-photo-size-5-6055620501579083144-x.png)

Using `found` variable

## Error

return when user needs to see the error (input error, validation error)

should not return error when user does not need to see the error (it will be returned to user as `err:internal_server_error`), so most of api call to 3rd party, database query error etc

panic only if need Tech Lead to check (in production, every panic will notify Tech Lead). So another way to think if should panic or not is to ask yourself is this error need check by Tech Lead in production server?

Some panic cases:
* API call to 3rd party
* Most errors from db

If user does not need to see, but also no need to ask Tech Lead to check, then log it to file (using LogSerious)



## JSON Field Validation

![image-20211103095200230](image-20211103095200230.png)

## Database Execution

To execute database: insert, update, upsert. In the source will have the functions to support that.

If these functions does not support the case your are facing let dicuss with TeachLead.

Insert:

![Screen Shot 2023-11-20 at 16 09 50](https://github.com/arrowltd/docs/assets/17697751/adadf160-5a02-4b4e-8e0f-5146ba2bf326)

Update:

![Screen Shot 2023-11-20 at 16 10 28](https://github.com/arrowltd/docs/assets/17697751/cc34d7a3-1f6b-4cc6-8e54-98fead391223)

Upsert:

![Screen Shot 2023-11-20 at 16 11 50](https://github.com/arrowltd/docs/assets/17697751/06f07e35-5203-4896-8140-59d9b51760ab)

## Select/Query functions.
If you are facing the case you need to Select/Queries in a table with the same columns but with different conditions. To do this you need to define a original function with input a **Condition List** then you define a related function (name, conditons) base on its logically.

Example:

- Write a original function:

![Screen Shot 2023-11-20 at 16 25 55](https://github.com/arrowltd/docs/assets/17697751/7d6181ba-c57c-410d-a10b-37a40fc441d8)

- Define related functions with the original one.

![Screen Shot 2023-11-20 at 16 30 13](https://github.com/arrowltd/docs/assets/17697751/e0619731-e691-4535-a884-ba62533e72ef)

![Screen Shot 2023-11-20 at 16 30 48](https://github.com/arrowltd/docs/assets/17697751/8c46daa7-0429-4743-89a5-5486b58d00d4)

## Center Format.

If a feature need to create a new package, we must to add a object center at this package and all the functions/features/logics  will be called through this object.

Example: We have a role feature and it needs to be defined in a separate package.

Step 1: we create a new package and define an object center (RoleCenter) in this package 

![image](https://github.com/arrowltd/docs/assets/17697751/8b57ec94-8095-4080-bcdd-9a4f85677117)

Step 2: Define functions/feature/logic in role package and attached to RoleCenter

![image](https://github.com/arrowltd/docs/assets/17697751/47c5a1ef-4479-4dba-ac5f-2fd9dfee09c0)

Step 3: Go back to the models and init Role Center at there

![image](https://github.com/arrowltd/docs/assets/17697751/f1dcc645-08dd-4ef0-83b8-4cbe34b21c05)

Step 4: Call the functions in object center (Role Center) base on the logic if needed.

![image](https://github.com/arrowltd/docs/assets/17697751/5d815cd5-6cc2-41d6-8d15-934b35d1f53a)

### Identify the relationship between multiple centers: 
For some cases you create object center  and this center maybe need to call features/logics from another centers.
To do this we need to indentify the relationship between the centers firstly (discuss with TeachLead to make sure this).

#### Center contains the others.
Example: We have SubAccountCenter And RoleCenter, we can realize SubAccountCenter contains RoleCenter for sure to define that we have to do:

Step1: Define a RoleCenter like the step at example above.

Step 2: Define a SubCenter constains RoleCenters

![image](https://github.com/arrowltd/docs/assets/17697751/135def74-bbd9-4333-aff0-48df6b3a8f12)

Step3: Init the SubCenter at models

![image](https://github.com/arrowltd/docs/assets/17697751/6b45f2af-489f-48ef-afec-aefa236a1b58)

For some reasons/logics roleCenter need to action back to subCenter we shoudn’t define subCenter at roleCenter. If you do that we will get the error “import cycle not allow”

Example:

![image](https://github.com/arrowltd/docs/assets/17697751/397a4e1a-08a5-4fdb-9681-75853587262e)

To do this case: 

Step 1: Add a attributes (roleDelegate) into subCenter

![image](https://github.com/arrowltd/docs/assets/17697751/c84a6541-cd06-484e-9ba9-79e250672416)

Step2: Comeback to models and define a function RoleCallBackHandler

![image](https://github.com/arrowltd/docs/assets/17697751/9def3e8f-7df6-41a9-9e46-376407794fef)

Step3: At the subCenter you just call RoleCallbackHandler if you need some update from role.

![image](https://github.com/arrowltd/docs/assets/17697751/d7b05d66-581d-4229-8181-521d8d8b5011)

#### Centers are same level:
For the same level center we call define separately at sample above and the communication between these centers will be through by models.

Example:

![image](https://github.com/arrowltd/docs/assets/17697751/cfb59d2e-f149-4d0b-a0ec-85fc8a688aa0)

### Query in center

Each table query must be executed at the relevant center.

Example: table role have to query in role center only.

## Display for FE

If we have an struct with mulitple attributes inside and we do not need to display all of these attributes at FE, we can create another struct with the related attributes to display at FE.

Example: we have a struct like this, it has multiple attributes inside and we no need to display all of them.

![Screen Shot 2023-11-30 at 15 03 40](https://github.com/arrowltd/docs/assets/17697751/0810d039-f508-4376-921c-6e1de981e664)

Create an another struct (format: {main struct name}DisplayForFE) and define the attributes need to show.
![Screen Shot 2023-11-30 at 15 07 10](https://github.com/arrowltd/docs/assets/17697751/36cff644-8fd9-431c-9a74-ee55ea1caa3a)

And the displayForFE struct base on the attributes from main struct.

![Screen Shot 2023-11-30 at 15 08 14](https://github.com/arrowltd/docs/assets/17697751/cd9dd151-8a29-447b-9e8a-1df8e3d3ec4e)








