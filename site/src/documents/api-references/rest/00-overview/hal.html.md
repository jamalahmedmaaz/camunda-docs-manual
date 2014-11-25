---

title: 'Hypertext Application Language (HAL)'
category: 'Overview'

---

The REST API provides some resources in an additional media type. The
[HAL][hal] media type `application/hal+json` describes a format which contains
links and information of other resources. This allows us to embed the
process definition or assignee of a task directly in the response. Which
reduces the number of necessary requests to gather all informations regarding a
single task or a list of tasks.

### Caching of HAL relations

During the generation of a HAL response linked resources are resolved to embed
them.  Some of these resolved resources, like process definitions or users, are
rarely modified. Also if user information are stored in an external system like
LDAP every response also will access this external system which is an
unnecessary overhead. To reduces such expensive requests the REST API can be
configured to use a cache to temporary store such relations.

This caching can be configured in the `web.xml` file of the REST API.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">

  <!-- ... -->

  <listener>
    <listener-class>org.camunda.bpm.engine.rest.hal.cache.HalRelationCacheBootstrap</listener-class>
  </listener>

  <context-param>
    <param-name>org.camunda.bpm.engine.rest.hal.cache.config</param-name>
    <param-value>
      {
        "cacheImplementation": "org.camunda.bpm.engine.rest.hal.cache.DefaultHalResourceCache",
        "caches": {
          "org.camunda.bpm.engine.rest.hal.user.HalUser": {
            "capacity": 100,
            "secondsToLive": 900
          },
          "org.camunda.bpm.engine.rest.hal.processDefinition.HalProcessDefinition": {
            "capacity": 1000,
            "secondsToLive": 600
          }
        }
      }
    </param-value>
  </context-param>

  <!-- ... -->

</web-app>
```

To bootstrap the caching the `HalRelationCacheBootstrap` context listener is
used. It is configured by the context parameter
`org.camunda.bpm.engine.rest.hal.cache.config`. The configuration is provided
as JSON and consists of two properties:

<table class="table table-striped">
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>cacheImplementation</td>
    <td>
      The class which is used as cache. The class has to implement the
      <code>org.camunda.bpm.engine.rest.cache.Cache</code> interface.
      A simple default implementation is provided by the
      <code>org.camunda.bpm.engine.rest.hal.cache.DefaultHalResourceCache</code> class.
    </td>
  </tr>
  <tr>
    <td>caches</td>
    <td>
      A JSON object to specify which HAL relations should be cached. Every HAL relation cache is configured
      separately and identified by the HalResource class to cache. The possible configuration parameters
      depend on the cache implementation and have to be available as setters on the implementation class.
    </td>
  </tr>
</table>

The simple default cache implementation `DefaultHalResourceCache` provides following configuration options:

<table class="table table-striped">
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>capacity</td>
    <td>
      The number of maximal cache entries.
    </td>
  </tr>
  <tr>
    <td>secondsToLive</td>
    <td>
      The number of seconds a cache entry is valid. If a cache entry is expired it is removed
      and again resolved.
    </td>
  </tr>
</table>



[hal]: http://stateless.co/hal_specification.html