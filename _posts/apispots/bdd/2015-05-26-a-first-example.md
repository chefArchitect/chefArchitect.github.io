---
layout: project.bdd
title: A first example - BDD Testing
headline: A First Example
description: Learn how to use the service following a step-by-step example.
excerpt: "API Testing for Humans"
search_omit: true
categories: apispots/bdd
tags: [apispots,bdd,testing]
---

# Testing an API
{:class="page-header"}

In this section you will how to start using the **BDD API Testing** service for running tests against a RESTful API.  
{:class="lead"}

For this first example, we will be using the [**OpenWeatherMap** API](http://openweathermap.org/api){:target='_blank'} that is an open weather forecast service.

## Create the test story file

The first thing we need to do for starting off with our tests, is to create a file that will contain the basic test scenario that we will try out. So, let's create a new empty file and give it the name **first-one.feature**.  Notice the *.feature* suffix - this is a convention used by the *Cucumber* framework and it's good to stick with it for convenience.  If you recall in BDD, test stories usually refer to single features and are meant to be autonomous. 

<div class="bs-callout bs-callout-info">
    <h4>Heads up!</h4>
    <p>For authoring your test files using the Gherkin language, you can use any type of editor you want.  The good news is that you can find a Gherkin syntax plugin for many of the popular IDEs and editors.  <a href='https://github.com/cucumber/gherkin/wiki/Tool-Support' target='_blank'>Have a look at this list</a>.</p>
</div>


## Add a test feature 

For a full reference on the Gherkin syntax, please read the [reference manual](https://cucumber.io/docs/reference){:target='_blank'} on the web site of the Cucumber framework.
{:class="lead"}

In the new file we have just created, the first thing to do is add the title and description of the API feature we want to test.  Every RESTful API is supposed to provide at least an **endpoint** and an **operation** that maps to an HTTP method (GET, POST, DELETE, etc).

Let's suppose we are prospect clients of this API and we want to make sure that it does what it says it does.  In this scenario, we can split the API into virtual functional segments that we can map out to individual test features.  As the matter of fact the documentation from their web site has already done this for as as shown in the following picture:

![OpenWeatherMap API]({{site.url}}/assets/apispots/bdd/openweathermap-api.png "OpenWeatherMap API"){:class='img-thumbnail'}

The API structure is pretty much self explanatory, so let's give it a spin by testing the **Current weather data** feature.    

<div class="bs-example">
   Copy and paste the following Gherkin snippet into the feature file.  This section is the feature title and a short description on what this 
   test is all about. 
</div>
<div class="highlight"><pre><code class="gherkin" data-lang="gherkin_en">
Feature: OpenWeatherMap API - Current weather data
  
  As a client of the OpenWeatherMap API
  I want to run tests
  In order to validate 'Current weather data' operations
</code></pre></div>

<div class="bs-callout bs-callout-warning">
    <h4>Heads up!</h4>
    <p><b>Feature:</b> is a reserved keyword, whereas everything else is just comments - pay attention to the spaces and tabs.  Make sure
   you read the Gherkin primer first or use a Gherkin editor just to be sure you're on the right track.</p>
</div>

## Let's add some background

Now that we know the target API and the feature we want to test, it's time to let the service know about the API too.  Since we do not have any other meta-data info on this API (e.g. *Swagger* definition) the only thing we know is its **base URL**.  From the API documentation this would be:

> http://api.openweathermap.org/data/2.5

Note that this is a versioned API and how this reflects to the URL (the */2.5* part that is).  Our service needs to know about this, so let's add another section in our test file:

<div class="bs-example">
The background section is an important part of the service - in Gherkin a <b>Background</b> section is executed prior to any of the test scenarios to follow and is a convenient way of re-using a number of steps that are required to be executed before hand prior to any other step within a test feature file.  
</div>
<div class="highlight"><pre><code class="gherkin" data-lang="gherkin_en">
  Background: 
    Given a "REST" API definition at "http://api.openweathermap.org/data/2.5"
</code></pre></div>

What we have done here is to tell the service that we will be testing a **generic REST** API with base definition at URL *http://api.openweathermap.org/data/2.5*.  As you will see next, all endpoint operations will reuse this base URL information in a *relative path* mode.  

<div class="bs-callout bs-callout-info">
    <h4>Recipe</h4>
    <p>This <b>Given</b> statement is provided by the service as is and the only thing you need to do is to specify the API definition type and URL.</p>
    <blockquote>Given a "REST | Swagger | ...]" API definition at "[base or definition URL]"</blockquote>
</div>

## Let's write a first scenario

Now that we have defined the API to be tested, it's time to write our first scenario.  According to the API's documentation, one thing that we can do with the API is to get weather data for a single location.  So this is what we will be testing.

From the documentation we can see that the current weather feature is provided through the **/weather** endpoint and to get the weather data for a specific city we have to call the **GET** method passing a query string with the city name and optionally the country code as the request params.

<div class="bs-example">
Copy and paste the following scenario in the feature file.      
</div>
<div class="highlight"><pre><code class="gherkin" data-lang="gherkin_en">
  Scenario: Call current weather data for one location and by city name
  
    Given endpoint "/weather" and method "get"
      And request query param "q" equals "London,UK"
      And request type "application/json"
    When the request is executed
    Then response status is "ok"
</code></pre></div>

Let's break down the snippet to see what we have stated.  Each line in this snippet is called a **test step** in BDD terminology and defines an action the system has to take in order to execute the test.  Test steps are executed in turn and in the sequence they are defined. All steps are templated and provided by our service - you can think of them as testing ingredients.  You can potentially mix-n-match these step templates in order to compose your tests. 

### Given

The **Given** section provides the contextual information required in order for this test to be executed.  It is a set of steps that the service understands and are templated, i.e. there is a standard syntax for doing certain things and most of these templates have variable placeholders to be filled-in.

In this example the first statement tells the service we will be executing a request at the **/weather** endpoint of the API and we will be calling the **GET** method.  Since we have provided the **Background** info on the previous step there is no need to specify the rest of the API URL details.  

The second line defines an additional step within the *Given* context (**And**) and tells the system that the request should carry a **query parameter** which is called **q** and should have the value **London,UK**.

The third line tells the service that the request type should be **application/json**.  This will eventually set behind the scenes the **Content-Type** and **Accepts** headers of the request to the corresponding value.

### When

The **When** section defines the actual test action to be performed - in this service there is only one type of action to be performed and that is a request execution.

### Then

The **Then** section defines assertions for validating the results of the operation that was executed.  The most basic type of validation we can possibly do at this level is to check that the response status is **ok**.  

## Time to run the test

Now that we have created our very basic test scenario, it's time to run it through the service and see what happens.  Since the service provides it's own RESTful API there are two ways to do it.

### Use Swagger Explorer

You can interact with the service directly through the Swagger Explorer application running at 

> [http://localhost:3000/docs](http://localhost:3000/docs){:target='_blank'}

Click on the **/tests** endpoint and open the **POST** method.  There is only one required parameter you should fill in and that is the location of the test file we want to run.  Pick the location where your file has been created.

Now press the **Try it out** button and if all went well the service should return a **200OK** response and a body like the following:

<div class="highlight"><pre><code class="language-html" data-lang="html">
Feature: OpenWeatherMap API - Current weather data

  As a client of the OpenWeatherMap API
  I want to run tests
  In order to validate 'Current weather data' operations


  Scenario: Call current weather data for one location and by city name      
    Given a "REST" API definition at "http://api.openweathermap.org/data/2.5"
    Given endpoint "/weather" and method "get"
    And request query param "q" equals "London,UK"
    And request type "application/json"
    When the request is executed
    Then response status is "ok"

1 scenario (1 passed)
6 steps (6 passed)
</code></pre></div>  

<div class="bs-callout bs-callout-warning">
    <h4>Strange characters in the response</h4>
    <p>Don't freak out! The service is returning the console output from the Cucumber runtime and it contains ANSI colors in the stream.</p>
</div>

### Use any other REST client tool

You can use any other REST client tool you wish, e.g. [**curl**](http://curl.haxx.se/){:target='_blank'}.  Good thing about using *curl* is that you will get to see the test results in ANSI colors on the terminal, which is much simpler to read.  For example:

<div class="bs-example">
<span class="gp">$ </span> curl -i -F file=@/home/chris/tests/openweathermap.current.feature  http://localhost:3000/tests/run
</div>
<div class="highlight"><pre><code class="language-bash" data-lang="bash">
Feature: OpenWeatherMap API - Current weather data

  As a client of the OpenWeatherMap API
  I want to run tests
  In order to validate 'Current weather data' operations


  Scenario: Call current weather data for one location and by city name       # tmp/1432662589705-29129-2d1673fc61776d54:11
    Given a "REST" API definition at "http://api.openweathermap.org/data/2.5" # tmp/1432662589705-29129-2d1673fc61776d54:9
    Given endpoint "/weather" and method "get"                                # tmp/1432662589705-29129-2d1673fc61776d54:13
    And request query param "q" equals "London,UK"                            # tmp/1432662589705-29129-2d1673fc61776d54:14
    And request type "application/json"                                       # tmp/1432662589705-29129-2d1673fc61776d54:15
    When the request is executed                                              # tmp/1432662589705-29129-2d1673fc61776d54:16
    Then response status is "ok"                                              # tmp/1432662589705-29129-2d1673fc61776d54:17


1 scenario (1 passed)
6 steps (6 passed)
</code></pre></div>  

