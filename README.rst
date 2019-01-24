=================
Serverless Golang
=================

- AWS Lambda:
  - `slides in revealjs <slides/>`_
  - `pdf export of slides <slides/index.pdf>`_
- TBD more about testing
- TBB Google Cloud Functions
- TBD Kubeless

Feedback? Questions? Do not hesitate to send me an email. Helpful? Give a LIKE to `a LinkedIn post about this talk <https://www.linkedin.com/feed/update/urn:li:activity:6494326597385023488/>`_ or a `STAR <https://github.com/wojciech12/talk_serverless_in_golang>`_ to this github repo.

Golang AWS Lambda with Serverless and SAM
=========================================

Hello-worder:

::

  brew tap aws/tap
  brew install aws-sam-cli

::

  mkidr $GOPATH/github.com/wojciech12/planning-poker
  cd $GOPATH/github.com/wojciech12/planning-poker
  npm install -g serverless
  serverless create -t aws-go-dep

::

   cat > template.yml <<EOF
  AWSTemplateFormatVersion : '2010-09-09'
  Transform: AWS::Serverless-2016-10-31
  Description: A hello world application.
  Resources:
    HelloWorldFunction:
      Type: AWS::Serverless::Function
      Properties:
        Handler: bin/hello
        Runtime: go1.x
        Events:
          Vote:
            Type: Api
            Properties:
              Path: /
              Method: get
EOF


Test locally:

::

  sam local start-api
  sam local start-api --env-vars env.json
  sam local generate-event apigateway authorizer > event.json
  sam local invoke "HelloWorldFunction" -e event.json


::

  sam local generate-event apigateway authorize

see:

- https://docs.aws.amazon.com/lambda/latest/dg/kinesis-tutorial-spec.html
- https://stackoverflow.com/questions/52251075/how-to-pass-parameters-to-serverless-invoke-local


::

   curl  http://127.0.0.1:3000/hello
   curl  http://127.0.0.1:3000/hello/wojtek
   curl  http://127.0.0.1:3000/hello?name=wojtek

::

  python3 -m venv .venv
  source .venv/bin/activate
  pip install localstack

Related work
============

- https://github.com/serverless/serverless-golang
- https://hackernoon.com/lessons-learned-a-year-of-going-fully-serverless-in-production-3d7e0d72213f
- https://dev.to/yos/getting-started-with-serverless-go--1lff
- https://epsagon.com/blog/the-most-popular-deployment-tools-for-serverless/
- https://docs.aws.amazon.com/lambda/latest/dg/lambda-go-how-to-create-deployment-package.html
- https://www.alexedwards.net/blog/serverless-api-with-go-and-aws-lambda
