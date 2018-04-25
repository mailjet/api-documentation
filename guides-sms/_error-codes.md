# Error Codes

In this section we'll include detailed descriptions of the error codes you might encounter when using the different endpoints of the SMS API.

## Send SMS Error Codes

| Error Code | Error Message                                                                  | Case Description                                                                                               | Error Related To       | Status Code |
|------------|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|------------------------|-------------|
| `sms-0001` | Insufficient funds.                                                            | There are not enough funds in the AKID's SMS wallet, or no deposit has been made in this currency.             |                        | 400         |
| `sms-0002` | Unsupported country code.                                                      | The value does not start with any of the accepted country codes.                                               | `To`                   | 400         |
| `sms-0003` | SMS per day limit reached.                                                     | Daily SMS sending limit has been reached.                                                                      |                        | 400         |
| `sms-0004` | No account.                                                                    | when no account or when account has not been set yet.                                                          |                        | 400         |
| `mj-0002`  | Malformed JSON, please review the syntax and properties types.                 | The API user has sent a JSON with an invalid syntax.                                                           |                        | 400         |
| `mj-0003`  | Missing mandatory property.                                                    | A mandatory property is missing or with null value. The functional specification defines mandatory properties. | `To`, `From` or `Text` | 400         |
| `mj-0004`  | Type mismatch. Expected type "[property]".                                     | The value type for the respective property is incorrect.                                                       | `To`, `From` or `Text` | 400         |
| `mj-0006`  | Characters limit exceeded for the property. Max allowed - [number].            | The string contains more than the maximum allowed characters.                                                  | `To`, `From` or `Text` | 400         |
| `mj-0011`  | Input payload must be less than [size]MB.                                      | The payload size is over the limit.                                                                            |                        | 400         |
| `mj-0015`  | Not authorised.                                                                | The user did not provide valid authorization credentials.                                                      |                        | 401         |
| `mj-0020`  | Characters limit below the minimum for the property. Min allowed - [number].   | The string contains less than the minimum allowed characters.                                                  | `From` or `To`         | 400         |
| `mj-0021`  | You do not have access to this resource.                                       | The AKID does not have access to the resource.                                                                 |                        | 403         |

## SMS Stats Error Codes

**For `GET /sms`:**

| Error Code   | Error Message                                                         | Case Description                                          | Error Related To                                    | Status Code |
|--------------|-----------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------------------------------|-------------|
| `mj-0004`    | Type mismatch. Expected type "[t]".                                   | The provided value type is unexpected.                    | `FromTS`, `ToTS`, `StatusCode`, `Offset` or `Limit` | 400         |
| `mj-0005`    | Value "[value]" is invalid. Allowed values are: [allowedValues].      | The provided value is not in the array of allowed values. | `StatusCode`                                        | 400         |
| `mj-0009`    | The datetime value "[date]" is not a valid RFC3339 datetime format.   | The timestamp value is not a valid RFC3339 format.        | `FromTS` or `ToTS`                                  | 400         |
| `mj-0015`    | Not authorised.                                                       | The user did not provide valid authorization credentials. |                                                     | 401         |
| `mj-0021`    | You do not have access to this resource.                              | The AKID does not have access to the resource.            |                                                     | 403         |

**For `GET /sms/count`:**

| Error Code   | Error Message                                                         | Case Description                                                              | Error Related To                 | Status Code |
|--------------|-----------------------------------------------------------------------|-------------------------------------------------------------------------------|----------------------------------|-------------|
| `mj-0004`    | Type mismatch. Expected type "[t]".                                   | The provided value type is unexpected.                                        | `FromTS`, `ToTS` or `StatusCode`   | 400         |
| `mj-0005`    | Value "[value]" is invalid. Allowed values are: [allowedValues].      | The provided value is not in the array of allowed values.                     | `StatusCode`                       | 400         |
| `mj-0009`    | The datetime value "[date]" is not a valid RFC3339 datetime format.   | The timestamp value is not a valid RFC3339 format.                            | `FromTS` or `ToTS`                 | 400         |
| `mj-0015`    | Not authorised.                                                       | The user did not provide valid authorization credentials.                     |                                    | 401         |
| `mj-0021`    | You do not have access to this resource.                              | The AKID does not have access to the resource.                                |                                    | 403         |
| `mj-0025`    | Value limit exceeded. Max allowed - [number].                         | The array property contains more than the maximum allowed number of elements. | `Limit`                            | 400         |

## SMS Stats Export Error Codes

**For `POST /sms/export`**

| Error Code   | Error Message                                                         | Case Description                                          | Error Related To   | Status Code |
|--------------|-----------------------------------------------------------------------|-----------------------------------------------------------|--------------------|-------------|
| `mj-0002`    | Malformed JSON, please review the syntax and properties types.        | The API user sends a Json with an invalid syntax.         |                    | 400         |
| `mj-0003`    | Missing mandatory property.                                           | A mandatory property is missing or with null value.       | `FromTS` or `ToTS` | 400         |
| `mj-0004`    | Type mismatch. Expected type "[t]".                                   | The provided value type is unexpected.                    | `FromTS` or `ToTS` | 400         |
| `mj-0009`    | The datetime value "[date]" is not a valid RFC3339 datetime format.   | The timestamp value is not a valid RFC3339 format.        | `FromTS` or `ToTS` | 400         |
| `mj-0015`    | Not authorised.                                                       | The user did not provide valid authorization credentials. |                    | 401         |
| `mj-0021`    | You do not have access to this resource.                              | The AKID does not have access to the resource.            |                    | 403         |

**For `GET /sms/export/{ID}`**

| Error Code   | Error Message                            | Case Description                                          | Error Related To   | Status Code |
|--------------|------------------------------------------|-----------------------------------------------------------|--------------------|-------------|
| `mj-0003`    | Missing mandatory property.              | A mandatory property is missing or with null value.       | `ID`               | 400         |
| `mj-0004`    | Type mismatch. Expected type "[t]".      | The provided value type is unexpected.                    | `ID`               | 400         |
| `mj-0015`    | Not authorised.                          | The user did not provide valid authorization credentials. |                    | 401         |
| `mj-0021`    | You do not have access to this resource. | The akid does not have access to the resource.            |                    | 403         |
