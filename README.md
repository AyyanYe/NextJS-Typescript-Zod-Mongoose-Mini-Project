# NextJS TypeScript Zod Mongoose Mini Project

This is a mini project that demonstrates the integration of **Zod** with **Mongoose** and **TypeScript** within a **Next.js** application. The project provides a basic structure for managing `User` and `Message` schemas in a MongoDB database using Mongoose.

## Project Structure

### Models

#### Message Schema
The `Message` schema represents a basic message structure stored in the database.

```typescript
import mongoose, { Schema, Document } from "mongoose";

export interface Message extends Document {
    content: string;
    createdAt: Date;
}

const MessageSchema: Schema<Message> = new Schema({
    content: {
        type: String,
        required: true
    },
    createdAt: {
        type: Date,
        required: true,
        default: Date.now
    }
});
```

## User Schema

The `User` schema extends the Mongoose Document interface to include properties such as username, email, password, verification code, and a list of messages associated with the user.

```typescript
export interface User extends Document {
    username: string;
    email: string;
    password: string;
    verifyCode: string;
    verifyCodeExpiry: Date;
    isVerified: boolean;
    isAcceptingMessage: boolean;
    message: Message[];
}

const UserSchema: Schema<User> = new Schema({
    username: {
        type: String,
        required: [true, "Username is required"],
        trim: true,
        unique: true
    },
    email: {
        type: String,
        required: [true, "Email is required"],
        unique: true,
        match: [/^[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?$/, 'Please use a valid email address']
    },
    password: {
        type: String,
        required: [true, 'Password is required'],
        unique: true
    },
    verifyCode: {
        type: String,
        required: [true, 'Verify code is required'],
        unique: true
    },
    verifyCodeExpiry: {
        type: Date,
        required: [true, 'Verify code expiry is required'],
        default: Date.now
    },
    isVerified: {
        type: Boolean,
        default: false
    },
    isAcceptingMessage: {
        type: Boolean,
        default: true
    },
    message: [MessageSchema]
});

const UserModel = (mongoose.models.User as mongoose.Model<User>) || (mongoose.model<User>("User", UserSchema));

export default UserModel;
```

## Technologies Used

- Next.js: A React framework that enables server-side rendering and static site generation.
- TypeScript: A typed superset of JavaScript that compiles to plain JavaScript.
- Mongoose: A MongoDB object modeling tool designed to work in an asynchronous environment.
- Zod: A TypeScript-first schema declaration and validation library.

## Getting Started

### Prerequisites
Make sure you have the following installed:

- Node.js
- MongoDB

### Installation

- Clone the repository
```bash
git clone https://github.com/AyyanYe/NextJS-Typescript-Zod-Mongoose-Mini-Project.git
cd NextJS-Typescript-Zod-Mongoose-Mini-Project
```
- Install dependencies:
```bash
npm install
```
- Create a .env.local file in the root directory and add your MongoDB connection string:
```env
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/yourdbname?retryWrites=true&w=majority
```
- Run the development server:
```bash
npm run dev
```
Open http://localhost:3000 with your browser to see the result.

## Usage

The project includes basic schemas for `User` and `Message`, making it easy to expand with more complex functionality such as user authentication, messaging systems, etc.

## License

This project is licensed under the MIT License.
