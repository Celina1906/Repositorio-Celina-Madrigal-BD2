**Resumen 4**  
Estudiante: Celina Madrigal Murillo  
Carné: 2020059364  

# Schema-Agnostic Indexing with Azure DocumentDB

## 1. INTRODUCTION

Azure DocumentDB is Microsoft’s multi-tenant distributed database service for managing JSON documents at Internet scale. The indexing subsystem needs to support automatic indexing of documents without requiring a schema or secondary indices, DocumentDB’s query language, real-time, consistent queries in the face of sustained high document ingestion rates, and multitenancy under extremely frugal resource budgets while still providing predictable performance guarantees and remaining cost effective.

### 1.1 Overview of the Capabilities

DocumentDB is based on the JSON data model and JavaScript language directly within its database engine. We believe this is crucial for eliminating the “impedance mismatch” between the application programming languages/type-systems and the database schema. Specifically, this approach enables the following DocumentDB capabilities:

- The query language supports rich relational and hierarchical queries. 
- The database engine is optimized to serve consistent queries in the face of sustained high volume document writes.
- Transactional execution of application logic provided via stored procedures and triggers, authored entirely in JavaScript and executed directly inside DocumentDB’s database engine.
- As a geo-distributed database system, DocumentDB offers well-defined and tunable consistency levels for developers to choose from and corresponding performance guarantees.
- As a fully-managed, multi-tenant cloud database service, all machine and resource management is abstracted from users.

### 1.2 Resource Model

A tenant of DocumentDB starts by provisioning a database account. A database account manages one or more DocumentDB databases. A DocumentDB database in-turn manages a set of entities: users, permissions and collections. A DocumentDB collection is a schema-agnostic container of arbitrary user generated documents. In addition to documents, a DocumentDB collection also manages stored procedures, triggers, user defined functions (UDFs) and attachments. Entities under the tenant’s database account – databases, users, collections, documents etc. are referred to as resources. Each resource is uniquely identified by a stable and logical URI and is represented as a JSON document.

### 1.3 System Topology

The DocumentDB service is deployed worldwide across multiple Azure regions. We deploy and manage DocumentDB service on clusters of machines each with dedicated local SSDs. Upon deployment, the DocumentDB service manifests itself as an overlay network of machines, referred to as a federation which spans one or more clusters.

### 1.4 Design Goals for Indexing

We designed the indexing subsystem of DocumentDB’s database 
engine with the following goals:

- **Automatic indexing:** Documents within a DocumentDB collection could be based on arbitrary schemas.
- **Configurable storage/performance tradeoffs:** Although documents are automatically indexed by default, developers should be able to make fine grained tradeoffs between the storage overhead of index, query consistency and write/query performance using a custom indexing policy.
- **Efficient, rich hierarchical and relational queries:** The index should efficiently support the richness of DocumentDB’s query APIs
- **Consistent queries in face of sustained volume of document writes:** For high write throughput workloads requiring consistent queries, the index needs to be updated efficiently and synchronously with the document writes.
- **Multi-tenancy:** Multi-tenancy requires careful resource governance.

## 2. SCHEMA AGNOSTIC INDEXING

### 2.1 No Schema, No Problem!

The schema of a document describes the structure and the type system of the document independent of the document instance. With a goal to eliminate the impedance mismatch between the database and the application programming models, DocumentDB exploits the simplicity of JSON and its lack of a schema specification. It makes no assumptions about the documents and allows documents within a DocumentDB collection to vary in schema, in addition to the instance specific values.

### 2.2 Documents as Trees

The technique which helps blurring the boundary between the schema of JSON documents and their instance values, is representing documents as trees.

### 2.3 Index as a Document

With automatic indexing, every path in a document tree is indexed. Each update of a document to a DocumentDB collection leads to update of the structure of the index.

### 2.4 DocumentDB Queries

Developers can query DocumentDB collections using queries written in SQL and JavaScript. Both SQL and JavaScript queries get translated to an internal intermediate query language called DocumentDB Query IL.

## 3. LOGICAL INDEX ORGANIZATION

