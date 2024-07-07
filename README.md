# @arunvaradharajalu/common.mongodb-paginator

[![npm version](https://badge.fury.io/js/%40arunvaradharajalu%2Fcommon.mongodb-paginator.svg)](https://badge.fury.io/js/%40arunvaradharajalu%2Fcommon.mongodb-paginator)
[![GitHub issues](https://img.shields.io/github/issues/arunv11u/common.mongodb-paginator)](https://github.com/arunv11u/common.errors/mongodb-paginator)
[![GitHub license](https://img.shields.io/github/license/arunv11u/common.mongodb-paginator)](https://github.com/arunv11u/common.mongodb-paginator/blob/master/LICENSE)

This package provides MongoDB pagination utilities for efficiently retrieving large datasets in parts. 

## Installation

To install the package, use npm:

```bash
npm install @arunvaradharajalu/common.mongodb-paginator
```

## Usage

### Basic Usage

Here is an example of how to use the MongoDB paginator in your project:

```typescript
import { MongoDBPaginatorImpl, PaginationOptions, PaginationWithIdOptions } from '@arunvaradharajalu/common.mongodb-paginator';
import { MongoDBRepository } from '@arunvaradharajalu/common.mongodb-api';

// Initialize MongoDB repository
const mongodbRepository = new MongoDBRepository(/* MongoDB connection details */);

// Initialize paginator
const paginator = new MongoDBPaginatorImpl(mongodbRepository);

// Define pagination options
const options = new PaginationOptions();
options.pageIndex = 0; // Page number
options.pageSize = 10; // Number of documents per page
options.sortField = 'name'; // Field to sort by
options.sortType = SortTypes.ASC; // Sort order (ascending or descending)

// Define a filter
const filter = { /* your filter criteria */ };

// Retrieve paginated data
paginator.find('yourCollectionName', filter, options)
  .then(result => {
    console.log('Documents:', result.docs);
    console.log('Total Count:', result.count);
  })
  .catch(error => {
    console.error('Error fetching paginated data:', error);
  });
```

### Advanced Usage with Aggregation

The paginator also supports MongoDB aggregation with pagination:

```typescript
const pipeline = [
  { $match: { /* your match criteria */ } },
  { $group: { _id: '$field', total: { $sum: '$value' } } }
];

paginator.aggregate('yourCollectionName', pipeline, options)
  .then(result => {
    console.log('Documents:', result.docs);
    console.log('Total Count:', result.count);
  })
  .catch(error => {
    console.error('Error fetching aggregated paginated data:', error);
  });
```

### Pagination with Index

You can also paginate using an index field other than `_id`:

```typescript
const paginationWithIdOptions = new PaginationWithIdOptions();
paginationWithIdOptions.pageSize = 5;
paginationWithIdOptions.sortField = 'name';
paginationWithIdOptions.sortType = SortTypes.ASC;

paginator.findWithIndex('yourCollectionName', filter, paginationWithIdOptions)
  .then(result => {
    console.log('Documents:', result.docs);
    console.log('Total Count:', result.count);
  })
  .catch(error => {
    console.error('Error fetching paginated data with index:', error);
  });
```

## API Reference

### Classes

`MongoDBPaginatorImpl`:
- Constructor: `new MongoDBPaginatorImpl(mongodbRepository: MongoDBRepository)`
- Methods: 
	- `find(collectionName: string, filter: Filter<Document>, options: PaginationOptions): Promise<{ docs: Document[], count: number }>`
	- `aggregate(collectionName: string, pipeline: Document[], options: PaginationOptions): Promise<{ docs: any[], count: number }>`
	- `findWithIndex(collectionName: string, filter: Filter<Document>[], options: PaginationWithIdOptions): Promise<{ docs: Document[], count: number }>`
	- `aggregateWithIndex(collectionName: string, pipeline: Document[], options: PaginationWithIdOptions): Promise<{ docs: any[], count: number }>`

`PaginationOptions`:
- Properties:
	- `pageIndex: number` (default: 0)
	- `pageSize: number` (default: 10)
	- `sortType: SortTypes` (default: SortTypes.DESC)
	- `sortField: string` (default: '_id')
	- `collationOptions: CollationOptions | null` (default: null)

`PaginationWithIdOptions`:
- Properties:
	- `type: PaginatorTypes` (default: PaginatorTypes.FORWARD)
	- `pageSize: number` (default: 5)
	- `sortType: SortTypes` (default: SortTypes.ASC)
	- `sortField: string` (default: '_id')
	- `sortFieldValue: string | null` (default: null)
	- `id: string | null` (default: null)
	- `collationOptions: CollationOptions | null` (default: null)


## Running Tests

To run the tests, use:

```bash
npm test
```

The test results will be generated in an HTML report with the title "MongoDB Paginator Test Report".

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue for bug fixes, improvements, or new features.

## Author

Arun Varadharajalu

## License

This project is licensed under the ISC License.