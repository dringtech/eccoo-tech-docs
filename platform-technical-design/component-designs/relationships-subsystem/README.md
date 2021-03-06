# relationships subsystem

The relationships subsystem is one half of the platform core, partnering the [transactions subsystem](../transactions-subsystem/). This component enables the multiple relationships enabled and required by the platform to be tracked:

* person-person \([relationships](relationship-component.md)\): I provide care to you
* person-role \([roles](roles-component.md)\): I am a care giver and a circle administrator - a.k.a. hats
* person-circle: I am part of the Blackshaw Head circle

The relationships subsystem is clustered around a Graph database which efficiently manages the social network enabled by the platform. This technology also lends itself to tracking [appointments](appointments-component.md) and [care notes](care-notes-component.md). The [matching](matching-component.md) algorithms will use the graph as a primary source when building carer recommendations.

The circles and other team-related aspects of the subsystem are out of scope for the MVP.

NB there is currently some fluidity in the location of the roles component - it will either be part of the relationships subsystem, or may better reside in the identify and access subsystem. This is to be resolved once the technology stack has been selected.

## Candidate technologies

The core of the subsystem will be a [graph database](https://en.wikipedia.org/wiki/Graph_database) servicing the other components. These are an efficient and scalable means of managing this kind of data. The most popular such database is [Neo4j](https://neo4j.com/). This has an open source Community Edition, as well as a series of [paid subscriptions](https://neo4j.com/subscriptions), including a Startup tier which offers free access to the enterprise-grade features. Cloud platforms also offer graph database services, including Amazon Neptune and Azure Cosmos DB's Gremlin API.

Each of the other components in this subsystem will be written as an API application exposing a defined interface to wrapper and control access to the graph. Implementation technology is less critical, and will be driven by the hosting and graph database product selected.

### GraphQL

GraphQL is a compelling technology for the constellation of APIs surrounding this data store, as it allows rapid iteration over the features without placing a burden on the API tier. Depending on implementation technology, it is possible to extend GraphQL to provide fine-grained access control over parts of the API based on appropriate Javascript Web Token provision, or to limit API calls to authenticated users. This would provide the role-based access control that will be needed. See the [graphql-auth-directives](https://www.npmjs.com/package/graphql-auth-directives) as an example of this approach.

By adopting GraphQL the platform team makes a strong statement about the technical contract between backend and frontend. This will further enhance the flexibility of delivery of solution, as changes could be made to different parts of the solution without impacting others. The definition of this contract ensures that app parts of the system speak the same language.

An example partial GraphQL schema covering the core of the relationships subsystem is given below. This demonstrates the cleanliness of the data model underpinning the 

```text
{
    type Person {
        id: ID!
        name: String
        hats: [Hat]
        carers: [Person]
        caredFor: [Person]
        appointments: [Appointment]
    }
    
    type Hat {
        ref: ID!
        name: String
    }
    
    type Appointment {
        dateTime: DateTime
        location: Address
        attendees: [Person]  
    }
    
    type CareNote {
        subject: Person
        creator: Person
        readers: [Person]
        text: String
        dateTime: DateTime
    }
    
    scalar PostCode
    
    type Address {
        postCode: PostCode
    }
    
    type Query {
        listPossibleMatches(criteria: MatchCriteria): [Person]
    }
    
    input MatchCriteria {
        hats: Hat
    }
}
```

