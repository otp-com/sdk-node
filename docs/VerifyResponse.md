
# VerifyResponse


## Properties

Name | Type
------------ | -------------
`otpId` | string
`status` | [Status](Status.md)
`matched` | boolean

## Example

```typescript
import type { VerifyResponse } from '@otp.com/sdk-node'

// TODO: Update the object below with actual values
const example = {
  "otpId": null,
  "status": null,
  "matched": null,
} satisfies VerifyResponse

console.log(example)

// Convert the instance to a JSON string
const exampleJSON: string = JSON.stringify(example)
console.log(exampleJSON)

// Parse the JSON string back to an object
const exampleParsed = JSON.parse(exampleJSON) as VerifyResponse
console.log(exampleParsed)
```

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


