# 1. Interface vs. Type
In Next.js/React, these are often interchangeable, but there is a standard convention.

## interface: 
Use this for defining object shapes (like Props, State, or Data models). They are better for error messages and can be extended.

## type: 
Use this for Unions, Primitives, Functions, or complex combinations.

How to write & use:
```Typescript

// ✅ Use Interface for Props
interface ButtonProps {
  label: string;
  isActive?: boolean; // Optional property
  onClick: () => void;
}
```

```Typescript
// ✅ Use Type for Unions or fixed string values
type ButtonVariant = 'primary' | 'secondary' | 'danger';
type Status = "pending" | "success" | "failed"
type ID = string | number

```
```Typescript
interface ExtendedButtonProps extends ButtonProps {
  variant: ButtonVariant;
}
```
# 2. Typing Components (Props)
You don't need to use React.FC anymore. It is cleaner to type the props directly in the function arguments.


## Standard Component:
```Typescript

interface UserCardProps {
  name: string;
  age: number;
  role: 'admin' | 'guest';
}

// Destructure props directly
export default function UserCard({ name, age, role }: UserCardProps) {
  return <div>{name} is an {role}</div>;
}
```
## Server Component props

```Typescript
interface PageProps {
  params: { id: string }
  searchParams: { q?: string }
}

export default function Page({ params, searchParams }: PageProps) {
  return <div>User ID: {params.id}</div>
}
```

## Component with children:
If your component wraps other elements (like a Layout or Card), use React.ReactNode.

```Typescript

import { ReactNode } from 'react';

interface LayoutProps {
  children: ReactNode; // Accepts JSX, strings, null, etc.
}

export default function MainLayout({ children }: LayoutProps) {
  return <main className="p-4">{children}</main>;
}
```

# 3. Typing Next.js Special Files (App Router)
Next.js 15+ has made params and searchParams asynchronous. You must type them as Promise.

## Pages (page.tsx)
For a route like /products/[id]:

```Typescript

// Types for Dynamic Routes
interface PageProps {
  params: Promise<{ id: string }>;
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>;
}

export default async function ProductPage({ params, searchParams }: PageProps) {
  // You must await them in Next.js 15+
  const { id } = await params;
  const query = await searchParams;

  return <h1>Product ID: {id}</h1>;
}
```

## Dynamic route params
```bash
app/user/[id]/page.tsx
```

```Typescript
interface PageProps {
  params: { id: string }
}

export default function Page({ params }: PageProps) {
  return <div>{params.id}</div>
}
```
## Nested params
```bash
app/blog/[category]/[slug]/page.tsx
```
```Typescript
type BlogParams = {
  params: {
    category: string
    slug: string
  }
}
```

##  Typing Search Params
```graphql
?query=nextjs&page=1
```
```Typescript
interface PageProps {
  searchParams: {
    query?: string
    page?: string
  }
}
```
## Layouts (layout.tsx)
Layouts strictly receive children and sometimes params.

```Typescript

import { ReactNode } from 'react';

interface DashboardLayoutProps {
  children: ReactNode;
  params: Promise<{ teamId: string }>; // If layout is inside a dynamic route
}

export default async function DashboardLayout({ children, params }: DashboardLayoutProps) {
  return <section>{children}</section>;
}
```
# 4. Functions & Event Handlers
Typing events correctly allows you to access properties like e.target.value without TypeScript yelling at you.

## Event Handlers:
Common types: React.ChangeEvent, React.FormEvent, React.MouseEvent.

```Typescript

'use client'; // Event handlers only work in Client Components

import { useState, ChangeEvent, FormEvent } from 'react';

export default function SignupForm() {
  const [email, setEmail] = useState('');

  // 1. Typing an Input Change
  // HTMLInputElement allows access to 'value', 'checked', etc.
  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    setEmail(e.target.value);
  };

  // 2. Typing a Form Submit
  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log('Submitting:', email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```
## Async function in API or server

