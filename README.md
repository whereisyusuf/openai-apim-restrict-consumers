# API Management Policy

This repository contains the API management policy for controlling access to OpenAI APIs. The policy is designed to prioritize certain consumers and restrict others based on the number of calls and tokens used.

## Overview

We create multiple products, each with threshold values of how many calls and tokens it can process. There are rate limits and custom policies that evaluate if the number of calls or the max tokens has reached the threshold and respond accordingly.

## Custom Policy

The custom policy checks for a context variable named `maxtokens`. If it is greater than or equal to 100, it sets a response with a response code of 429. Otherwise, it does nothing.

Here is the XML policy code:

```xml
<policies>
    <inbound>
        <choose>
            <when condition="@((int)context.Variables["maxtokens"] >= 100)">
                <return-response>
                    <set-status code="429" reason="Max tokens Reached" />
                    <set-body>@{ return "Max tokens limit reached"; }</set-body>
                </return-response>
            </when>
        </choose>
    </inbound>
</policies>
```

## Rate Limiting

Rate limiting is applied to control the number of API calls a consumer can make in a specific time period. If a consumer exceeds the rate limit, they will receive a response with a status code of 429 (Too Many Requests).

## Prioritizing Consumers

We prioritize certain consumers by assigning them to products with higher threshold values. This allows them to make more API calls and use more tokens than other consumers.

## Restricting Consumers

We restrict certain consumers by assigning them to products with lower threshold values. If they exceed these thresholds, they will receive a response with a status code of 429 (Too Many Requests), depending on whether they exceeded the token limit or the rate limit.

## Getting Started

To use this policy, you need to have Azure API Management service. Follow the [official documentation](https://docs.microsoft.com/en-us/azure/api-management/) to get started with Azure API Management.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License

[MIT](https://choosealicense.com/licenses/mit/)