The index is a union of all the documents and is also represented as a tree. Each node of the index tree contains a list of document ids corresponding to the documents containing the given label value. The tree representation of documents and the index enables a schema-agnostic database engine. Finally we looked at how queries also operate against the index tree.

### 3.1 Directed Paths as Terms

A term represents a unique path in the index tree.

#### 3.1.1 Encoding Path Information

The choice of encoding scheme for the path information significantly influences the storage size of the terms and consequently the overall index.

#### 3.1.2 Partial Forward Path Encoding Scheme

The partial forward path encoding involves parsing of the document from the root and selecting three suffix nodes successively to yield a distinct path consisting of exactly three segments.

#### 3.1.3 Partial Reverse Path Encoding Scheme

The partial reverse path encoding scheme is similar to the partial forward scheme, in that it selects three suffix nodes successively to yield a distinct path consisting of exactly three segments.

### 3.2 Bitmaps as Postings Lists

A postings list captures the document ids of all the documents which contain the given term. We apply two techniques:

- **Partitioning a Postings List:** Each insertion of a new document to a DocumentDB collection is assigned a monotonically increasing document id. 
- **Dynamic Encoding of Posting Entries:** This marks the upper bound on the postings list within a bucket.

### 3.3 Customizing the Index

Developers can customize the trade-offs between storage, write/query performance, and query consistency, by overriding the default indexing policy on a DocumentDB collection and configuring the following aspects: Including/Excluding documents and paths to/from index, Configuring Various Index Types and Configuring Index Update Modes.

## 4. PHYSICAL INDEX ORGANIZATION

### 4.1 The “Write” Data Structure

Index maintenance must be performed against the following constraints:
1. Index update performance must be a function of the arrival rate of the index-able paths.
2. Index update cannot assume any path locality among the incoming documents. 
3. Index update for documents in a collection must be done within the CPU, memory and IOPS budget allocated per DocumentDB collection.
4. Each index update should have the least possible write amplification.
5. Each index update should incur minimal read amplification.

### 4.2 The Bw-Tree for DocumentDB

This implementation is used as the foundational component of DocumentDB’s database engine for several reasons.The Bw-Tree uses latch-free in-memory updates and log structured storage for persistence.

#### 4.2.1 High Concurrency

The Bw-Tree operates in a latch-free manner, allowing a high degree of concurrency in a natural manner.

#### 4.2.2 Write Optimized Storage Organization

Bw-tree storage is organized in a log-structured 
manner and Bw-tree pages are flushed in an incremental.
manner.

### 4.3 Index Updates

#### 4.3.1 Document Analysis

The document analysis function A takes the document content D corresponding to a logical timestamp when it was last updated, and the indexing policy I and yields a set of paths P.

#### 4.3.2 Efficient and Consistent Index Updates

Recall that an index stores term-to-postings list mappings, where a postings list is a set of document ids. Thus, a new document insertion (or, deletion) requires updating of the postings lists for all terms in that document.

#### 4.3.3 Lazy Index Updates with Invalidation Bitmap

The lazy indexing mode is performed in the background, asynchronously with the incoming writes, usually when the replica is quiescent.

### 4.4 Index Replication and Recovery

#### 4.4.1 Index Replication

The primary replica receiving the writes analyzes the document and generates the terms. The primary replica applies it to its database engine instance as well as sending the stream containing the terms to the secondaries.

#### 4.4.2 Index Recovery

The Bw-Tree exposes an API that allows the upper layer in the indexing subsystem to indicate that all index updates below some LSN should be made stable. 

### 4.5 Index Resource Governance

#### 4.5.1 Index Resource Governance

All operations within a DocumentDB’s database engine are performed within the quota allocated for CPU, memory, storage IOPS and the on-disk storage.

## 5. INSIGHTS FROM THE PRODUCTION WORKLOADS

While we collect and analyze numerous indexing related metrics from DocumentDB service deployed worldwide in Azure datacenters, here we present a selected set of metrics which guided some of the design decisions we presented earlier in the paper. 