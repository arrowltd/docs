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




