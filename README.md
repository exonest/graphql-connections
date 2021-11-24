# GraphQL Connections (Pagination)

This package is a spec-compliant set of models and utilities based on [Relay's Graphql Cursor Connections Specification](https://relay.dev/graphql/connections.htm) and is created using [Generics](https://docs.nestjs.com/graphql/resolvers#generics)

## Introduction

To use pagination in GraphQL, it's proposed by the spec that we use the "Connections" pattern and expose these connections in a standardized way.

- In queries, you use `PaginationArgs` to slice and paginate the result.
- In response, you return `Connection` objects to provide cursors and a way to tell the client when more results are available.

## Installation

On Yarn:

```shell
yarn add @exonest/graphql-connections
```

On NPM:

```shell
npm install @exonest/graphql-connections
```

## Usage

You can create a new paginated model like so:

```ts
import { Paginated } from '@exonest/graphql-connections';
import { Product } from '../product.model';

export class ProductConnection extends Paginated(Product) {}
```

Note that for your connection to be compliant to relay's specifications, its name should end with `Connection`. ([reference](https://relay.dev/graphql/connections.htm#sec-Reserved-Types))

To create a query for a paginated resource, do like so:

```ts
@Query(() => ProductConnection)
resourcePublications(
    @Args() { after, before, first, last, skip }: PaginationArgs,
) {
    // return data based on pagination
}
```

### With Prisma

If you use Prisma, there is another spec-compliant package that facilitates returning data as connections: [@devoxa/prisma-relay-cursor-connection](https://github.com/devoxa/prisma-relay-cursor-connection)

### Apollo Cache Compatability

Be default, apollo server won't cache if a field on resolver is an object, thinking objects need database calls and probably are expensive to resolve. While in the case of pagination's `node` object, they are already solved. So I've added cache control inheritance to all the respective fields and you don't really have to worry about that. This package goes nicely with the other exonest package, [exonest/graphql-cache-control](https://github.com/exonest/graphql-cache-control)
