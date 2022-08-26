---
title: REST Resource Modeling
author: Nathaniel Sandberg
last updated: 6/11/2021
---

You can follow these resource guidelines to get the following benefits:

* API consumers get the desired functionality from the APIs.
* The APIs behave correctly.
* The APIs are maintainable.

You can name any information a resource.
The resource guidelines highlight important considerations
when designing the resources in your APIs.
With these resource guidelines, you can make informed decisions about the resources your API consumers need.

## Types of Resources

You can use one of the following strategies when you are creating resources:

* You can make fine-grained resources with smaller, more precise resources that the app must use more interactions with.
* You can use coarse-grained resources with larger, less frequent requests.

You can expect two outcomes if the API consumer must directly manipulate resources:

* The API consumer must make more requests.
* The API consumer must determine more of the business logic.
    * If the API consumers must determine more of the business logic, they may make mistakes when they are creating interactions.
    * API consumers more tightly couple with the API if they determine more of the logic at a low level.
    * API consumers of low-level logic have to update and redeploy code more often as the API updates.

Remember the following when designing APIs:

* Do not select resource names based on the underlying API details.
* The client shouldn't be manipulating internal domain knowledge.

You can implement the API design in one of two ways:

* You can allow the client to update the resource using PUT.
    * The client must have knowledge of the domain.
    * The client code is more brittle.
* You can design the API around the resources that are based on the business processes and domain events.

To escape low-level CRUD, create business operation or business process resources using nouns and not verbs.

## Final Considerations

Consider the following when designing your REST APIs:

* Business needs
* Technical considerations
* Cost-benefit analysis
