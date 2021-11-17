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