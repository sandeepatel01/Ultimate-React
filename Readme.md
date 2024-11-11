# Ultimate React & Next.JS

---

## **Next.js**

---

**Next.js** is a popular open-source **React framework** that enables developers to build server-side rendering (SSR) and static web applications with ease. 

- Developed and maintained by **Vercel.**
- it offers many powerful features that make it ideal for building modern web applications.

### Key Features:

---

1. **Server-Side Rendering (SSR)**:Enables rendering of pages on the server, providing better performance and SEO compared to client-side rendering.
2. **Static Site Generation (SSG)**:  Allows for generating static HTML at build time, making sites faster and more efficient.
3. **Hybrid Rendering**: Supports a mix of SSR, SSG, and client-side rendering within the same application.
4. **Routing System**: Provides a file-based routing system where routes are automatically created based on the `pages` directory structure.
5. **Image Optimization**: Built-in image component (`next/image`) that optimizes images for performance automatically.
6. **Internationalized Routing**: Supports multilingual websites with built-in routing and translations.
7. **Incremental Static Regeneration (ISR)**: Updates static content after deployment without needing a full rebuild.
8. **Automatic Code Splitting**: Only the necessary JavaScript for a particular page is loaded, improving performance.
9. **Full Stack Capabilities**: Can handle both frontend and backend logic, making it a full-stack framework.
10. **API Routes**: Allows you to build backend functionality and APIs as part of the Next.js app using serverless functions.

### Benefits of Using Next.js:

---

- **Improved Performance**: Thanks to server-side rendering and static site generation.
- **SEO Friendliness**: Server-rendered content makes it easier for search engines to crawl and index pages.
- **Developer Experience**: Simple setup, built-in features, and extensive documentation.
- **Fast Refresh**: Development is seamless with automatic updates when you change code.

## **Setting up NextJS Project**

---

### Getting Started

---

