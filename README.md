# Art Express API 🎨

A RESTful API built with Express.js and MongoDB for managing art collections with image uploads. This API provides endpoints for creating, reading, updating, and deleting art entries with associated images.

## 📋 Table of Contents

- [Features](#-features)
- [Technology Stack](#-technology-stack)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Environment Setup](#-environment-setup)
- [Running the Application](#-running-the-application)
- [API Endpoints](#-api-endpoints)
- [Request & Response Examples](#-request--response-examples)
- [Project Structure](#-project-structure)
- [Deployment](#-deployment)
- [License](#-license)

## ✨ Features

- **CRUD Operations**: Complete Create, Read, Update, and Delete functionality for art items
- **Image Upload**: Support for uploading and storing art images
- **Authentication**: Auth header-based access control for art collections
- **Auto-incrementing IDs**: Automatic sequential ID generation for art entries
- **MongoDB Integration**: NoSQL database with Mongoose ODM
- **Image Retrieval**: Dedicated endpoint for serving art images
- **Timestamps**: Automatic creation and update timestamps
- **Vercel Ready**: Configured for seamless deployment on Vercel

## 🛠 Technology Stack

- **Node.js**: JavaScript runtime environment
- **Express.js**: Fast, unopinionated web framework
- **MongoDB**: NoSQL database for data persistence
- **Mongoose**: MongoDB object modeling for Node.js
- **Multer**: Middleware for handling multipart/form-data (file uploads)
- **AWS SDK**: Amazon Web Services SDK (configured for potential S3 integration)
- **CORS**: Cross-Origin Resource Sharing enabled

### Dependencies

```json
{
  "express": "^4.19.2",
  "mongoose": "^8.4.1",
  "mongoose-sequence": "^6.0.1",
  "multer": "^1.4.5-lts.1",
  "multer-s3": "^3.0.1",
  "aws-sdk": "^2.1645.0",
  "cors": "^2.8.5"
}
```

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v14 or higher)
- **npm** or **yarn**
- **MongoDB** database (MongoDB Atlas account or local MongoDB installation)

## 🚀 Installation

1. **Clone the repository**

```bash
git clone https://github.com/dxvnee/art-express-api.git
cd art-express-api
```

2. **Install dependencies**

```bash
npm install
```

3. **Create Images directory** (if not exists)

```bash
mkdir Images
```

## ⚙️ Environment Setup

The application uses MongoDB Atlas for database connection. The connection string is configured in `server.js`:

```javascript
mongodb+srv://dxvnee:DTuzSPD1ip0vDr1C@artpediadb.cdylyrm.mongodb.net/Art?retryWrites=true&w=majority&appName=CreopediaDB
```

> **Note**: For production use, it's recommended to move the database connection string to environment variables using a `.env` file.

### Optional: Using Environment Variables

Create a `.env` file in the root directory:

```env
PORT=3800
MONGODB_URI=your_mongodb_connection_string
```

## 🏃 Running the Application

### Development Mode (with auto-reload)

```bash
npm run dev
```

### Production Mode

```bash
npm start
```

The server will start on port **3800** (or the port specified in `PORT` environment variable).

You should see:
```
Connected to database!
Server started on port 3000
```

Visit `http://localhost:3800` to verify the API is running. You should see:
```
Art API running...
```

## 📡 API Endpoints

### Base URL
```
http://localhost:3800/api/art
```

### Endpoints Overview

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/art` | Get all arts for authenticated user | Yes (Header) |
| GET | `/api/art/:id` | Get specific art by ID | Yes (Header) |
| GET | `/api/art/:id/image` | Get art image by ID | No |
| POST | `/api/art/createArtWithImage` | Create new art with image | Yes (Body) |
| PUT | `/api/art/:id` | Update art by ID | Yes (Header) |
| DELETE | `/api/art/:id` | Delete art by ID | Yes (Header) |

### Authentication

Most endpoints require an `auth` header for authentication:

```
auth: your_auth_token
```

## 📝 Request & Response Examples

### 1. Get All Arts

**Request:**
```http
GET /api/art
Headers:
  auth: your_auth_token
```

**Response:**
```json
[
  {
    "id": 1,
    "deskripsi": "Beautiful landscape painting",
    "alamat": "Jakarta, Indonesia",
    "harga": "500000",
    "gambar": "landscape.jpg",
    "auth": "your_auth_token",
    "createdAt": "2024-06-01T10:30:00.000Z",
    "updatedAt": "2024-06-01T10:30:00.000Z"
  }
]
```

### 2. Get Art by ID

**Request:**
```http
GET /api/art/1
Headers:
  auth: your_auth_token
```

**Response:**
```json
{
  "id": 1,
  "deskripsi": "Beautiful landscape painting",
  "alamat": "Jakarta, Indonesia",
  "harga": "500000",
  "gambar": "landscape.jpg",
  "auth": "your_auth_token",
  "createdAt": "2024-06-01T10:30:00.000Z",
  "updatedAt": "2024-06-01T10:30:00.000Z"
}
```

### 3. Get Art Image

**Request:**
```http
GET /api/art/1/image
```

**Response:**
```
Returns the image file (JPEG, PNG, etc.)
```

### 4. Create Art with Image

**Request:**
```http
POST /api/art/createArtWithImage
Headers:
  Content-Type: multipart/form-data
  
Body (form-data):
  deskripsi: "Beautiful landscape painting"
  alamat: "Jakarta, Indonesia"
  harga: "500000"
  gambar: [image file]
  auth: "your_auth_token"
```

**Response:**
```json
{
  "status": "success",
  "message": "Art created successfully",
  "art": {
    "id": 1,
    "deskripsi": "Beautiful landscape painting",
    "alamat": "Jakarta, Indonesia",
    "harga": "500000",
    "gambar": "landscape.jpg",
    "auth": "your_auth_token",
    "createdAt": "2024-06-01T10:30:00.000Z",
    "updatedAt": "2024-06-01T10:30:00.000Z"
  }
}
```

### 5. Update Art

**Request:**
```http
PUT /api/art/1
Headers:
  Content-Type: multipart/form-data
  auth: your_auth_token
  
Body (form-data):
  deskripsi: "Updated landscape painting"
  alamat: "Bandung, Indonesia"
  harga: "600000"
  gambar: [new image file]
```

**Response:**
```json
{
  "status": "success",
  "message": "Art created successfully"
}
```

### 6. Delete Art

**Request:**
```http
DELETE /api/art/1
Headers:
  auth: your_auth_token
```

**Response:**
```json
{
  "status": "success",
  "message": "Art created successfully"
}
```

## 📂 Project Structure

```
art-express-api/
├── controllers/
│   └── art.controller.js      # Business logic for art operations
├── models/
│   └── art.model.js           # Mongoose schema for Art
├── routes/
│   └── art.route.js           # API route definitions
├── Images/                     # Directory for uploaded images
├── .vercel/                    # Vercel deployment configuration
├── server.js                   # Main application entry point
├── package.json                # Project dependencies and scripts
├── vercel.json                 # Vercel deployment settings
└── README.md                   # Project documentation
```

### File Descriptions

- **server.js**: Main application file that sets up Express server, connects to MongoDB, and defines base routes
- **art.controller.js**: Contains all controller functions for handling art-related operations
- **art.model.js**: Defines the MongoDB schema for art items with auto-incrementing IDs
- **art.route.js**: Defines all API endpoints and maps them to controller functions
- **Images/**: Storage directory for uploaded art images

## 🚀 Deployment

### Deploying to Vercel

This project is configured for deployment on Vercel:

1. **Install Vercel CLI** (if not already installed)

```bash
npm install -g vercel
```

2. **Deploy**

```bash
vercel
```

3. **Production Deployment**

```bash
vercel --prod
```

The `vercel.json` configuration file is already set up with:
- Node.js runtime configuration
- Automatic routing to `server.js`
- Required dependencies installation

### Environment Variables on Vercel

Don't forget to add your environment variables in the Vercel dashboard:
- `MONGODB_URI`
- `PORT` (optional)

## 🔒 Security Notes

> ⚠️ **Important**: The current MongoDB connection string is hardcoded in the source code. For production use:
> 
> 1. Move the connection string to environment variables
> 2. Never commit sensitive credentials to version control
> 3. Use `.env` files for local development
> 4. Set up proper authentication mechanisms
> 5. Implement rate limiting and input validation

## 🧪 Testing

To test the API, you can use tools like:
- **Postman**: Import the API endpoints and test each one
- **cURL**: Command-line testing
- **Thunder Client**: VS Code extension
- **Insomnia**: REST API client

### Example cURL Request

```bash
# Get all arts
curl -X GET http://localhost:3800/api/art \
  -H "auth: your_auth_token"

# Create art with image
curl -X POST http://localhost:3800/api/art/createArtWithImage \
  -F "deskripsi=Beautiful painting" \
  -F "alamat=Jakarta" \
  -F "harga=500000" \
  -F "auth=your_auth_token" \
  -F "gambar=@/path/to/image.jpg"
```

## 📄 License

This project is licensed under the ISC License.

## 👤 Author

**dxvnee**

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

---

**Made with ❤️ using Node.js and Express**
