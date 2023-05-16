In the given transcript, the important terms and ideas are as follows:

1. Many-to-Many Relationship: It refers to a relationship between two entities where multiple entities can be associated with multiple entities from another entity. In the example, messages can have multiple tags, and tags can be used across multiple messages.

2. Joined Table: A joined table combines information from multiple tables using a common key or relationship. In this case, the joined table combines information from the "messages" table and the "tags" table based on the tag code and message ID.

3. Route Setup: The transcript demonstrates setting up a route to handle a GET request for retrieving a specific message along with its corresponding tags based on the provided ID.

4. SQL Query: The transcript mentions the need to write a SQL query to fetch the tags associated with a specific message. The query involves joining the "messages", "messages_tags", and "tags" tables to retrieve the corresponding tag names.

To provide a simple demonstration, here's an example of how the SQL query might look based on the information provided:

```javascript
SELECT m.message_id, m.message, t.tag_name
FROM messages m
JOIN messages_tags mt ON m.message_id = mt.message_id
JOIN tags t ON mt.tag_code = t.tag_code
WHERE m.message_id = 1;
```

This query fetches the message with ID 1 along with its associated tag names. The result will include the message ID, message text, and the corresponding tag names.

Please note that this is a simplified example, and the actual implementation may vary based on the specific database schema and query structure.