---
theme: seriph
class: text-center
highlighter: shiki
lineNumbers: true
transition: slide-left
---

# Next.js

## Innovation or Over‑Engineering?

Is it complexity or just React evolution?

---

# The Golden Era

- Mental Model: **“It’s just JavaScript.”**

<br/>

<v-click>
<h2>Everywhere.</h2>
</v-click>

<br/>

<v-click>
<ul>
  <li>Linters? JavaScript.</li>
  <li>Build tools? JavaScript.</li>
  <li>Compilers/Transpilers? Also JavaScript... for some reason.</li>
  <li>useEffects? Everywhere.</li>
</ul>
</v-click>

<br/>

<v-click>
Packages to expose the character "a"? Only in JavaScript. <a href="https://www.npmjs.com/package/@characters/a">https://www.npmjs.com/package/@characters/a</a>
</v-click>

<br/>

<v-click>
<p>
Tiny mental model. Predictable lifecycle. Almost zero magic.
</p>
</v-click>

---

<h1  class="flex justify-center">Freeeeeedom!</h1>

<div class="flex justify-center">
  <img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExa3VvdDQ2bXE2YTFmenZ1M3lueWU1Ynl6eTR6emhzZG0xdDlhaW0xNyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/SCoDlK7kpAElHKvSp6/giphy.gif"  style="width: 400px"  />
</div>

---

# With Next.js Pages Router

- Mental Model: **React with a server**

<br/>

<v-click>
<ul>
  <li><code>/pages/index.js</code> → Becomes your homepage. Magic, but the *good* kind.</li>
  <li><code>getServerSideProps</code> → “I swear it’s just a function.”</li>
  <li><code>getStaticProps</code> → Time-traveling data. Build once, serve forever.</li>
  <li><code>getStaticPaths</code> → Because your blog definitely has 14,000 pages.</li>
</ul>
</v-click>

<br/>

<v-click>
The functions must be named exactly right, or the magic breaks.
</v-click>

<br/>

<v-click>
<p>
Simple conventions. ¿Minimal? surprise. ¿predictable?
</p>
</v-click>
<v-click>
  <img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExMmE2emNwczF4Y245M29ybTlwdnp6ODB6aWZ5YmhqOTBvampnYWN1eCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/FCzMUL20U7LkA/giphy.gif" style="width: 150px"  />
</v-click>
---

### `pages/index.js`

```js
export default function Home() {
  return <h1>Hello from 2016</h1>;
}
```

<br/>

### `pages/blog/[slug].js`

```js
export async function getStaticPaths() {
  return {
    paths: [{ params: { slug: "hello-world" } }],
    fallback: false,
  };
}

export async function getStaticProps({ params }) {
  return {
    props: { slug: params.slug },
  };
}

export default function Blog({ slug }) {
  return <h2>Blog post: {slug}</h2>;
}
```

---

# Simple, Yes… But Also…

### Data Fetching Soup

`getStaticProps` + `getStaticPaths` + `getServerSideProps`

Which one should I use?

<br />

### Bundles That Accidentally Bench-Press 40kg

Everything becomes client-side JavaScript unless you _carefully_ avoid it.
(_Spoiler: we did not carefully avoid it._)

<br/>

<v-click>
<p>
So yes, simple. But also... ¿accidentally chaotic?. Yet nextjs worked great for many use cases, but it needed to evolve.
</p>
</v-click>

---

# So... Where are we now?

<v-click>
<h2>
The PhD Era.
</h2>
<p>
How many PhDs in caching do I need to build a button?
</p>
</v-click>

<br />

<v-click>
<ul>
<li>Motto: "It's just JavaScript... maybe... sometimes"</li>
<li>Mental Model: Where does this code run? Only the lord knows.</li>
</ul>
</v-click>

<v-click>
<p>
Caption: Where am I running? What is cached? Who wrote this?
</p>
</v-click>

<v-click>
You want evolution so you meant over-engineering?
<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExNm1qMWZxaTh4bWx2bGl3d3lsMGUxbGxtZDYwMTZoanBndDcyOXptaiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/e5Ro17b1nX57pqbCo1/giphy.gif" style="width: 200px"  />
</v-click>

---

# With Next.js App Router

Everything is a Server Component... until it suddenly isn’t.

<v-click>
<ul>
  <li><code>app/</code> directory: Because we needed <em>one more</em> way to make pages.</li>
  <li><code>page.js</code>: Your new god. It wraps everything. Forever.</li>
  <li><code>loading.js</code>: Because your application needs a bit of suspense.</li>
  <li><code>error.js</code>: Because errors need their own special place.</li>
  <li><code>"use client", "use server"</code>: A threat, not a directive. The beginning of the end.</li>
</ul>
</v-click>

