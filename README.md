## Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.

# Notes:

## Chapter 3

### Why optimize fonts?

Cumulative Layout Shift (CLS) is a metric used by Google to evaluate the performance and user experience of a website.
With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.
Next.js automatically optimizes fonts in the application when you use the next/font module. It downloads font files at build time and hosts them with your other static assets.

Tailwind class I didn't know before: `antialiased` smooths out the font.

### Why optimize images?

The <Image> Component comes with automatic image optimization, such as:

- Preventing layout shift automatically when images are loading.
- Resizing images to avoid shipping large images to devices with a smaller viewport.
- Lazy loading images by default (images load as they enter the viewport).
- Serving images in modern formats, like WebP and AVIF, when the browser supports it.

## Chapter 4: Creating Layouts and Pages

One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering:

## Chapter 5: Navigating Between Pages

### Why optimize navigation?

When use the `<a>` HTML element there's a full page refresh on each page navigation!

To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated (aislado). If a certain page throws an error, the rest of the application will still work.

## Chapter 6: Setting Up Your Database

## Chapter 7: Fetching Data

Benefits of fetching data on server components:

- Server Components support promises, providing a simpler solution for asynchronous tasks like data fetching. You can use async/await syntax without reaching out for useEffect, useState or data fetching libraries.
- Server Components execute on the server, so you can keep expensive data fetches and logic on the server and only send the result to the client.
- As mentioned before, since Server Components execute on the server, you can query the database directly without an additional API layer.

There are a few reasons why we'll be using SQL:

- SQL is the industry standard for querying relational databases (e.g. ORMs generate SQL under the hood).
- Having a basic understanding of SQL can help you understand the fundamentals of relational databases, allowing you to apply your knowledge to other tools.
- SQL is versatile, allowing you to fetch and manipulate specific data.
- The Vercel Postgres SDK provides protection against SQL injections.

There are two things you need to be aware of:

1. The data requests are unintentionally blocking each other, creating a request waterfall.
2. By default, Next.js prerenders routes to improve performance, this is called Static Rendering. So if your data changes, it won't be reflected in your dashboard.

### What are request waterfalls?

A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data.

This pattern is not necessarily bad. There may be cases where you want waterfalls because you want a condition to be satisfied before you make the next request.

**Parallel data fetching**

In JavaScript, you can use the Promise.all() or Promise.allSettled() functions to initiate all promises at the same time

## Chapter 8: Static and Dynamic Rendering

### What is Static Rendering?

With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or during **revalidation**. The result can then be distributed and cached in a Content Delivery Network (CDN).

Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page. It might not be a good fit for a dashboard that has personalized data that is regularly updated.

**Benefits of static rendering:**

- Faster Websites - Prerendered content can be cached and globally distributed. This ensures that users around the world can access your website's content more quickly and reliably.
- Reduced Server Load - Because the content is cached, your server does not have to dynamically generate content for each user request.
- SEO - Prerendered content is easier for search engine crawlers to index, as the content is already available when the page loads. This can lead to improved search engine rankings.

### What is Dynamic Rendering?

With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page).

**Benefits of dynamic rendering:**

- Real-Time Data - Dynamic rendering allows your application to display real-time or frequently updated data. This is ideal for applications where data changes often.
- User-Specific Content - It's easier to serve personalized content, such as dashboards or user profiles, and update the data based on user interaction.
- Request Time Information - Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL search parameters.

## Chapter 9: Streaming

### What is Streaming?

Streaming is a data transfer technique that allows you to break down a route into smaller _"chunks"_ (trozos) and progressively stream them from the server to the client as they become ready.

There are two ways you implement streaming in Next.js:

1. At the page level, with the loading.tsx file.
2. For specific components, with `<Suspense>`.

_**Route Groups**_ allow you to organize files into logical groups without affecting the URL path structure. When you create a new folder using parentheses `()`, the name won't be included in the URL path. So `/dashboard/(overview)/page.tsx` becomes `/dashboard`. You can also use route groups to separate your application into sections (e.g. (marketing) routes and (shop) routes) or **by teams for larger applications**.

## Chapter 11: Adding Search and Pagination

### Why use URL search params?

There are a couple of benefits of implementing search with URL params:

- Bookmarkable and Shareable URLs: Since the search parameters are in the URL, users can bookmark the current state of the application, including their search queries and filters, for future reference or sharing.
- Server-Side Rendering and Initial Load: URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering.
- Analytics and Tracking: Having search queries and filters directly in the URL makes it easier to track user behavior without requiring additional client-side logic.

These are the Next.js client hooks that you'll use to implement the search functionality:

1. **useSearchParams** - Allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`.
2. **usePathname** - Lets you read the current URL's pathname. For example, for the route `/dashboard/invoices`, usePathname would return '/dashboard/invoices'.
3. **useRouter** - Enables navigation between routes within client components programmatically. There are multiple methods you can use.

### What is debouncing?

Debouncing is a programming practice that limits the rate at which a function can fire. In our case, you only want to query the database when the user has stopped typing.

**How Debouncing Works:**

- Trigger Event: When an event that should be debounced (like a keystroke in the search box) occurs, a timer starts.
- Wait: If a new event occurs before the timer expires, the timer is reset.
- Execution: If the timer reaches the end of its countdown, the debounced function is executed.

## Chapter 12: Mutating Data

Behind the scenes, Server Actions create a POST API endpoint. This is why you don't need to create API endpoints manually when using Server Actions.

#### Storing values in cents

It's usually good practice to store monetary values in cents in your database to eliminate JavaScript floating-point errors and ensure greater accuracy.

### `revalidatePath`

Next.js has a Client-side Router Cache that stores the route segments in the user's browser for a time. Along with prefetching, this cache ensures that users can quickly navigate between routes while reducing the number of requests made to the server.

Since you're updating the data displayed in the invoices route, you want to clear this cache and trigger a new request to the server. You can do this with the `revalidatePath` function from Next.js:

## Chapter 13: Handling errors

## Chapter 14: Improving Accessibility

### What is accessibility?

Accessibility refers to designing and implementing web applications that everyone can use, including those with disabilities. It's a vast topic that covers many areas, such as keyboard navigation, semantic HTML, images, colors, videos, etc.

Next.js includes the `eslint-plugin-jsx-a11y` plugin to help catch accessibility issues early

```json
"scripts": {
    "build": "next build",
    "dev": "next dev",
    "seed": "node -r dotenv/config ./scripts/seed.js",
    "start": "next start",
    "lint": "next lint"
},
```

```terminal
npm run lint
✔ No ESLint warnings or errors
```

### Improving form accesibility

There are three things we're already doing to improve accessibility in our forms:

- **Semantic HTML:** Using semantic elements (`<input>`, `<option>`, etc) instead of `<div>`. This allows Assistive Technologies (AT) to focus on the input elements and provide appropriate contextual information to the user, making the form easier to navigate and understand.
- **Labelling:** Including `<label>` and the htmlFor attribute ensures that each form field has a descriptive text label. This improves AT support by providing context and also enhances usability by allowing users to click on the label to focus on the corresponding input field.
- **Focus Outline:** The fields are properly styled to show an outline when they are in focus. This is critical for accessibility as it visually indicates the active element on the page, helping both keyboard and screen reader users to understand where they are on the form. You can verify this by pressing tab.

## Chapter 15: Adding Authentication

The advantage of employing Middleware for this task is that the protected routes will not even start rendering until the Middleware verifies the authentication, enhancing both the security and performance of your application.
