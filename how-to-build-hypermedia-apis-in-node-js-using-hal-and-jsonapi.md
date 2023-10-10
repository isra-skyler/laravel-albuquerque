
# How to Build Hypermedia APIs in Node.js using HAL and JSON:API
As the lead backend developer at [Hybrid Web Agency](https://hybridwebagency.com/), I'm always seeking opportunities to enhance my expertise and push myself outside my comfort zone. While much of my work involves complex Laravel APIs, I strive to stay up-to-date on emerging standards across other frameworks as well.

My latest challenge was a personal passion project - building a hypermedia-driven API from the ground up using Node.js. My objective was ambitious yet practical: implement the HATEOAS architectural style according to both the HAL and JSON:API specifications. Through diligent experimentation and testing, I aimed to gain extensive hands-on experience applying these standards.

Over six months, I developed a fully functional order management system responding to requests with hypermedia links. This allowed deep evaluation of HAL and JSON:API for structuring relationships. Along the way, I encountered challenges that revealed nuanced differences between the approaches.

In this post, I will discuss the insights uncovered during my research. We'll examine how each spec models interconnected order, product and customer resources. I'll provide extensive code examples demonstrating how hypermedia enables intuitive API discovery.

Most importantly, sharing my learnings from this endeavor aims to benefit fellow API designers. It also allows me to enhance the sophistication we bring to client solutions through Hybrid Web Agency. My quest is to continually expand both my personal abilities and our organizational expertise.





### What is HATEOAS?

HATEOAS (Hypertext as the Engine of Application State) is an architectural style for building RESTful APIs centered around the concept of hypermedia. It establishes that clients should obtain all interactive context needed for subsequent requests from hyperlinks embedded in responses, rather than relying on out-of-band knowledge.

By including hypermedia controls like links, forms, etc, HATEOAS APIs provide rich metadata that clients use to autonomously navigate available functionality. For example, retrieving an order resource would disclose links to related invoices, products, statuses etc. This teaches clients which interactions are valid without predefining sequences.

Adopting this hypermedia-driven approach uncouples clients from specific implementation details, allowing APIs to evolve freely over time through structural changes. New resources and relationships are automatically discoverable via prompts in responses.

HATEOAS also produces "self-documenting" interfaces where responses prescribe their own interactive context. This imbues APIs with natural exploration, resilience to change, and flexibility in how their domain can progress independent of clients.

In essence, HATEOAS establishes REST systems navigated through hypermedia prompts rather than static out-of-band configuration. Well-designed HATEOAS interfaces cultivate intrinsic discoverability, independence and freedom to develop through each linked request-response interaction.





### Benefits of HATEOAS

By embracing the HATEOAS approach, REST APIs gain several important benefits:

**Self-documenting Interfaces** - Responses containing hypermedia controls serve to describe their own interactive affordances. This makes APIs intrinsically discoverable through ubiquitous documentation embedded in each exchange.

**Loose Coupling** - Clients depend only on the hypermedia format and are uncoupled from specific resources, operations, or schemas used in responses. This allows APIs to evolve independent of clients by adding or replacing links over time. 

**Evolution Support** - Changing the API structure through new resource additions, moved endpoints, or modified relationships seamlessly propagates to existing clients. They automatically discover updates by simply following refreshed hypermedia prompts.

**Extensibility** - Hypermedia responses facilitate extension points for adding links to future or external functionality not envisioned during initial development. This future-proofs APIs to evolve beyond their original scope.

**Discoverability** - By learning valid transitions through each response, clients can programmatically detect and leverage new properties or operations without prior notifications. 

**Flexibility** - HATEOAS decouples clients from rigid usage patterns and allows dynamic, fluid navigation tailored to contextual needs for each request.

In summary, embracing the HATEOAS style imbues REST APIs with natural resilience to change, aiding maintainability and expanding utility over their operational lifetimes. Hypermedia controls foster extensible, self-guiding interfaces.





## Evaluating Options for Hypermedia Links

When developing HATEOAS-driven Node APIs, two popular specifications for representing relational links are Hypertext Application Language (HAL) and JSON:API.

### Hypertext Application Language

HAL provides a simple format using a top-level "_links" property to associate resources via URLs.

```json 
{
  "_links": {
    "self": {"href": "/orders/1"},
    "items": {"href": "/orders/1/items"}
  }
}
```

It serves basic needs with minimal overhead.

### JSON:API

JSON:API establishes a robust standard, formally connecting resources through a "relationships" portion containing related IDs and hypermedia links. 

```
"relationships": {
  "items": {   
    "links": {
      "related": "/orders/1/relationships/items"
    },
    "data": [...] 
  }
}
```

This supports complex use cases but demands more implementation effort.






Here is the implementation section for a HAL API in Node.js:

### Key Considerations

HAL excels for lightweight hypermedia, while JSON:API acts as an exhaustive framework. Evaluate an API's scope, scale and interface sophistication to determine the appropriate level of specification needed. Overall, both unlock the HATEOAS design virtues.

## Implementing HAL in a Node.js API

Setting up a HAL API in a Node.js/Express application.

### Installing Dependencies

Bringing in the `express-hal-builder` module to generate HAL representations:

```
npm install express-hal-builder
```

### Defining HAL Resources

Creating Order and OrderItem schema objects:

```js
const Order = {/*...*/};
const OrderItem = {/*...*/}; 
```

### Building the Hypermedia 

Rendering resources and including links with the builder:

```js
app.get('/orders/:id', (req, res) => {

  const order = /*...*/;
  const builder = new HalBuilder();

  builder.representEntity(order)
    .childLink(order.items, '/orders/'+id+'/items');

  res.status(200).json(builder.get());

});
```

This exposes the HAL API structure.


## Mapping the HAL API Data Model through Exploration

Send initial requests to uncover starting endpoints and gradually reveal the domain model by:

- Parsing responses to catalogue resource and link types
- Extracting URL values used to connect related resources  
- Traversing embedded links to explore the relationship graph

## Developing a Connected Data API using JSON:API 

This guide demonstrates how to construct a RESTful backend service representing relationships between resources adhering to the JSON:API standard using Node and Express.

### Importing Required Dependencies

The `jsonapi-serializer` module helps generate standardized JSON:API response documents.

### Defining Core Resource Schemas

Models describe attributes and structure of entities like Order and LineItem.

### Generating JSON:API Responses  

To associate data across resources, responses:

1. Represent a root/parent resource

2. Embed or reference related child resources 

```js
app.get('/orders/:id', async (req, res) => {

  const order = await Order.findById(req.params.id);

  res.json(order.serialize({
    include: 'lineItems'  
  }));

});
```

This provides a foundation for developing REST APIs that expose interconnected data via the JSON:API specification.



# Conclusion
In summary, developing REST APIs using specifications like HAL and JSON:API provides structure and discoverability and future-proofs applications by supporting emerging technologies. Following established standards helps ensure APIs are intuitive for consumers to understand and explore.

If your team is working on a project that requires implementing such specifications to build a connected data API, doing so can be a complex endeavor requiring varying expertise. This is where working with an experienced Node.js development partner can help accelerate your project to completion.

As a full-service Node.js development company located in Albuquerque, we offer [Node.js Development Services in Albuquerque](https://hybridwebagency.com/albuquerque-nm/custom-laravel-development-services/) to help organizations build robust, specification-driven APIs. Our team of senior engineers has extensive experience implementing HAL, JSON:API and other standards to represent relationships between resources. Whether assisting with POC development, scaling an existing API, or managing an entire project lifecycle, we aim to deliver production-ready REST services on-time and on-budget.

By leveraging our expertise in Node.js, related frameworks and best practices for API design, your team can stay focused on core priorities while we ensure technical implementation meets quality and performance standards. Contact us to discuss how our Albuquerque-based services could benefit your project.



## References

- https://www.halspec.org/
The official specification website for HAL (Hypertext Application Language), detailing the standard.

- https://jsonapi.org/
The official website for the JSON:API specification, including documentation on compliant API design. 

- https://expressjs.com/en/advanced/best-practice-performance.html
Best practices guide from the Express.js website covering performance, security, and standard compliance for Node.js APIs.  

- https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design
Documentation from Microsoft on API design best practices, including modeling relationships and using standards.

- https://swagger.io/resources/building-blocks/hypermedia/
Swagger overview of hypermedia APIs and industry standards like HAL and JSON:API for enabling discoverability.
