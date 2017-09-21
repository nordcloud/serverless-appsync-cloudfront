# serverless-api-cloudfront

[![serverless](http://public.serverless.com/badges/v3.svg)](http://www.serverless.com)
[![npm version](https://badge.fury.io/js/serverless-api-cloudfront.svg)](https://badge.fury.io/js/serverless-api-cloudfront)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/Droplr/serverless-api-cloudfront/master/LICENSE)
[![npm downloads](https://img.shields.io/npm/dt/serverless-api-cloudfront.svg?style=flat)](https://www.npmjs.com/package/serverless-api-cloudfront)

Automatically creates properly configured AWS CloudFront distribution that routes traffic
to API Gateway.

Due to limitations of API Gateway Custom Domains, we realized that setting self-managed CloudFront distribution is much more powerful.

**:zap: Pros**

- Allows you to set-up custom domain for your API Gateway
- Enables CDN caching of resources - so you don't waste Lambda invocations or API Gateway traffic
  for serving static files (just set proper Cache-Control in API responses)
- Much more CloudWatch statistics of API usage (like bandwidth metrics)
- Real world [access log](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html) - out of the box, API Gateway currently does not provide any kind of real "apache-like" access logs for your invocations

## Installation

```
$ npm install --save-dev serverless-api-cloudfront
```

## Configuration

* All apiCloudFront configuration parameters are optional - e.g. don't provide ACM Certificate ARN
  to use default CloudFront certificate (which works only for default cloudfront.net domain).
* This plugin **does not** set-up automatically Route53 for newly created CloudFront distribution.
  After creating CloudFront distribution, manually add Route53 ALIAS record pointing to your
  CloudFront domain name.
* First deployment may be quite long (e.g. 10 min) as Serverless is waiting for
  CloudFormation to deploy CloudFront distribution.

```
# add in your serverless.yml

plugins:
  - serverless-api-cloudfront

custom:
  apiCloudFront:
    domain: my-custom-domain.com
    certificate: arn:aws:acm:us-east-1:000000000000:certificate/00000000-1111-2222-3333-444444444444
    logging:
      bucket: my-bucket.s3.amazonaws.com
      prefix: my-prefix
```