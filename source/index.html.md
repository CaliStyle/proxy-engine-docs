---
title: Proxy Engine Documentation

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='https://github.com/CaliStyle/trailpack-proxy-engine'>Proxy Engine</a>

includes:
  - errors

search: true
---

# Introduction
Welcome to the Proxy Engine Docs! Proxy Engine is an Open Source Node.js Framework created by the team at [Cali Style Technologies](https://cali-style.com).

## What is Proxy Engine?
Proxy Engine is a modern backend scaffold for node applications built on the flexibility and speed of [trails.js](http://trailsjs.io) which means extending it is easy and intuitive.

Proxy Engine acts as an event management engine for a highly scalable and expandable architecture. It's "event protocol" allows for "plugins" to be easily implemented or ripped out to lighten the burden on development costs without sacrificing ease of use and scalability.

## 4 Core Features
The goals of Proxy Engine is to be a free form scaffold that focuses on modern enterprise grade applications and testability. Each of these Core Features can be managed via a Worker Profile, allowing you to easily create micro services, or worker instances of your app. 

Proxy Engine marshals 4 Core Features:

### Events 
Events are either emitted or consumed, if an event handler fails, it can retired based on "retry rules". In short:
- Publish
- Subscribe
- Retry 

A single Published Event can be consumed by an infinite amount of Subscribers. Subscribers that fail can have an infinite amount of Retries. This can be handy for fixing a bug post morten or if a 3rd party service is unavailable.  Events are optionally stored in a database as a "Recorded History". Any event that fails is automatically stored, as well as any retry attempts.

### Tasks
Tasks are long running scripts that are processed in a que. For example: processing a video or crunching a large data set. Tasks can publish events, but not consume them. However, an event can trigger the creation of a new task.

### Timed Jobs (Cron Jobs)
Timed Jobs are scrips that process at specific times.  For example, once an hour, every day at noon, every 3rd wednesday of a month, etc. Timed Jobs can publish events, but not consume them. However, an event can enable or disable a Timed Job. Under the hood, Proxy Engine uses [Node Schedule](https://www.npmjs.com/package/node-schedule) to define when the jobs will be run.

### Error Management
Error Management is one of the most important things to remember in development, especially for Node.js applications. Under the hood, Proxy Engine uses [Boom](https://www.npmjs.com/package/boom) to define errors throughout the APIs.

# Plugins
Plugins are actually Trails.js "Trailpacks" that are built specifically for Proxy Engine.  However, since a Proxy Engine app is at it's core a Trails App, an extensive amount of trailpacks can easily be used.

Currently in the Proxy Engine ecosystem and actively maintained by Cali Style: 

<aside class="success">
*WIP is a Work In Progress
</aside>

- [Proxy-Sequelize](https://github.com/calistyle/trailpack-proxy-sequelize) The default ORM for all Proxy Engine Apps. 
- [Proxy-Router](https://github.com/calistyle/trailpack-proxy-router) The Content Router with built in AAA Testing (WIP)
- [Proxy-Cart](https://github.com/calistyle/trailpack-proxy-cart) A robust eCommerce Backend
  * [Proxy-Cart-Countries](https://github.com/calistyle/trailpack-proxy-cart-countries) Default Tax Rate Provider and Shipping Zones validator.
- [Proxy-Email](https://github.com/calistyle/trailpack-proxy-email) A robust Email Management System.
- [Proxy-Notifications](https://github.com/calistyle/trailpack-proxy-notifications) A robust Notifications System that works with Proxy Email.
- [Proxy-CMS](https://github.com/calistyle/trailpack-proxy-cms) A robust Content Management System (WIP)
- [Proxy-Analytics](https://github.com/calistyle/trailpack-proxy-analytics) A robust Analytics System (WIP)
- [Proxy-Sitemap](https://github.com/calistyle/trailpack-proxy-sitemap) A robust Sitemap Builder (WIP)
- [Proxy-Social](https://github.com/calistyle/trailpack-proxy-social) A robust Social network (WIP)
- [Proxy-Permissions](https://github.com/calistyle/trailpack-proxy-permissions) A robust ERP level Permissions Systems and Access Control List (ACL)
- [Proxy-Generics](https://github.com/calistyle/trailpack-proxy-generics) An adapter protocol for common functions
  * [Stripe.com Payment Processor](https://github.com/CaliStyle/proxy-generics-stripe)
  * [Authorize.net Payment Processor](https://github.com/CaliStyle/proxy-generics-authorize.net) (WIP)
  * [GoShippo.com Shipping/Fulfillment Processor](https://github.com/CaliStyle/proxy-generics-shippo) (WIP)
  * [Shipstation.com Shipping/Fulfillment Processor](https://github.com/CaliStyle/proxy-generics-shipstation) (WIP)
  * [Taxjar.com Tax Processor](https://github.com/CaliStyle/proxy-generics-taxjar) (WIP)
  * [Mandrill Email Provider](https://github.com/CaliStyle/proxy-generics-mandrill)
  * [Gcloud Data Store Provider](https://github.com/CaliStyle/proxy-generics-gcloud) (WIP)
  * [Google Maps Geolocation Provider](https://github.com/calistyle/proxy-generics-google-maps) (WIP)
  * [Cloudinary Image Provider](https://github.com/calistyle/proxy-generics-cloudinary) (WIP)
  * [Render Service](https://github.com/calistyle/proxy-generics-render) for rendering HTML or Markdown

# Example App
We created a boiler plate to get you started
[Proxy Engine Example](https://github.com/CaliStyle/proxy-engine-example)

# Dependencies
## Supported ORMs
| Repo          |  Build Status (edge)                  |
|---------------|---------------------------------------|
| [trailpack-proxy-sequelize](https://github.com/trailsjs/trailpack-proxy-sequelize) | [![Build status][ci-sequelize-image]][ci-sequelize-url] |

## Supported Webserver
| Repo          |  Build Status (edge)                  |
|---------------|---------------------------------------|
| [trailpack-express](https://github.com/trailsjs/trailpack-express) | [![Build status][ci-express-image]][ci-express-url] |

# Installation
To install Proxy Engine:

<code>
$ npm install trailpack-proxy-engine
</code>

If you are using Trail's yo generator:

<code>
$ yo trails:trailpack trailpack-proxy-engine
</code>

# Configuration
To configure Proxy Engine is simple, there are only 2 files to create/modify:

## config/main.js
> config/main.js

```javascript
module.exports = {
  packs: [
    // ... other trailpacks
    require('trailpack-proxy-engine')
    // ... other proxy-packs
  ]
}
```
Like all Trails' trailpacks, it must be included in the `config/main.js` file.

## config/proxyEngine.js
> config/proxyEngine.js

```javascript
module.exports = {
  // If the app is in live mode
  live_mode: true,
  // If every event should be saved automatically
  auto_save: false,
  // The worker env profile for this app
  profile: 'testProfile',
  // Configure Cron Jobs
  crons_config: {
    // Delay when cron jobs are allowed to start being processed in seconds
    uptime_delay: 180,
    // The profiles that are able to run specified crons
    profiles: {
      // If the worker matches `testProfile`, then it's crons can run
      testProfile:  ['onTestCron.test'],
      // If the worker matches `otherProfile`, then it's crons can run
      otherProfile: ['otherTestCron.test']
    }
  },
  // Configure Events
  events_config: {
    profiles: {
      // If the worker matches `testProfile`, then it's events can run
      testProfile:  ['onTestEvent.test'],
      // If the worker matches `otherProfile`, then it's events can run
      otherProfile: ['otherTestEvent.test']
    }
  },
  // Configure Tasks
  tasks_config: {
    // The profiles that are able to run specified tasks
    profiles: {
      // If the worker matches `testProfile`, then it's tasks can run
      testProfile: ['TestTask'],
      // If the worker matches `otherProfile`, then it's tasks can run
      otherProfile: ['OtherTestTask']
    }
  }
}
```
Also, Proxy Engine takes it's own config in the `config/proxyEngine.js` file.


### Config Parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
live_mode | Boolean | false | If the app is in live mode
auto_save | Boolean | false | If every Event should be saved
profile | String | null | The name of this App Instance's profile
crons_config | Object | {} | The configuration of the Timed Jobs (Cron Jobs)
events_config | Object | {} | The configuration of the Events
tasks_config | Object | {} | The configuration of Tasks.

# Tutorial: Event
Proxy Engine events are contained into __two__ base categories: Instance, Global.

Events should always return a promise and resolve (succeed) and return a value or they should reject (throw an error) which will trigger a retry on the burn down schedule.

## Instance Events
Instance Events are events that __only a single instance__ in a cluster must deal with. The overwhelming majority of events fall into this category. Mostly, database events, service events etc.

## Global Events (TODO)
Global Events are events that __every instance__ in a cluster must respond to. Very few events fall into this category. Mostly, global settings changes, plugin updates, notifications, etc. This requires redis.

Events make it easy to extend functionality without having to edit or change the core of any Proxy Engine Module.

## Subscribing to Events
Subscribe takes three arguments:

Argument | Type | Default | Description
-------- | ---- | ------- | -----------
callAgainLocation | String | NONE | The location of the Callback function in dot notation eg. `onCustomer.created`. This is used when the first try of the event fails.
eventName | String | NONE | The name of the event to subscribe to eg. `customer.created`
func | Function | NONE | The function to execute when this event happens.

<aside class="success">
NOTE: Proxy Engine will automatically run the subscribe method for your events if you provide a `subscribe` method in your event handler.
</aside>

## Automatically Subscribing From Profile
> Example Profile Config: app/config/proxyEngine.js

```javascript
module.exports = {
  // ... other configuration
  profile: 'testProfile',
  events_config: {
    profiles: {
      testProfile: [
        'onTestEvent.test3',
        'onTestEvent.test4'
      ],
      otherProfile: [
        'onTestEvent.test'
      ]
    }
  }
}
```
Alternatively, you can let your profile decide what events it will subscribe to:

## Subscribing from a Service
> Subscribe to an Event from a Service

```javascript
// From some service in your app
const ProxyEngineService = this.app.services.ProxyEngineService
// Create a token that you can use later.
const token = ProxyEngineService.subscribe('callAgainLocation', 'eventName', callback)
```
Also Alternatively, you can subscribe to an event in a service.


## Unsubscribe from an Event

> Unsubscribe to an Event from a Service

```javascript
// continued from above
ProxyEngineService.unsubscribe(token)
```
Once subscribed, to unsubscribe just call the unsubscribe method using the token it was created with.

## Creating Event functions
Create events in the `/api/events` directory.

#### Subscribe
The `subscribe()` method method has reserved functionality and is intended to automatically subscribe instances regardless of their worker profile.  It's possible to have an instance level cron job gather information from a remote site and change the instance by publishing and event that it is automatically subscribed to. 

> Example: api/events/onTestEvent.js

```javascript
const Event = require('trailpack-proxy-engine').Event
module.exports = class onTestEvent extends Event {
  // Subscribe Method is called during the Configuration of the trailpack, regardless of worker profile.
  subscribe() {
    this.app.services.ProxyEngineService.subscribe('onTestEvent.test','test', this.test)
    this.app.services.ProxyEngineService.subscribe('onTestEvent.test2','test2', this.test2)
  }

  test(msg, data, options) {
    // This function will be run when a `test` event is published
    return Promise.resolve()
  }
  test2(msg, data, options) {
    // This function will be run when a `test2` event is published
    return Promise.reject()
  }
}
```

# Tutorial: Task
While event functions respond to events, tasks initiate functions allowed on a specific worker. A common use of a task is a micro-service or a worker environment for example, processing video or any other significant process that should be segregated out to a more adapt worker environment.

## Creating Task functions
Create tasks in the `/api/tasks` directory. 

```javascript
TODO
```

# Tutorial: Timed Job (Cron) 

## Creating a Cron
Create cron jobs in the `/api/crons` directory. Cron Jobs use [node-schedule](https://www.npmjs.com/package/node-schedule) and are configured per worker profile.  

Using the `uptime_delay` in the `crons_config` can also allow you to delay when cron jobs start to be scheduled. This is useful for when you have an app instance that starts while another is gracefully being shut down.

### Schedule Method
> Example: api/crons/onTestCron.js

```javascript
module.exports = class onTestCron extends Cron {
  // Schedule Method is called during the Configuration of the trailpack
  // Schedule method is reserved to automatically do something regardless of profile.
  schedule() {
    this.test()
  }
  test() {
    console.log('I have been scheduled')
    const j = this.scheduler.scheduleJob('42 * * * *', () => {
      console.log('The answer to life, the universe, and everything!')
    })
  }
}
```
The `schedule()` method has reserved functionality and is intended to automatically schedule other tasks regardless of worker profile.  This is useful for cron jobs that do instance level maintenance. It is not recommended to be used for cron jobs that perform global operations.

<aside class="success">
NOTE: If a Cron fails, it is not automatically retried. If you want parts of your cron job to retry automatically, you will need publish them as an event and consume them as a subscriber.
</aside>

# API: Endpoints
## 
```json
[{
    method: ['GET'],
    path: '/events',
    handler: 'EventController.findAll',
    config: {
      validate: {
        query: {
          offset: joi.number(),
          limit: joi.number(),
          where: joi.object(),
          sort: joi.array().items(joi.array()),
        }
      },
      app: {
        proxyRouter: {
          ignore: true
        },
        proxyPermissions: {
          resource_name: 'apiGetEventsRoute',
          roles: ['admin']
        }
      }
    }
},
{
    method: ['POST'],
    path: '/event',
    handler: 'EventController.create',
    config: {
      app: {
        proxyRouter: {
          ignore: true
        },
        proxyPermissions: {
          resource_name: 'apiPostEventRoute',
          roles: ['admin']
        }
      }
    }
},
{
    method: ['GET'],
    path: '/event/:id',
    handler: 'EventController.findOne',
    config: {
      validate: {
        params: {
          id: joi.string().required()
        }
      },
      app: {
        proxyRouter: {
          ignore: true
        },
        proxyPermissions: {
          resource_name: 'apiGetEventIdRoute',
          roles: ['admin']
        }
      }
    }
}]
```

# API: Controllers

## EventController

# API: Crons
> Example api/crons/index.js

# API: Events
> Example api/events/index.js

```javascript
exports.myEvent = require('./myEvent')
```
Create new Event handlers in the `api/events` directory

# API: Models

## Event
Attribute | Type | Default | Description
--------- | ---- | ------- | -----------


## EventSubscribers
Attribute | Type | Default | Description
--------- | ---- | ------- | -----------

# API: Services
## ProxyEngineService

# API: Tasks


[ci-sequelize-image]: https://img.shields.io/travis/trailsjs/trailpack-proxy-sequelize/master.svg?style=flat-square
[ci-sequelize-url]: https://travis-ci.org/trailsjs/trailpack-proxy-sequelize

[ci-express-image]: https://img.shields.io/travis/trailsjs/trailpack-express/master.svg?style=flat-square
[ci-express-url]: https://travis-ci.org/trailsjs/trailpack-express
