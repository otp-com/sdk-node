
# OtpResponse


## Properties

Name | Type
------------ | -------------
`otpId` | string
`status` | [Status](Status.md)
`channel` | [Channel](Channel.md)
`maskedRecipient` | string
`actionUrl` | string

## Example

```typescript
import type { OtpResponse } from '@otp.com/sdk-node'

// TODO: Update the object below with actual values
const example = {
  "otpId": null,
  "status": null,
  "channel": null,
  "maskedRecipient": null,
  "actionUrl": https://wa.me/13845555555?text=Verify%20me%3A%20aB3xZ-9kQ2m-7pLw4-2mN8k,
} satisfies OtpResponse

console.log(example)

// Convert the instance to a JSON string
const exampleJSON: string = JSON.stringify(example)
console.log(exampleJSON)

// Parse the JSON string back to an object
const exampleParsed = JSON.parse(exampleJSON) as OtpResponse
console.log(exampleParsed)
```

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