1. **Install Node.js (If Not Already Installed)**
Ensure you have **Node.js** installed, as Next.js requires it. You can download it from [nodejs.org](https://nodejs.org/). This will also install **npm** (Node Package Manager).
2. Create a New Next.js App

        To start a Next.js project, you can use the following command:

```tsx
npx create-next-app@latest my-next-app
```

- Replace `my-next-app` with your desired project name.
1. Navigate to Your Project Directory

```tsx
cd my-next-app
```

1. **Run the Development Server**

```tsx
npm run dev
```

1. **Project Structure Overview**

Your new Next.js project will include the following structure:

```tsx
my-next-app/
├── pages/                # Routes and pages
│   ├── _app.js           # Custom app component
│   ├── index.js          # Home page (default route)
│   └── api/              # API routes
├── public/               # Static assets (images, etc.)
├── styles/               # CSS/SCSS files
│   ├── globals.css       # Global styles
├── package.json          # Project dependencies and scripts
└── next.config.js        # Next.js configuration file (optional)
```

1. Install Additional Packages (Optional)

Depending on your project requirements, you might need additional packages. Install them using:

```tsx
npm install package-name
```

1. Build and Export for Production

When your project is ready for deployment, build it with:

```tsx
npm run build
```

## **Routing in NextJS**

---

Routing in **Next.js** is based on the file and folder structure within the `pages` directory.

Each file automatically becomes a route, making it simple to set up and maintain.

### 1. Basic Routing

---

**File-Based Routing**: Any JavaScript or TypeScript file in the `pages` directory automatically becomes a route.

```tsx
pages/
├── index.js          // Accessible at "/"
├── about.js          // Accessible at "/about"
└── contact.js        // Accessible at "/contact"
```

### 2. Nested Routes

---

You can create subdirectories in `pages` to create nested routes.

```tsx
pages/
├── blog/
│   ├── index.js      // Accessible at "/blog"
│   └── [slug].js     // Dynamic route, accessible at "/blog/some-article"
├── products/
│   └── details.js    // Accessible at "/products/details"
```

### 3. Dynamic Routing

---

- Dynamic routes are created using **square brackets** (`[param]`).
- For instance, `pages/blog/[slug].js` would match routes like `/blog/my-first-post`.
- To access the `slug` parameter, use the `useRouter` hook from `next/router`:

```tsx
import { useRouter } from 'next/router';

const BlogPost = () => {
  const router = useRouter();
  const { slug } = router.query;

  return <div>Blog Post: {slug}</div>;
};

export default BlogPost;
```

### 4. Catch-All Routes

---

- Catch-all routes are created using `[...param]`. They capture all matching segments.
- Example: `pages/docs/[...slug].js` matches `/docs`, `/docs/introduction`, `/docs/guide/setup`, etc.
- To handle a catch-all route, check the `slug` array:

```tsx
const Docs = () => {
  const router = useRouter();
  const { slug } = router.query;

  return <div>Path segments: {slug ? slug.join('/') : 'Home'}</div>;
};

export default Docs;
```

### 5. Custom 404 Page

---

Create a `404.js` file in the `pages` directory to customize the 404 error page.

```tsx
const Custom404 = () => {
  return <h1>404 - Page Not Found</h1>;
};

export default Custom404;
```

### 6. API Routes

---

Next.js supports building serverless API endpoints within the `pages/api` directory.
Example: `pages/api/hello.js`

```tsx
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello World' });
}
```

Access this route at `/api/hello`.

### 7. Linking Between Pages

---

Use the built-in `Link` component from `next/link` to navigate between pages without a full page refresh.

```tsx
import Link from 'next/link';

const HomePage = () => (
  <div>
    <h1>Welcome to my site</h1>
    <Link href="/about">
      <a>Go to About Page</a>
    </Link>
  </div>
);
```

## **Client Side Data Fetching in NextJS**

---

Client-side data fetching in Next.js is an effective way to fetch data after the page has been loaded in the browser.

This approach is useful for dynamic data that changes frequently or for data that should be fetched based on user interaction. 

### 1. Using `useEffect` and `fetch`

---

The most straightforward way to fetch data on the client side is by using the `useEffect` hook in combination with the native `fetch` API or any HTTP client (e.g., `axios`).

```tsx
import { useEffect, useState } from 'react';

const ClientSidePage = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Fetch data from an API endpoint
    fetch('/api/data')
      .then((response) => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        return response.json();
      })
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch((error) => {
        setError(error);
        setLoading(false);
      });
  }, []); // The empty dependency array ensures this runs only once after the initial render

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <div>
      <h1>Client Side Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default ClientSidePage;

```

### 2. Fetching Data with `axios`

---

You can also use `axios`, which provides additional features such as request interceptors and automatic JSON transformation.

```tsx
import { useEffect, useState } from 'react';
import axios from 'axios';

const ClientSidePage = () => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    axios.get('/api/data')
      .then((response) => {
        setData(response.data);
        setLoading(false);
      })
      .catch((error) => {
        setError(error);
        setLoading(false);
      });
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <div>
      <h1>Client Side Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default ClientSidePage;

```

### 3. Using `SWR` for Data Fetching

---

**SWR** (stale-while-revalidate) is a React hook library created by Vercel for data fetching. It provides built-in caching, revalidation, and error handling.
**Installation**:

```tsx
npm install swr
```

**Usage**:

```tsx
import useSWR from 'swr';

const fetcher = (url) => fetch(url).then((res) => res.json());

const ClientSidePage = () => {
  const { data, error } = useSWR('/api/data', fetcher);

  if (error) return <p>Error: {error.message}</p>;
  if (!data) return <p>Loading...</p>;

  return (
    <div>
      <h1>Client Side Data with SWR</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default ClientSidePage;

```

### **Benefits of SWR**:

---

- **Caching**: Data is cached and reused.
- **Revalidation**: Automatically re-fetches data in the background.
- **Error Handling**: Easy to manage with built-in support.
- **Optimistic UI**: Allows updating the UI optimistically.

### **Best Practices for Client-Side Data Fetching**:

---

- **Avoid Overfetching**: Only fetch data that is necessary for the current page or component.
- **Handle Loading and Errors**: Always show a loading state and handle errors gracefully.
- **Data Caching**: Use a library like **SWR** or **React Query** for better caching and revalidation.
- **Environment Variables**: Store sensitive API keys in environment variables and access them securely (ensure that client-side code does not expose secrets).

Using Environment Variables:

```tsx
useEffect(() => {
  const API_URL = process.env.NEXT_PUBLIC_API_URL; // Use NEXT_PUBLIC_ prefix for client-side

  fetch(`${API_URL}/data`)
    .then((response) => response.json())
    .then((data) => setData(data))
    .catch((error) => console.error('Error fetching data:', error));
}, []);
```

### **Server Side Data Fetching in NextJS**

---

Server-side data fetching in **Next.js** allows you to pre-render pages with data fetched from an external source at request time. This helps in creating fast and SEO-friendly pages with fresh content.

Next.js offers two main functions for server-side data fetching:

1. **`getServerSideProps`**
2. **API routes**

### Using `getServerSideProps`

---

`getServerSideProps` is a Next.js function that runs only on the server side. 

 It is used to fetch data before rendering a page, making the content available at request time.

Basic Example of `getServerSideProps`:

```tsx
export async function getServerSideProps(context) {
  // Fetch data from an external API or database
  const res = await fetch('https://api.example.com/data');
  const data = await res.json();

  // Pass data to the page component as props
  return {
    props: {
      data,
    },
  };
}

const ServerSidePage = ({ data }) => {
  return (
    <div>
      <h1>Server-Side Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
};

export default ServerSidePage;

```

### **How `getServerSideProps` Works**

---

- **Runs at Request Time**: `getServerSideProps` runs at every request, meaning data is always up-to-date but adds server response time to the overall page load.
- **Context Parameter**: The `context` object provides details about the request, such as query parameters, cookies, and more