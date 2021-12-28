
## Run locally sample application

This project contains the Stock Trader sample from [AWS](https://docs.aws.amazon.com/step-functions/latest/dg/tutorial-state-machine-using-sam.html)

With this commands below you can run AWS step functions locally on docker.

To use the SAM CLI, you need the following tools:

* SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
* Node.js - [Install Node.js 14](https://nodejs.org/en/), including the NPM package management tool.
* Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

To build and run locally your application for the first time, run the following in your shell:

```bash
sam build
docker network create foo
sam local start-lambda --docker-network foo
```

Open a new Terminal and run the following:

```bash
docker run --network foo -p 8083:8083 --env-file localsettings.txt amazon/aws-stepfunctions-local
```

Open another Terminal and run the following:

```bash
aws stepfunctions create-state-machine --endpoint http://localhost:8083 --definition file://statemachine/stock_trader.asl.json --name "StockTradingStateMachine" --role-arn "arn:aws:iam::012345678901:role/DummyRole"

aws stepfunctions --endpoint http://localhost:8083 start-execution --state-machine arn:aws:states:us-east-1:123456789012:stateMachine:StockTradingStateMachine --name test

aws stepfunctions --endpoint http://localhost:8083 describe-execution --execution-arn arn:aws:states:us-east-1:123456789012:execution:StockTradingStateMachine:test
```

