
# VerificationExchangeRequest


## Properties

Name | Type
------------ | -------------
`verificationToken` | string

## Example

```typescript
import type { VerificationExchangeRequest } from '@otp.com/sdk-node'

// TODO: Update the object below with actual values
const example = {
  "verificationToken": otp_vt_3xZ9kQ2m7pLw42mN8kaB3xZ9kQ2m7pLw,
} satisfies VerificationExchangeRequest

console.log(example)

// Convert the instance to a JSON string
const exampleJSON: string = JSON.stringify(example)
console.log(exampleJSON)

// Parse the JSON string back to an object
const exampleParsed = JSON.parse(exampleJSON) as VerificationExchangeRequest
console.log(exampleParsed)
```

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)