```Typescript
async function getUser(id: string): Promise<User> {
  const res = await fetch(`/api/user/${id}`)
  return res.json()
}
```
# 5. API Routes (Route Handlers)
In App Router (route.ts), you use NextRequest and NextResponse instead of the old req, res.

```Typescript

import { NextRequest, NextResponse } from 'next/server';

// Handle GET requests
export async function GET(request: NextRequest) {
  // Get search params from URL
  const searchParams = request.nextUrl.searchParams;
  const query = searchParams.get('query');

  return NextResponse.json({ message: `You searched for ${query}` });
}

// Handle POST requests
export async function POST(request: NextRequest) {
  // Typing the body data
  interface BodyData {
    username: string;
  }
  
  const body: BodyData = await request.json();
  
  return NextResponse.json({ status: 'User created', user: body.username });
}
```
# 6. Server Actions (Forms)
When using Server Actions (functions that run on the server), type the FormData input.

```Typescript

// actions.ts
'use server';

export async function createPost(formData: FormData) {
    // formData.get returns 'FormDataEntryValue | null'
    // We usually cast it to string if we are sure it exists
    const title = formData.get('title') as string;
    const content = formData.get('content') as string;

    if (!title) throw new Error("Title is required");

    console.log('Saved:', title);
}
```

# 7. Typing Next.js Metadata
In the App Router, you define SEO (Metadata) in layout.tsx or page.tsx. Next.js provides a built-in type for this.

## Static Metadata
Use the Metadata type import from next.
```Typescript
import type { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'My Dashboard',
  description: 'Analytics and reports',
  openGraph: {
    images: ['/og-image.png'],
  },
};

export default function Page() {
  return <h1>Dashboard</h1>;
}
```
## Dynamic Metadata
If your page title depends on dynamic params (e.g., a product name), use generateMetadata.

```Typescript

import type { Metadata, ResolvingMetadata } from 'next';

interface Props {
  params: Promise<{ id: string }>;
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>;
}

// Use the same Props type as your Page component
export async function generateMetadata(
  { params }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  
  const { id } = await params;

  // Fetch data to get the dynamic title
  const product = await fetch(`https://api.example.com/products/${id}`).then((res) => res.json());

  return {
    title: product.name,
    description: `Buy ${product.name} now`,
  };
}

export default function Page({ params }: Props) {
   // Page content...
}

```
# 8. Typing Hooks
TypeScript can often infer types (like useState(0) is clearly a number), but you need explicit types for objects, arrays, or null values.
## useState
Use generics `<Type>` when the initial state is complex or might be null.
```Typescript
'use client';

import { useState } from 'react';

interface User {
  id: number;
  name: string;
}

export default function Profile() {
  // 1. Simple Inference (No extra typing needed)
  const [count, setCount] = useState(0);

  // 2. Object or Null (Common for fetching data)
  // User | null means it starts empty but will eventually hold a User
  const [user, setUser] = useState<User | null>(null);

  // 3. Arrays
  const [tags, setTags] = useState<string[]>([]);
}
```

## useRef
This is commonly used for accessing DOM elements (like inputs).
```Typescript
import { useRef, useEffect } from 'react';

export default function Search() {
  // Initialize with null because the DOM element doesn't exist on first render
  const inputRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    // Optional chaining (?) is needed because current might be null
    inputRef.current?.focus();
  }, []);

  return <input ref={inputRef} />;
}
```
## useReducer
Use a discriminated union for actions to ensure type safety in your reducer switch cases.

```Typescript
import { useReducer } from 'react';

// Define the State
interface State {
  count: number;
}

// Define Actions (Discriminated Union)
type Action = 
  | { type: 'increment' } 
  | { type: 'decrement' } 
  | { type: 'reset'; payload: number }; // Action with payload

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment': 
      return { count: state.count + 1 };
    case 'decrement': 
      return { count: state.count - 1 };
    case 'reset': 
      return { count: action.payload };
    default: 
      return state;
  }
}

