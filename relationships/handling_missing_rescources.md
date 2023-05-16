In the given transcript, the important terms and ideas are as follows:

1. Object-Oriented Programming (OOP): The transcript introduces the concept of OOP and explains its benefits, such as organizing code, encapsulating data and methods, and achieving polymorphism. It highlights the advantages of using classes to abstract away implementation details and improve code readability.

2. Refactoring and Improvement: The transcript acknowledges the limitations of the current approach, which involves writing SQL queries directly in the routes. It emphasizes the need to refactor and improve the code by separating view logic from data or model logic. The goal is to build an object-oriented solution to enhance code organization and reusability.

3. Encapsulation: Encapsulation refers to the practice of grouping related data and methods together within a class or object. It allows for better organization and modularity by creating logical capsules or units of functionality. Encapsulating code in this manner helps improve code maintainability and understandability.

4. Polymorphism: Polymorphism enables different types or objects to exhibit similar behavior or have common methods. The transcript uses the example of animals (cats and dogs) with different speak methods (e.g., "Meow" and "Woof") to illustrate polymorphism. It highlights how polymorphic behavior can be achieved through classes and inheritance.

To provide a simple demonstration, here's an example illustrating the concepts discussed:

```javascript
class Message {
  constructor(id, text) {
    this.id = id;
    this.text = text;
  }

  getTags() {
    // Perform the SQL query to retrieve associated tags for the message
    const query = `SELECT tag FROM tags WHERE message_id = $1`;
    const result = db.query(query, [this.id]);
    return result.rows.map((row) => row.tag);
  }

  static getById(id) {
    // Perform the SQL query to fetch a message by its ID
    const query = `SELECT id, message FROM messages WHERE id = $1`;
    const result = db.query(query, [id]);
    if (result.rows.length === 0) {
      return null;
    }
    const { id: messageId, message } = result.rows[0];
    return new Message(messageId, message);
  }
}

// Usage example
const message = Message.getById(1);
if (message) {
  const tags = message.getTags();
  console.log(`Message: ${message.text}`);
  console.log(`Tags: ${tags.join(", ")}`);
} else {
  console.log("Message not found");
}
```

In this example, a `Message` class is defined with a constructor, `getTags` method, and a static method `getById` to retrieve a message by its ID from the database. The `getTags` method performs an SQL query to fetch associated tags for the message. The code demonstrates how encapsulation and object-oriented design can be used to abstract away SQL queries and provide a cleaner interface for interacting with the database.

Please note that this is a simplified demonstration, and the actual implementation may vary based on the specific requirements, database structure, and framework used.