# api-handle
A lightweight, flexible JavaScript library for managing API requests with built-in error handling, request cancellation, and response caching.

[![npm version](https://img.shields.io/npm/v/api-handle.svg)](https://www.npmjs.com/package/api-handle)
[![License](https://img.shields.io/github/license/anasraheemdev/api-handle)](https://github.com/anasraheemdev/api-handle/blob/main/LICENSE)

## Features

- 🚀 Simple, intuitive API for making HTTP requests
- ⚡ Promise-based with async/await support
- 🔄 Automatic request cancellation for race conditions
- 💾 Optional response caching
- 🛡️ Built-in error handling
- 🔌 Interceptors for request and response processing
- 📦 Lightweight with zero dependencies
- 🧩 TypeScript support

## Installation

```bash
npm install api-handle
# or
yarn add api-handle
# or
pnpm add api-handle
```

## Quick Start

```javascript
import apiHandle from 'api-handle';

// Basic GET request
const fetchUsers = async () => {
  try {
    const response = await apiHandle.get('https://api.example.com/users');
    console.log(response.data);
  } catch (error) {
    console.error('Error fetching users:', error.message);
  }
};

// POST request with data
const createUser = async (userData) => {
  try {
    const response = await apiHandle.post('https://api.example.com/users', userData);
    console.log('User created:', response.data);
  } catch (error) {
    console.error('Error creating user:', error.message);
  }
};
```

## API Reference

### Making Requests

```javascript
// GET request
apiHandle.get(url, config);

// POST request
apiHandle.post(url, data, config);

// PUT request
apiHandle.put(url, data, config);

// PATCH request
apiHandle.patch(url, data, config);

// DELETE request
apiHandle.delete(url, config);
```

### Configuration Options

```javascript
const config = {
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token'
  },
  timeout: 5000, // Request timeout in milliseconds
  cache: true, // Enable response caching
  cacheTime: 60000, // Cache duration in milliseconds
  retries: 3, // Number of retry attempts for failed requests
  onUploadProgress: (progressEvent) => {
    // Handle upload progress
  },
  onDownloadProgress: (progressEvent) => {
    // Handle download progress
  }
};
```

## Advanced Usage

### Request Cancellation

```javascript
const source = apiHandle.CancelToken.source();

apiHandle.get('/users', {
  cancelToken: source.token
});

// Cancel the request
source.cancel('Request canceled by user');
```

### Response Caching

```javascript
// Enable caching for a specific request
const getUsers = async () => {
  const response = await apiHandle.get('/users', {
    cache: true,
    cacheTime: 300000 // Cache for 5 minutes
  });
  return response.data;
};
```

### Interceptors

```javascript
// Request interceptor
apiHandle.interceptors.request.use(
  (config) => {
    // Modify config before request is sent
    config.headers.Authorization = `Bearer ${localStorage.getItem('token')}`;
    return config;
  },
  (error) => {
    // Handle request error
    return Promise.reject(error);
  }
);

// Response interceptor
apiHandle.interceptors.response.use(
  (response) => {
    // Any status code within the range of 2xx
    return response;
  },
  (error) => {
    // Handle specific HTTP errors
    if (error.status === 401) {
      // Handle unauthorized error
    }
    return Promise.reject(error);
  }
);
```

## Error Handling

```javascript
try {
  const response = await apiHandle.get('/users');
  // Handle successful response
} catch (error) {
  if (error.isCancel) {
    console.log('Request canceled:', error.message);
  } else if (error.isTimeout) {
    console.log('Request timed out');
  } else if (error.isNetwork) {
    console.log('Network error');
  } else {
    console.log('Error:', error.message);
    console.log('Status:', error.status);
    console.log('Response:', error.response);
  }
}
```

## Creating Instances

```javascript
const api = apiHandle.create({
  baseURL: 'https://api.example.com',
  headers: {
    'Content-Type': 'application/json'
  },
  timeout: 10000
});

// Use the instance
api.get('/users');
```

## TypeScript Support

```typescript
import apiHandle, { ApiResponse, ApiConfig } from 'api-handle';

interface User {
  id: number;
  name: string;
  email: string;
}

const fetchUsers = async (): Promise<User[]> => {
  const response: ApiResponse<User[]> = await apiHandle.get<User[]>('/users');
  return response.data;
};
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Author

- **Anas Raheem** - [anasraheemdev](https://github.com/anasraheemdev)

---

Made with ❤️ by [Anas Raheem](https://github.com/anasraheemdev)
