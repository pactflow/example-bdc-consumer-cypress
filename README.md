# Example Cypress Consumer

This is an example of a React consumer using Cypress to demonstrate the bi-directional contract testing capability of [Pactflow](https://pactflow.io). 

It implements a "Product" website. You can see the [Provider](https://github.com/pactflow/example-pactflow-example-provider-dredd) counterpart (see below for other compatible example providers).

It is using a public tenant on Pactflow, which you can access [here](https://test.pactflow.io) using the credentials `dXfltyFMgNOFZAxr8io9wJ37iUpY42M`/`O5AIZWxelWbLvqMd8PkAVycBJh2Psyg1`.

Unlike with Pact, the provider does not need to replay any tests to ensure it remains compatible with its consumers. This is achieved with a call to "can-i-deploy", which performs a cross-contract validation, ensuring any consumer consumes a valid subset of the OAS for the provider.

![Consumer Test](docs/consumer-scope.png 'Consumer Test')

When you run the CI pipeline (see below for doing this), the pipeline should perform the following activities (simplified):

![Consumer Pipeline](docs/consumer-pipeline.png 'Consumer Pipeline')

**Note:** Check out branch [use-pact-cypress-adaptor](https://github.com/pactflow/example-bdc-consumer-cypress/tree/use-pact-cypress-adaptor) to leverage [pact-cypress-adaptor package](https://www.npmjs.com/package/@pactflow/pact-cypress-adaptor) for bi-directional contract testing.

### Pre-requisites

**Software**:

- Tools listed at: https://docs.pactflow.io/docs/workshops/ci-cd/set-up-ci/prerequisites/
- A pactflow.io account with an valid [API token](https://docs.pactflow.io/docs/getting-started/#configuring-your-api-token)

#### Environment variables

To be able to run some of the commands locally, you will need to export the following environment variables into your shell:

- `PACT_BROKER_TOKEN`: a valid [API token](https://docs.pactflow.io/docs/getting-started/#configuring-your-api-token) for Pactflow
- `PACT_BROKER_BASE_URL`: a fully qualified domain name with protocol to your pact broker e.g. https://testdemo.pactflow.io
- `PACT_PROVIDER=pactflow-example-provider-dredd`: this changes the default provider to the Dredd based provider (https://github.com/pactflow/example-provider-dredd)
- `PACT_PROVIDER=pactflow-example-provider-postman`: ... Postman (https://github.com/pactflow/example-provider-postman)
- `PACT_PROVIDER=pactflow-example-provider-restassured`: ... Rest Assured (https://github.com/pactflow/example-provider-restassured)

### Usage

#### Use case with Cypress

NOTE: Cypress tests are located in `./cypress/integration`. See below for how to start cypress test, generate consumer contract and publish contract to pactflow.

- Spin up the ui project by running `npm run start`

then

- `make test_cypress` - run cypress test locally
- `make fake_ci_cypress` - emulate the CI process locally

_How to use Cypress_

- Spin up the ui project by running `npm run start`
- Define your pact provider and consumer name at `cypress.json` as cypress environment variables
- You can stub your network request and response with `cy.intercept`, and record network call to a consumer driven contract with `cy.usePactWait`. Each request you want to add to the contract must call this method.
- `npm run cypress:headless:chrome` - this will run cypress e2e test in headless mode, and write stubbed network calls a pact file
- `npm run cypress:run` - this will run cypress e2e test with browser ui

### FAQ


#### 1. Do this work with `cy.route`?

TBC

#### 2. Can I use this with a standard Pact verification?

No, you should not do this. This example doesn't support matching rules (meaning tests will be brittle), and the lack of specificity and control over the various testing states will lead to a lot of [pain](https://pactflow.io/blog/a-disastrous-tale-of-ui-testing-with-pact/).

See https://github.com/pactflow/example-bdc-consumer-cypress for an example repository that shows how to use Pact+Cypress effectively.

