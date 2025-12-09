---
title: "Week 13 Worklog"
date: 2025-12-01
weight: 13
chapter: false
pre: " <b> 1.13. </b> "
---

### Week 13 Objectives:

- Integrate Backend (API Gateway).
- Deploy CloudFront + WAF and Route53 (Custom Domain).

### Tasks to Execute This Week:

| **Day** | **Task**                                                                                                                                                                                                                     | **Start**    | **Finish**     | **References**                                                                                                                                                                                                                                                                       |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 2      | - Connect API Gateway (REST).<br>- Handle CORS issues.                                                                                                                                                                        | 02/12/2025   | 02/12/2025     | [CORS in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html) <br> [Axios Documentation](https://axios-http.com/docs/intro)                                                                           |
| 3      | - Integrate Student CRUD:<br>&emsp;+ Create (Add new student)<br>&emsp;+ Read (Get list/details)<br>&emsp;+ Update (Update information)<br>&emsp;+ Delete (Remove student)<br>- Handle loading/error states on the UI.        | 03/12/2025   | 03/12/2025     | [React Query](https://tanstack.com/query/latest) <br> [Error Handling Best Practices](https://kentcdodds.com/blog/use-react-error-boundary-to-handle-errors-in-react)                                                                        |
| 4      | Deploy **CloudFront + WAF**.<br>- Create WAF Web ACL with security rules.<br>- Create CloudFront Distribution.<br>- Add API Gateway as an Origin.                                                                            | 04/12/2025   | 04/12/2025     | [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/what-is-aws-waf.html) <br> [CloudFront Distribution](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-working-with.html)                      |
| 5      | - Configure **Route53** (Custom Domain).<br>- Create DNS Records for the domain.<br>- Configure SSL Certificate with ACM (AWS Certificate Manager).                                                                           | 05/12/2025   | 05/12/2025     | [Route 53 DNS](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) <br> [ACM Certificate](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html)                                                             |
| 6      | - Refactor Frontend code.<br>- Optimize Performance (lazy loading, memoization).<br>- Write code documentation.                                                                                                                | 06/12/2025   | 06/12/2025     | [React Performance](https://react.dev/learn/render-and-commit) <br> [Code Documentation](https://jsdoc.app/)                                                                                                                                 |

### Week 13 Achievements:

**1. Successfully connected API:**

- Integrated Frontend with API Gateway built by the Backend team.
- Resolved CORS issues by working directly with the Backend team.
- Built API service with Axios, including automatic JWT Token injection.
- Implemented loading spinner and error notifications for API calls.

**2. Completed Student CRUD Interface:**

- Built add/edit student form with validation.
- Displayed student list using a table with pagination and search.
- Implemented student deletion with confirmation modal.
- Used React Query for data management and auto-refresh after mutations.

**3. Deployed CloudFront + WAF:**

- Created WAF Web ACL with essential security rules (SQL Injection, XSS, Rate Limiting).
- Created CloudFront Distribution for content delivery.
- Added API Gateway as an Origin in CloudFront.
- Configured appropriate Cache Behavior for API requests.

**4. Configured Route53 and SSL:**

- Created Hosted Zone for custom domain.
- Configured DNS Records (A Record, CNAME) pointing to CloudFront.
- Requested SSL Certificate from ACM for the domain.
- Attached certificate to the CloudFront Distribution.
- Successfully tested accessing the system via HTTPS through the custom domain.

**5. Learned:**

- How to integrate Frontend with REST API.
- Understanding CDN and how CloudFront optimizes content delivery.
- Configuring WAF to protect applications from common attacks.
- Managing DNS using Route53 and configuring SSL Certificates with ACM.
- Collaborating with Backend team to resolve CORS and test End-to-End flows.

---
