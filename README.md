# retroboard

<a href="http://localhost:8000/appstacks/git?repoName=retroboard"><img src="https://img.shields.io/badge/Preview%20in%20appCd-blue?style=flat&logo=data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyBpZD0iTGF5ZXJfMiIgZGF0YS1uYW1lPSJMYXllciAyIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAzODcuNSA2MDAiPgogIDxkZWZzPgogICAgPHN0eWxlPgogICAgICAuY2xzLTEgewogICAgICAgIGZpbGw6ICM3OWM4NDI7CiAgICAgIH0KCiAgICAgIC5jbHMtMSwgLmNscy0yIHsKICAgICAgICBzdHJva2Utd2lkdGg6IDBweDsKICAgICAgfQoKICAgICAgLmNscy0yIHsKICAgICAgICBmaWxsOiAjM2VhOGY0OwogICAgICB9CiAgICA8L3N0eWxlPgogIDwvZGVmcz4KICA8cGF0aCBjbGFzcz0iY2xzLTEiIGQ9Im0yNDguODUsMzEzLjJsLTQzLjA4LDQzLjA4YzMuODcsMTIuNzIuODEsMjcuMS05LjIzLDM3LjE0bC05Mi45NSw5Mi45NWMtMTAuOTYsMTAuOTYtMTEuMDEsMjguODQtLjExLDM5Ljg1LDUuMzMsNS4zOSwxMi40NCw4LjM2LDIwLjAxLDguMzhoLjA4YzcuNTUsMCwxNC42NC0yLjk0LDE5Ljk4LTguMjhsMTM5LjIzLTEzOS4yM2MxMS4wMi0xMS4wMiwxMS4wMi0yOC45NCwwLTM5Ljk2bC0zMy45My0zMy45M1oiLz4KICA8cGF0aCBjbGFzcz0iY2xzLTIiIGQ9Im0xMjQuNiw0MDAuMzdoLjA4YzcuNTUsMCwxNC42NC0yLjk0LDE5Ljk4LTguMjhsMTM5LjIzLTEzOS4yM2M1LjM0LTUuMzQsOC4yOC0xMi40Myw4LjI4LTE5Ljk4cy0yLjk0LTE0LjY0LTguMjgtMTkuOThMMTQ0LjY1LDczLjY3Yy01LjM0LTUuMzQtMTIuNDMtOC4yOC0xOS45OC04LjI4aC0uMDhjLTcuNTguMDItMTQuNjgsMy0yMC4wMSw4LjM4LTEwLjksMTEuMDItMTAuODUsMjguOS4xLDM5Ljg1bDkyLjk1LDkyLjk0YzcuMDMsNy4wMywxMC45LDE2LjM3LDEwLjksMjYuMzFzLTMuODcsMTkuMjgtMTAuOSwyNi4zMWwtOTIuOTUsOTIuOTVjLTEwLjk2LDEwLjk2LTExLjAxLDI4Ljg0LS4xLDM5Ljg1LDUuMzMsNS4zOSwxMi40NCw4LjM2LDIwLjAxLDguMzhaIi8+CiAgPHBhdGggY2xhc3M9ImNscy0xIiBkPSJtMTQzLjU1LDIwNy45Yy01LjM0LTUuMzQtMTIuNDMtOC4yOC0xOS45OC04LjI4aC0uMDhjLTcuNTguMDItMTQuNjgsMy0yMC4wMSw4LjM4LTEwLjksMTEuMDItMTAuODUsMjguOS4xMSwzOS44NWw0MC4wMyw0MC4wMywzOS45Ni0zOS45Ni00MC4wMy00MC4wM1oiLz4KPC9zdmc+&labelColor=141416&logoColor=white" alt="badge for appCD.com" height=35 /></a>


retroboard is a app written in **python** to create kanban boards that can be used for different purposes like capturing notes for a [Retrospective](https://www.atlassian.com/agile/scrum/retrospectives) meeting, creating a Pros/Cons list, tracking TODOs, etc.

This app demonstrates a serverless application with multiple functions that can be deployed to **Lambdas**, uses **DynamoDB** for data storage and **SES** for sending summary emails. The UI of the app can be served with S3.

appCD provides a powerful and flexible way to define and manage your infrastructure. We encourage you to use appCD to generate IaC for this application and try and deploy this app in your own AWS Account. 

Get started with IaC generation for this repo by following the instructions on [appCD Documentation](https://docs.appCD.io/getting-started)


### Environment Variables

- functions/email-summary
```
SES_SENDER_EMAIL_ADDRESS - email address from which the summary email will be sent
```

- functions/slack-alerts
```
SLACK_WEBHOOK_URL - slack webhook url to send alerts when a new board is created
```

### Architecture

```mermaid
graph LR
    F[Web Browser] -->|serves| G[s3-web-app]
    F[Web Browser] -->|http| A
    A[lambda-api] -->|read/write| B[dynamodb]
    A[lambda-api] --> |slack alert on new board creation| H[lambda-slack-alerts]
    A -->|queue email request| C[sqs]
    C -->|event-source-mapping| D[lambda-send-email]
    D -->|trigger summary email| E[ses]
    
```

## Other Sample Projects to try

- [hello-kitty](https://github.com/appcd-demo/hello-kitty) - a static website that can be deployed to Lambda and S3


## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
