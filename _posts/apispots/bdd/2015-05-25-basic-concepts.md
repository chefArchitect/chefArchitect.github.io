---
layout: project.bdd
title: Basic concepts - BDD Testing
headline: Basic concepts
description: Learn the basic concepts behind the service, its architecture and run you first test against a live API
excerpt: "API Testing for Humans"
search_omit: true
categories: apispots/bdd
tags: [apispots,bdd,testing]
---


# Philosophy
{:class="page-header"}

* Testing should be a core process in any software product
* Testing is imposed by a culture shared among team members
* Testing processes should be easy enough to be used by non-technical people too
* Testing should be performed on an automatic / manual basis  
{:class="lead"}

## Behavioral Driven Development (BDD)

[Behavior-driven development (BDD)](http://en.wikipedia.org/wiki/Behavior-driven_development){:target='_blank'} is based on the principles of test-driven development (TDD) and leverages the concept to a higher level of abstraction so that non-technical stakeholders can collaborate and take part in the process too.

>In software engineering, behavior-driven development (abbreviated BDD) is a software development process based on test-driven development (TDD).  Behavior-driven development combines the general techniques and principles of TDD with ideas from domain-driven design and object-oriented analysis and design to provide software development and management teams with shared tools and a shared process to collaborate on software development.
>
>Although BDD is principally an idea about how software development should be managed by both business interests and technical insight, the practice of BDD does assume the use of specialized software tools to support the development process. Although these tools are often developed specifically for use in BDD projects, they can be seen as specialized forms of the tooling that supports test-driven development. The tools serve to add automation to the ubiquitous language that is a central theme of BDD.

## User Stories

The foundation element behind BDD, is the **user story**.

>In software development and product management, a user story is one or more sentences in the everyday or business language of the end user or user of a system that captures what a user does or needs to do as part of his or her job function. User stories are used with agile software development methodologies as the basis for defining the functions a business system must provide, and to facilitate requirements management. It captures the 'who', 'what' and 'why' of a requirement in a simple, concise way, often limited in detail by what can be hand-written on a small paper note card.

A user story fully describes what a user can do with an application / service / system within the context of a specific functional feature.

<div class="highlight"><pre><code class="gherkin_en" data-lang="gherkin">
Feature: Feature
  Scenario Outline: A scenario
    # A comment
    @tag @tagz
    Given I have 4 cukes in my "big" and 'Given' Given belly
      """
      A doc string with Given
      """
    And a table:
      | with | some |
      | rows |      |
</code></pre></div>