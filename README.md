# API Versioning Support in Spring Framework 7

## Overview

This sample application exemplifies an application that has endpoints with multiple endpoints available for multiple versions of the API.
This can be used to support Springdoc project's generation of OpenAPI specifications based on Spring applications that include multiple API versions. 

## Use Cases Covered

From springdoc-openapi Jira issue: https://github.com/springdoc/springdoc-openapi/issues/2975#issuecomment-2848740547

### Auto-generate multiple OpenAPI specifications

API Developer wants to auto-generate multiple versions of their OpenAPI specification from single Spring application so that they can share each for API discovery

#### Acceptance Criteria

GIVEN Version Server sample application
WHEN springdoc-openapi is executed to generate OpenAPI specification(s)
THEN three OpenAPI specifications will be generated for the following versions defined in the application:
* 1.0.0
* 1.1.0
* 1.2.0
AND `/accounts/{id}/statements` API endpoint specification for version 1.2.0 will include query param for `type`
AND `/accounts/{id}/statements` API endpoint specification in versions 1.0.0 and 1.1.0 will NOT have a query param

### Ensure API version request strategy is exposed

API Consumer wants to view which API version request strategy (i.e. Path, Header or Query Param) to use so that they can target version specific API routes

#### Acceptance Criteria

GIVEN Version Server sample application
WHEN springdoc-openapi is executed to generate OpenAPI specification(s)

AND application properties include `spring.mvc.apiversion.use.header=X-Api-Version`
THEN generated OpenAPI specification shows that specific versions of all API endpoints are accessible using HTTP Header `X-Api-Version`

AND application properties include `spring.mvc.apiversion.use.path-segment=1`
AND AccountController and StatementController endpoints are defined with `/api/{version}/...` path prefix
THEN generated OpenAPI specification shows that specific versions of all API endpoints are accessible using path `/api/{version}/**`

### Unsupported versions are shown as deprecated

API Consumer wants to view which API routes are deprecated on a specific API version so that they can change to new API calls

GIVEN Version Server sample application
WHEN springdoc-openapi is executed to generate OpenAPI specification(s)
AND application properties includes `spring.mvc.apiversion.supported=1.1.0,1.2.0`
THEN generated OpenAPI specification for version 1.0.0 will show as deprecated
AND generated OpenAPI specifications for versions 1.1.0 and 1.2.0 will NOT show any deprecation message

## Learn More 

Demo sample app and [slides](API-Versioning-SpringOne-2025.pdf) for Spencer Gibb's [API Versioning in Spring](https://event.vmware.com/flow/vmware/explore2025lv/content/page/catalog?search.product=1707435488591001gzW1&tab.sessioncatalogtabs=1747347809815001igUo&search=%22API%20Versioning%20in%20Spring%22) talk at SpringOne 2025.