<v-click>
<p>
App Router simplifies things... by introducing 17 new concepts.
</p>
</v-click>

<v-click>
<img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExY3drNm5vMnhuaWNub2VjeXZkY2hvc3c0eTNqZm5nMHNneGdvN21sayZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/yAYZnhvY3fflS/giphy.gif" style="width: 300px" />
</v-click>

---

# App Router: To Be Fair... It is Simpler (Sometimes)

<v-click>

### File-based routing becomes _really_ clean

`app/dashboard/page.js`

```jsx
export default function Dashboard() {
  return <h1>Dashboard</h1>;
}
```

<p>One file = one route. Zero ceremony.</p>

</v-click>

<br/>

<v-click>

### Server Components make data fetching amazingly simple

```jsx
export default async function Page() {
  const data = await fetch("https://api.example.com").then((r) => r.json());
  return <pre>{JSON.stringify(data)}</pre>;
}
```

<p>No <code>getStaticProps</code>. No <code>getServerSideProps</code>.Just <em>async</em> components. Very nice, no reinvented wheel, just following the standards.</p>

</v-click>

---

### Nested layouts without losing your sanity

`app/layout.js`

```jsx
export default function RootLayout({ children }) {
  return (
    <html>
      <body>{children}</body>
    </html>
  );
}
```

<p>A layout system that doesn’t need a PhD in “_app.js gymnastics.”</p>

<v-click>
<p>
So even if Next.js App Router is more complex and has a bit of more magic, it simplifies many things and improves developer experience in many ways.
</p>
</v-click>

<v-click>
We have now a clear separation between Layouts, Pages, Templates, and Components. Everything has its place. Everything makes sense. Everything is great.
</v-click>

---

Then did we did not over-engineer right? Is just regular evolution right?

<v-click>
<img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExa3pjdWQ4YnRxYzd6bnViODdoYzZrMDBudW8wYnA0d29jejU3N2tnaCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/0sgkfvoaCONHNN0pLs/giphy.gif" style="width: 400px"  />
</v-click>

---

# Directives - Magic or Madness?

---

### `use cache`

Allows you to mark a route, React component, or a function as cacheable.

```jsx
// File level
"use cache";

export default async function Page() {
  // ...
}

// Component level
export async function MyComponent() {
  "use cache";
  return <></>;
}

// Function level
export async function getData() {
  "use cache";
  const data = await fetch("/api/data");
  return data;
}
```

---

### `use cache: private`

Works just like use cache, but allows you to use runtime APIs like cookies, headers, or search params.

```jsx
async function getRecommendations(productId: string) {
  "use cache: private";
  cacheTag(`recommendations-${productId}`);
  cacheLife({ stale: 60 });
 ...
}

export default async function ProductPage({
  params,
}: {
  params: Promise<{ id: string }>,
}) {
  const recommendations = await getRecommendations(productId);
  ....
}
```

---

### `use cache: remote`

Lets you declaratively specify that a cached output should be stored in a remote cache instead of in-memory.

```jsx
async function getProductsByCategory(category: string) {
  'use cache: remote'
  return db.products.findByCategory(category)
}

export default async function ProductsPage({
  params,
  searchParams,
}: {
  params: Promise<{ category: string }>
  searchParams: Promise<{ minPrice?: string }>
}) {
    const products = await getProductsByCategory(category)
  ...
}
```

---

### `use client`

Declares an entry point for the components to be rendered on the client side.

```jsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

---

## `use server`

Designates a function or file to be executed on the server side.

### `use server` at the top of a file

```jsx
"use server";
import { db } from "@/lib/db"; // Your database client