```
## Custom Hook

Here is how to create a reusable Custom Hook for data fetching.

The key to making a custom hook flexible in TypeScript is using Generics (`<T>`). This allows the hook to work with any data shape (Users, Products, Posts) without rewriting the code.

## 1. The Custom Hook: useFetch
Create a file named useFetch.ts (usually in a /hooks folder).
```Typescript
import { useState, useEffect } from 'react';

// 1. Define the return type of the hook
interface FetchResult<T> {
  data: T | null;
  isLoading: boolean;
  error: string | null;
  refetch: () => void; // Useful feature to reload data manually
}

// 2. Use <T> to make the hook generic (it doesn't know what T is yet)
export function useFetch<T>(url: string): FetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [isLoading, setIsLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);
  const [trigger, setTrigger] = useState(0); // Used to force a re-fetch

  useEffect(() => {
    const controller = new AbortController(); // Prevents memory leaks if component unmounts

    const fetchData = async () => {
      setIsLoading(true);
      setError(null);

      try {
        const response = await fetch(url, { signal: controller.signal });

        if (!response.ok) {
          throw new Error(`Error: ${response.statusText}`);
        }

        // 3. Cast the JSON result to T
        const json: T = await response.json();
        setData(json);
      } catch (err: any) {
        if (err.name !== 'AbortError') {
          setError(err.message || 'Something went wrong');
        }
      } finally {
        setIsLoading(false);
      }
    };

    fetchData();

    // Cleanup function
    return () => controller.abort();
  }, [url, trigger]); // Re-run if URL or trigger changes

  const refetch = () => setTrigger((prev) => prev + 1);

  return { data, isLoading, error, refetch };
}
```

## 2. How to Use It in a Component
Now, you pass your specific interface (e.g., User) into the hook when you call it.

```Typescript
'use client';

import { useFetch } from '@/hooks/useFetch';

// 1. Define the specific shape of data you expect
interface User {
  id: number;
  name: string;
  email: string;
}

export default function UserProfile() {
  // 2. Pass <User[]> to the hook. 
  // Now 'data' is typed as 'User[] | null' automatically.
  const { data, isLoading, error, refetch } = useFetch<User[]>('https://jsonplaceholder.typicode.com/users');

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <button onClick={refetch} className="mb-4 bg-blue-500 text-white p-2 rounded">
        Reload Users
      </button>

      <ul>
        {/* TypeScript knows 'user' has .id and .name */}
        {data?.map((user) => (
          <li key={user.id}>
            {user.name} ({user.email})
          </li>
        ))}
      </ul>
    </div>
  );
}
```
## Key TypeScript Concepts Used Here:
## 1. Generics (`<T>`):

Think of T as a placeholder variable.

When you write `useFetch<User[]>`, TypeScript replaces every instance of T inside the hook with User[].

## 2.Return Type `(FetchResult<T>)`:

This explicitly tells TypeScript that if I ask for a User, the data property returned will be User.

## 3. State Typing:

`useState<T | null>(null)` ensures the data state can handle both the initial empty state and the eventual populated data.
# 9. Typing API Fetchers
When fetching data (using fetch or libraries like Axios/TanStack Query), you never truly know what the server returns. You must define the expected shape manually.

## Standard Fetch Function
Always separate the API Response shape from the Data model.
```Typescript
// 1. Define what the API returns
interface Product {
  id: string;
  name: string;
  price: number;
}

interface ApiResponse {
  success: boolean;
  data: Product[];
}

// 2. Create the fetcher function
async function getProducts(): Promise<ApiResponse> {
  const res = await fetch('https://api.example.com/products');
  
  if (!res.ok) {
    throw new Error('Failed to fetch data');
  }

  // TypeScript now trusts that 'data' matches the ApiResponse interface
  const data: ApiResponse = await res.json(); 
  return data;
}

// 3. Usage in a Server Component
export default async function ProductList() {
  const response = await getProducts();
  
  return (
    <ul>
      {response.data.map((product) => (
        <li key={product.id}>{product.name}</li>
      ))}
    </ul>
  );
}
```