
---

# GraphQL Server with Apollo, Redis, and MongoDB

## Description
This project creates a GraphQL server using Apollo Server that interfaces with a MongoDB database containing data about authors and books. Redis is used for caching to optimize queries and reduce database load. The server supports a range of queries and mutations for managing author and book data, while caching frequently accessed data to enhance performance.

## Features
- **GraphQL API with Apollo Server**: Provides a flexible API to interact with authors and books.
- **MongoDB Integration**: Manages and persists data in a NoSQL format.
- **Redis Caching**: Caches query results to improve performance on repeated data retrievals.
- **Data Relationships**: Supports nested data by linking books to authors, and vice versa.
- **Validation and Error Handling**: Ensures valid data entries and manages errors for robust functionality.

## Tech Stack
- Node.js
- Apollo Server (GraphQL)
- Redis
- MongoDB

## Schema

### Types
#### Author
- `_id: String`
- `first_name: String`
- `last_name: String`
- `date_of_birth: String`
- `hometownCity: String`
- `hometownState: String`
- `numOfBooks: Int` - Computed field counting books by the author.
- `books(limit: Int): [Book]` - Resolves a list of books by the author with optional limit.

#### Book
- `_id: String`
- `title: String`
- `genres: [String]`
- `publicationDate: String`
- `publisher: String`
- `summary: String`
- `isbn: String`
- `language: String`
- `pageCount: Int`
- `price: Float`
- `format: [String]`
- `author: Author` - Linked author resolver.

## API Endpoints

### Queries
- `authors: [Author]` - Retrieves all authors, with caching (expires in 1 hour).
- `books: [Book]` - Retrieves all books, with caching (expires in 1 hour).
- `getAuthorById(_id: String!): Author` - Fetches a single author by ID with indefinite caching.
- `getBookById(_id: String!): Book` - Fetches a single book by ID with indefinite caching.
- `booksByGenre(genre: String!): [Book]` - Fetches books by genre (case-insensitive, caches by lowercase genre for 1 hour).
- `booksByPriceRange(min: Float!, max: Float!): [Book]` - Retrieves books within a specified price range (caches for 1 hour).
- `searchAuthorsByName(searchTerm: String!): [Author]` - Searches for authors by name (case-insensitive, caches search results for 1 hour).

### Mutations
- `addAuthor(first_name: String!, last_name: String!, date_of_birth: String!, hometownCity: String!, hometownState: String!): Author` - Adds a new author to MongoDB and updates Redis cache.
- `editAuthor(_id: String!, first_name: String, last_name: String, date_of_birth: String, hometownCity: String, hometownState: String): Author` - Updates an author’s information (partial updates allowed).
- `removeAuthor(_id: String!): Author` - Deletes an author and all related books, updating Redis cache.
- `addBook(title: String!, genres: [String!]!, publicationDate: String!, publisher: String!, summary: String!, isbn: String!, language: String!, pageCount: Int!, price: Float!, format: [String!]!, authorId: String!): Book` - Adds a new book if the author exists, updating MongoDB and Redis.
- `editBook(_id: String!, title: String, genres: [String], publicationDate: String, publisher: String, summary: String, isbn: String, language: String, pageCount: Int, price: Float, format: [String], authorId: String): Book` - Updates a book’s details (partial updates allowed), with author verification if authorId is changed.
- `removeBook(_id: String!): Book` - Deletes a book and removes it from the author's record and Redis cache.

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/sachindevangan/GraphQL-Server-with-Apollo-Redis-and-MongoDB
   ```
2. Navigate to the project directory:
   ```bash
   cd graphql-server
   ```
3. Install dependencies:
   ```bash
   npm install
   ```
4. Configure your MongoDB and Redis connection settings in a `.env` file.
5. Seed the MongoDB database with the provided seed data (if applicable).
6. Start the server:
   ```bash
   npm start
   ```

## Usage
- Use GraphQL playground or a client like Postman to test queries and mutations.
- Refer to **API Endpoints** for query and mutation options.

## License
This project is licensed under the MIT License.

---