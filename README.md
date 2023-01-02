# Specification

**fs-db** in a simple document database designed with the following principals in mind:

* Implementable in any programming language (including `bash`) using only standard/core libraries.
* Embeddable.
* Operations are O(1) or O(n) time-complexity (I think).

## Primitives

### Database

* A **database** is a set of document collections. 
* Every document is a member of a single collection.
* A database has a unique identifier, which must be a legal filename.
* A database is contained in a directory on the file-system. Everything in the directory is part the database. 

E.g. `my-db`

### Collection

* A **collection** is a set of documents. 
* Every collection is part of a single database.
* A collection has a unique identifier, which must be a legal filename.
* A collection is container within single specific directory. Every file within that directory is a document.

E.g. `my-db/collections/customers`

### Document

* A **document** is an binary object. 
* Every document is a member of a single collection. 
* Each document has an unique identfier (within its collection) which must be a legal filename, e.g. a GUID. 
* Documents are stored on disk as files, who's name is the same as the identiferier. 

E.g. `my-db/collections/customers/abc-123`

### Index

* An **index** is a mapping from a specific value to set of document identifers for a collection.
* Every index has a unique ID within its collection, which must be a legal filename. 
* An index is a directory on a disk, containing sym-links to the documents for the value.

E.g. `my-db/indexes/customers/first-name/john/abc-123`

## Example

Consider a database for CRM. it might contain customers. 


```bash
# create the database
mkdir crm
# create a collection named "customers"
mkdir crm/collections/customers
# create a customer
echo '{"firstName": "John"}' > crm/collections/customers/abc-123
# create an index
mkdir crm/indexes/customers/first-name
# index that customer
ln -s crm/collections/customers/abc-123 crm/indexes/customers/first-name/John/abc-123
# list customers
ls crm/collections/customers
# get a customer
cat crm/collections/customers/abc-123
# find customers named "John"
ls crm/indexes/customers/first-name/John
```

## Limitations

### Due To File System

* Queries are only as fast a file-system operations.
* File-systems have limitations on valid file name.
* File-systems have limitations on number of files.
* File-systems have limitations on file size.

### Compared To Other Databases

* No grouping operations.
* No partial select.
* No inner/outer join collections.

## Benefits

* Trivial human-readable syntax and semantics.
* Portable.
* Easily supports files.
