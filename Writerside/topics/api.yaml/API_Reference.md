# API Reference

## Resource mapping

The API provides the following endpoints for interacting with the platform:

### Posts

{type="wide" sorted="asc"}
GET /api/Posts
: Retrieve all posts

GET /api/Posts/{id}
: Retrieve a post by ID

POST /api/Posts
: Create a new post

## Post Object

The `Post` model in the application has the following properties:
- `ObjectId`: 12 bytes
- `string` x 3: 1,500 bytes (assuming a string is approximately 500 bytes)
- `DateTime` x 2: 16 bytes

Breakdown:

- `ObjectId`: This is the unique identifier for each post. It's 12 bytes in size.
- `string` x 3: These are various text fields in the post, such as the title, content, etc. Each string is assumed to be approximately 500 bytes, so three strings would be 1,500 bytes in total.
- `DateTime` x 2: These are timestamps for when the post was created and last updated. Each DateTime is 8 bytes, so two would be 16 bytes.

So, the total size of a `Post` object would be approximately 1,528 bytes.

## Traffic Estimates

Based on the size of the data transferred for each endpoint and the estimated usage patterns, I made the following traffic estimates:

### `POST /api/Posts`

Each `Post` object is approximately 1,528 bytes in size. If a post is created every second, then this endpoint would handle 86,400 requests per day. In terms of bytes per second, this would be 1,528 bytes per second.

### `GET /api/Posts/{id}`

Knowing that the `id` is a 24-character hexadecimal string and the `Post` object is approximately 1,528 bytes, then each request would transfer approximately 1,552 bytes of data (1,528 bytes for the `Post` object + 24 bytes for the `id`). If this endpoint is hit once every second, then it would also transfer 1,552 bytes per second.