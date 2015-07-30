## Appistack 

Appistack is a technology stack template for startups and API-driven applications.  It was designed with Hackathon developers in mind:
Appistack enables developers to hit the ground running with an API and single page app deployed to Heroku/Divshot in two hours or less.  
Get the CRUD out of the way and start building.

Appistack includes the following projects:
  
- [appistack/appistack-rails](http://github.com/appistack/appistack-rails): An API-only implementation of Rails with token authentication.
- [appistack/appistack-ng](http://github.com/appistack/appistack-ng): An AngularJS/Bootstrap soundboard app, with great examples for webaudio and canvas. 
- [appistack/appistack-swift](http://github.com/appistack/appistack-swift): An iOS soundboard app written in bleeding edge Swift 2.0.
- appistack/appistack-droid: An Android soundboard app (coming soon)
- appistack/appistack-ionic: An ionic implementation of the Angular app.

## Appistack Goals

#### Generic Featureset

The features are fairly generic.  That is, for the most part, Appistack only has the features that every startup application would need to begin with.
The API has token-based authentication, which is a must for Android/iOS apps.  The client apps all have user registration & login built into them.  
The Angular app already includes UI for email confirmation and password reset flows.

There are just three models in each app: Users, Artists and Sounds, which is an exception to the goal of a generic set of features.
Developers will need to rip out the code for Artist & Sounds, as their app will likely use a different set of models.  ...

#### Generic Technical Implemenation

The technical implementation is designed to be as generic as possible, so you can just fork the projects and build on top of them.
This way, you can hit the ground running instead of spending a ton of time ripping code out.  

There are a few features missing, like user profile pictures.  This is because the technical implementation for these features is 
usually specific to the technology used.  A feature like profile pictures has backend technical implementation details that are determined 
and constrained by design decisions on the frontend.  So, instead I've added Gravatar to the API, which returns a gravatar_url along with each user.  
 
#### Limited Testing Implementation

Testing is highly opinionated and each team will test using different technologies. That's why there is mostly no testing included in the projects.    
Some teams will opt for full-stack integration testing, loading separate projects in containers.  Others will 
want to mock the API responses to run acceptance tests on each client.  The Angular project has a few basic Protractor tests 
written using ngMockE2E.  The Angular project can actually run without the API server because all the API responses have been mocked.  
The Rails project has a few basic unit tests included, but the idea is that you'll be able to easily add in testing however you prefer it.

#### Generic API between Server/Clients

The API is designed so that the API or clients can easily be replaced.  You can easily run a Django or Clojure API and connect the
same Angular app to it.  Or you can build a Backbone app and easily connect it -- with no changes required to format of the API responses.  
Any API implementation will need to be configured to return authentication/registration responses in the format of DeviseTokenAuth.  NgTokenAuth is
used by the Angular app and there are non-Angular implementations for the same DeviseTokenAuth API responses.

#### Separated Concerns

In addition to being a template for startups, Appistack is a good example of the benefits of separated concerns in your architecture.  
Your API should be an great API and that's it.  It should create resources and serve data to various clients, which all expect
interactions with the API to occur in a common language.  When your API becomes concerned with solving too many problems,
your application because less technically agile.  Down the road, it becomes harder to try new technologies and scalability 
becomes a problem.  Scalability is hard enough when you're sharding your data sources and distributing your services, 
so why would you want an unnecessary caching layer to add another dimension of complexity? 
 
Great example: your API should not give a !@#$ about caching client-side HTML page fragments.  If your application server uses memcached to 
cache HTML views, have fun scaling that !@#$. It's a hard problem to solve and very expensive.  Client-side caching is not a problem your API 
should be concerned with, nor should it be concerned with managing or building your static assets!  Fortunately API-driven Single-Page Apps, 
many problems are much simpler.  If your API does actually *need* a caching layer for JSON responses, then congratulations -- you probably
make a lot more money than me.
