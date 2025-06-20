# cachet-api fork

## Why?

The docker image provided by cachet has a nginx with cache set up
in front of it. We want to get incidents in "realtime", so we don't
want cached results.

The PR adding this feature was not accepted by the author of the
original lib (and that's ok), so we forked it.

--------

> Original readme:

# cachet-api
[![npm version](https://badge.fury.io/js/cachet-api.svg)](https://www.npmjs.com/package/cachet-api)

A Node.js API client for [Cachet](https://cachethq.io/).

> [Cachet](https://cachethq.io/) is a beautiful and powerful open source status page system, a free replacement to services such as StatusPage.io, Status.io and others.

## Usage

First, install the package using npm:

```shell
npm install cachet-api --save
```

Then, start using the package by importing and configuring it:

```js
var CachetAPI = require('cachet-api');

// Fill in the parameters accordingly
var cachet = new CachetAPI({
    // Base URL of your installed Cachet status page
    url: 'https://demo.cachethq.io',
    // Cachet API key (provided within the admin dashboard)
    apiKey: '***REMOVED***'
});
```

Make sure to fill in your Cachet status page `url` as well as your Cachet admin account's `apiKey`, which you can find in the [Cachet dashboard](https://docs.cachethq.io/docs/api-authentication#api-token).

## Get Component Info

Use `cachet.getComponentById(id)` to fetch details about an existing component:

```js
// Prepare a component ID to fetch
var componentId = 1;

// Get component info by ID
cachet.getComponentById(componentId)
    .then(function (component) {
        // Log component info
        console.log('Component', component);
    }).catch(function (err) {
        // Log errors to console
        console.log('Fatal Error', err);
    });
```

## Publish a Metric Point

Use `cachet.publishMetricPoint(point)` to publish a new metric point to an existing metric:

```js
// Prepare a metric point to publish (so it shows up on the metric's graph)
var metricPoint = {
    // Metric ID
    id: 1,
    // Metric point value
    value: 3.37,
    // Metric point timestamp (optional, defaults to now)
    timestamp: Math.round(new Date().getTime() / 1000)
};

// Publish it so it shows up on the status page
cachet.publishMetricPoint(metricPoint)
    .then(function (response) {
        // Log API response
        console.log('Metric point published at ' + response.data.created_at);
    }).catch(function (err) {
        // Log errors to console
        console.log('Fatal Error', err);
    });
```

## Report an Incident

Use `cachet.reportIncident(incident)` to report a new status incident:

```js
// Prepare an incident to report
var incident = {
    // Incident name
    name: 'Database connectivity issues',
    // Incident description (supports markdown)
    message: 'We\'re investigating connectivity issues with the main DB.',
    // Incident status (https://docs.cachethq.io/docs/incident-statuses)
    status: 'Investigating',
    // Whether the incident will be visible to the public or only to logged in users
    visible: true,
    // Whether to send out e-mail notifications to subscribers regarding this incident
    notify: true,
    // Component ID affected by this incident (optional)
    component_id: 1,
    // Component status (required if component_id is specified) (https://docs.cachethq.io/docs/component-statuses)
    component_status: 'Partial Outage'
};

// Report it so it shows up on the status page
cachet.reportIncident(incident)
    .then(function (response) {
        // Log API response
        console.log('New incident reported at ' + response.data.created_at);
    }).catch(function (err) {
        // Log errors to console
        console.log('Fatal Error', err);
    });
```

## License

Apache 2.0
