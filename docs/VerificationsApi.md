# VerificationsApi

All URIs are relative to *https://api.otp.com*

| Method | HTTP request | Description |
|------------- | ------------- | -------------|
| [**exchangeVerification**](VerificationsApi.md#exchangeverification) | **POST** /api/v1/verifications/exchange | Exchange a verification token for the recipient it proves. |



## exchangeVerification

> VerificationExchangeResponse exchangeVerification(verificationExchangeRequest)

Exchange a verification token for the recipient it proves.

Call this from YOUR backend, with a server key from your API Keys page, using the verification_token your app received from POST /client/otp/verify. It returns the recipient that was actually verified. This is the only trustworthy answer to \&quot;did this user prove they control this number\&quot;: the &#x60;matched&#x60; field the device saw is a UI hint, read off a device you do not control, and an app can claim anything. Exchanging is idempotent for the same API key within the token lifetime, so a retry after a network failure returns the same result instead of losing the verification. Any other key, a second use, or an expired token gets a 404.

### Example

```ts
import {
  Configuration,
  VerificationsApi,
} from '@otp.com/sdk-node';
import type { ExchangeVerificationRequest } from '@otp.com/sdk-node';

async function example() {
  console.log("🚀 Testing @otp.com/sdk-node SDK...");
  const config = new Configuration({ 
    // Configure HTTP bearer authorization: bearerAuth
    accessToken: "YOUR BEARER TOKEN",
  });
  const api = new VerificationsApi(config);

  const body = {
    // VerificationExchangeRequest
    verificationExchangeRequest: ...,
  } satisfies ExchangeVerificationRequest;

  try {
    const data = await api.exchangeVerification(body);
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

// Run the test
example().catch(console.error);
```

### Parameters


| Name | Type | Description  | Notes |
|------------- | ------------- | ------------- | -------------|
| **verificationExchangeRequest** | [VerificationExchangeRequest](VerificationExchangeRequest.md) |  | |

### Return type

[**VerificationExchangeResponse**](VerificationExchangeResponse.md)

### Authorization

[bearerAuth](../README.md#bearerAuth)

### HTTP request headers

- **Content-Type**: `application/json`
- **Accept**: `application/json`


### HTTP response details
| Status code | Description | Response headers |
|-------------|-------------|------------------|
| **200** | The verification this token proves. |  -  |
| **401** | Missing or invalid API key (also returned for a publishable key used here, a disabled app, or a suspended company). |  -  |
| **404** | Verification token not found, expired, or already exchanged. The same 404 covers a token that is not yours and one already exchanged by a different key of your own, so the endpoint cannot be used to probe which tokens exist. |  -  |
| **422** | Request body failed validation. |  -  |
| **503** | The platform is closed for maintenance. Retry after the interval in the Retry-After header. |  -  |

[[Back to top]](#) [[Back to API list]](../README.md#api-endpoints) [[Back to Model list]](../README.md#models) [[Back to README]](../README.md)

