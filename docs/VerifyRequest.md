
# VerifyRequest


## Properties

Name | Type
------------ | -------------
`otpId` | string
`code` | string

## Example

```typescript
import type { VerifyRequest } from '@otp.com/sdk-node'

// TODO: Update the object below with actual values
const example = {
  "otpId": null,
  "code": null,
} satisfies VerifyRequest

console.log(example)

// Convert the instance to a JSON string
const exampleJSON: string = JSON.stringify(example)
console.log(exampleJSON)

// Parse the JSON string back to an object
const exampleParsed = JSON.parse(exampleJSON) as VerifyRequest
console.log(exampleParsed)
```

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