export async function fetchUsers() {
  const users = await db.user.findMany();
  return users;
}
```

### `use server` inline

```jsx
export default async function PostPage({ params }: { params: { id: string } }) {
  const post = await getPost(params.id);
  async function updatePost(formData: FormData) {
    "use server";
    await savePost(params.id, formData);
    revalidatePath(`/posts/${params.id}`);
  }
  return <EditPost updatePostAction={updatePost} post={post} />;
}
```

---

# React joins the party with Directives for the compiler

### `use memo`

```jsx
function MyComponent() {
  "use memo";
  // ...
}
```

Marks a function for React Compiler optimization.

### `use no memo`

```jsx
function MyComponent() {
  "use no memo";
  // ...
}
```

Prevents a function from being optimized by React Compiler.

---

# Others are also joining

TypeGPU https://docs.swmansion.com/TypeGPU/

### `use gpu`

```jsx
const neighborhood = (a: number, r: number) => {
  "use gpu";
  return d.vec2f(a - r, a + r);
};
```

Allows the function to be picked up by our dedicated build plugin (unplugin-typegpu) and transformed into a format TypeGPU can understand.

---

<div class="flex justify-center">
  <img src="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExb2w3MDhmaXQzem1rbXdycG1mY2U4MzhsZnBvang2cnl3anlpa2l0bCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/1Be4g2yeiJ1QfqaKvz/giphy.gif"  style="width: 600px"  />
</div>

---

# The "Simplicity" Trap

<v-click>
If modern Next.js feels like PHP... should we go back to PHP?
</v-click>
<br/><br/>
<v-click>
<b>No.</b> Because modern UX demands:
<ul>
<li>Instant transitions</li>
<li>Optimistic UI</li>
<li>Rich interactivity</li>
<li>Offline support</li>
<li>Zero loading states</li>
<li>Many more...</li>
</ul>
</v-click>
<br/>
<v-click>
The Reality is that complexity is conserved. You either handle it in the Framework (Next.js), or you handle it in your Own ¿Spagetti? Code.

The problem isn't complexity itself; it's Accidental Complexity vs. Essential Complexity. You need both but at what degree?
</v-click>

---

# Accidental vs. Essential Complexity

Some complexity is nature. Some is... our fault.

<v-click>
<h3>Essential Complexity</h3>
<p>
The real, unavoidable problems:<br/>
data fetching, routing, caching, rendering, performance.
</p>
</v-click>

<v-click>
<h3>Accidental Complexity</h3>
<br/>
The problems we created along the way:
<ul>
  <li>“Is this server or client?” — a daily existential crisis.</li>
  <li><code>"use client"</code> — the scarlet letter of modern React.</li>
  <li>RSC boundaries that feel like zoning laws.</li>
  <li>Caching rules that require a flowchart and emotional support.</li>
</ul>
</v-click>

<br/>

<v-click>
<p>
Next.js App Router tries to reduce essential complexity... but sometimes smuggles in a bit of accidental complexity on the side.
</p>
</v-click>

---

# When Complexity Breaks

Two major incidents in late 2025 proved that the mental model is becoming too fragile in both ways, either too much simplicity or too much complexity.

---

# "React2Shell" (CVE‑2025‑55182)

The push for Server Components blurred the line between backend and frontend, introducing a new attack vector.

### The Vulnerability:

- Mechanism: Unsafe deserialization in the React Flight protocol.
- The Attack: Attackers crafted malicious payloads sent to RSC endpoints.
- Result: Remote Code Execution (RCE) on the rendering server.

<br/>

### Why This Matters

In the "Old React" (Client-side), an RCE was impossible because the code ran in the user's browser.
By moving React to the server, we inherited server-side security risks, often without the security training required to handle them.

---

# Cloudflare Outage

A massive outage wasn’t caused by hackers... **but by a React Hook.**

### The Anatomy of the Failure:

- The Code: A dashboard component with a complex useEffect.
- The Bug: A subtle dependency array mistake created an infinite re-render loop.
- The Impact: The frontend inadvertently DDoS'd Cloudflare's internal Tenant Service API.

<br/>

### The Lesson

<v-click>
A UI bug can now take down backend infrastructure.
</v-click>

<v-click>
<p>
Frontend devs:
</p>
<img src="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExb2IwOGU3OGpmdngwYWMzY216Y3BoMTJrY3A5NmZtMmQ4bnhsODFnNiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/1vRCeaHbgATwA/giphy.gif" style="width: 200px"/>
</v-click>

---

# So… Innovation or Over‑Engineering?

<br/>

### Innovation: **Yes**

- Compiler → Rust-powered, lightning-fast builds
- Streaming → React Server Components, live from the server
- Server Components → Zero-bundle magic where it counts
- 0kb client bundles → Less JS, more performance

<br/>

### Over‑Engineering: **Also yes**

- Dev mental model exploding → “Is this running on the server or client?”
- Server security risks → Server Components = new attack surface
- Steep learning curves → directives, RSC boundaries, caching rules

<br/>

<v-click>
<p>
Innovation brings power… and existential dread.
<br/>
Next.js now is simpler in some ways, more magical (and confusing) in others.
</p>
</v-click>

---

# Final Thought

**Complexity is a budget.** Spend it where it buys **user value**, not where it buys **developer headaches**.

### The Ferrari theory

If nextjs is a Ferrari and you only need to go to the grocery store, maybe a honda civic is a better fit.

Alternatives:

- [Tanstack router](https://tanstack.com/router/latest/docs/framework/react/overview)
- [Tanstack start](https://tanstack.com/start/latest/docs/framework/react/overview)

<br/>
Lastly...
<v-click>
<p>
If Next.js becomes any more server-heavy… can we just call it <b>NextPHP.js</b>?
</p>
</v-click>

---

# Thank you

You may applaud now.
