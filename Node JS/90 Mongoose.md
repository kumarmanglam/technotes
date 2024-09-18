
Certainly! Below are the basic syntax and usage patterns for working with Mongoose, a popular Node.js library for MongoDB object modeling:

```markdown
# Mongoose Basics

## Installation
```bash
npm install mongoose
```

## Importing Mongoose
```javascript
const mongoose = require('mongoose');
```

## Connecting to MongoDB
```javascript
mongoose.connect('mongodb://localhost:27017/mydatabase', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
```

## Defining a Schema
```javascript
const { Schema } = mongoose;

const userSchema = new Schema({
  name: String,
  age: Number,
  email: String,
});
```

## Creating a Model
```javascript
const User = mongoose.model('User', userSchema);
```

## Creating and Saving Documents
```javascript
const user = new User({
  name: 'John Doe',
  age: 30,
  email: 'john@example.com',
});

user.save()
  .then((result) => {
    console.log('Document saved successfully:', result);
  })
  .catch((error) => {
    console.error('Error saving document:', error);
  });
```

## Querying Documents
```javascript
User.find({ name: 'John Doe' })
  .then((users) => {
    console.log('Users found:', users);
  })
  .catch((error) => {
    console.error('Error finding users:', error);
  });
```

## Updating Documents
```javascript
User.updateOne({ name: 'John Doe' }, { age: 31 })
  .then((result) => {
    console.log('Document updated successfully:', result);
  })
  .catch((error) => {
    console.error('Error updating document:', error);
  });
```

## Deleting Documents
```javascript
User.deleteOne({ name: 'John Doe' })
  .then((result) => {
    console.log('Document deleted successfully:', result);
  })
  .catch((error) => {
    console.error('Error deleting document:', error);
  });
```

## Closing Connection
```javascript
mongoose.connection.close();
```
