---
header-id: learning-more-about-osgi
---

# Learning More about OSGi

[TOC levels=1-4]

There is much more to learn about developing apps using OSGi. Several resources
are listed below and many more abound. To make the best of your time, however,
avoid OSGi service articles that explain techniques that are older and more
complicated than Declarative Services.

Developers new to OSGi should check out these resources:

-   [Introduction to Liferay Development](/docs/7-1/tutorials/-/knowledge_base/t/introduction-to-liferay-development): 
    For using OSGi to develop on @product@.

-   [OSGi enRoute](http://enroute.osgi.org/) is a site the OSGi Alliance
    provides to the OSGi community. Its
    [Tutorials](https://enroute.osgi.org/Tutorial/)
    provide hands-on experience with OSGi modules and Declarative Services.

-   [OSGi Alliance's Developer](https://www.osgi.org/developer) section
    explains OSGi's architecture and modularity. 

If you're ready to dive deep into OSGi, read the OSGi specifications. They're
well-written and provide comprehensive details on all that OSGi offers.
[*The OSGi Alliance OSGi Compendium: Release 6*](https://osgi.org/download/r6/osgi.cmpn-6.0.0.pdf)
specifies the following services that @product-ver@ leverages extensively.

-   *Declarative Services Specification*

-   *Configuration Admin Service Specification*: For modifying deployed
    bundles. Since Configuration Admin services are already integrated with
    Declarative Services, however, Liferay developers need not use the low-level
    API.

-   *Metatype Service Specification*: For describing attribute types as metadata.
