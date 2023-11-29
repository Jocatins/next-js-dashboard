## Next.js App Router Course - Starter

This is the starter template for the Next.js App Router Course. It contains the starting code for the dashboard application.

/app: Contains all the routes, components, and logic for your application
/app/lib: Contains functions used in your application, such as reusable utility functions and data fetching functions.
/app/ui: Contains all the UI components for your application, such as cards, tables, and forms. To save time, we've pre-styled these components for you.
/public: Contains all the static assets for your application, such as images

`clsx` is a library that lets you toggle class names easily.

`Cumulative Layout Shift` is a metric used by Google to evaluate the performance and user experience of a website. With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.

Next.js automatically optimizes fonts in the application when you use the next/font module. It downloads font files at build time and hosts them with your other static assets. This means when a user visits your application, there are no additional network requests for fonts which would impact performance.

`antialiased` class smooths out the font. It's not necessary to use this class, but it adds a nice touch

`Optimizing images`
you can use the next/image component to automatically optimize your images.

One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called `partial rendering:`

##### Automatic code-splitting and prefetching

To improve the navigation experience, Next.js `automatically code splits `your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

Futhermore, in production, whenever <Link> components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

### Pattern: Showing active links

`A common UI pattern` is to show an active link to indicate to the user what page they are currently on. To do this, you need to get the user's current path from the URL. Next.js provides a hook called `usePathname() `that you can use to check the path and implement this pattern.

Since `usePathname()` is a hook, you'll need to turn nav-links.tsx into a Client Component.

### Populating the database `troubleshooting`

- Make sure to reveal your database secrets before copying it into your .env file.
- The script uses bcrypt to hash the user's password, if bcrypt isn't compatible with your environment, you can update the script to use bcryptjs instead.
- If you run into any issues while seeding your database and want to run the script again, you can drop any existing tables by running DROP TABLE tablename in your database query interface. See the executing queries section below for more details. But be careful, this command will delete the tables and all their data. It's ok to do this with your example app since you're working with placeholder data, but you shouldn't run this command in a production app.

In Next.js, you can create API endpoints using Route Handlers.

There are a few cases where you have to write database queries:

- When creating your API endpoints, you need to write logic to interact with your database.
- If you are using React Server Components (fetching data on the server), you can skip the API layer, and query your database directly without risking exposing your database secrets to the client.

#### What are request waterfalls?

A "`waterfall`" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data.

we need to wait for fetchRevenue() to execute before fetchLatestInvoices() can start running, and so on.

#### Parallel data fetching

---

A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.

In JavaScript, you can use the Promise.all() or Promise.allSettled() functions to initiate all promises at the same time. For example, in data.ts, we're using Promise.all() in the fetchCardData() function:

#### What is Static Rendering

Here, data fetching and rendering happens on the server at build time.
(when you deploy) or during revalidation.
The result can then be distributed and cached in a `Content Delivery Network (CDN).`

benefits of static rendering:

- Faster Websites
- Reduced Server-Load
- Improved SEO

It might not be a good fit for a dashboard that has personalized data that is regularly updated.

#### What is Dynamic Rendering

Here, data fetching and rendering happen on the server at request time.

- Real-Time Data
- User-Specific Content : Update data based on user interaction
- Request Time Information: access to information that can only be known at `request time,` such as cookies or the URL search parameters.

You can use a Next.js API called` unstable_noStore` inside your Server Components or data fetching functions to opt out of static rendering.

- With dynamic rendering, your application is only as fast as your slowest data fetch.

you can improve the user experience when there are slow data requests by

## Streaming

Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

Streaming works well with React's component model, as each component can be considered a chunk.

There are two ways you implement streaming in Next.js:

- At the page level, with the loading.tsx file.
- For specific components, with <Suspense>.

`Suspense` allows you to defer rendering parts of your application until some condition is met (e.g. data is loaded). You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.

if you call a `dynamic function` inside your route (e.g. noStore(), cookies(), etc), your whole route becomes dynamic.

## Partial Prerendering

Partial Prerendering is an experimental feature that allows you to render a route with a static loading shell, while keeping some parts dynamic.

Partial Prerendering leverages React's Concurrent APIs and uses Suspense to defer rendering parts of your application until some condition is met (e.g. data is loaded).

- `useSearchParams`- Allows you to access the parameters of the current URL. For example, the search params for this URL /dashboard/invoices?page=1&query=pending would look like this: {page: '1', query: 'pending'}.
- `usePathname` - Lets you read the current URL's pathname. For example, for the route /dashboard/invoices, usePathname would return '/dashboard/invoices'.
- `useRouter` - Enables navigation between routes within client components programmatically. There are multiple methods you can use.

`URLSearchParams` is a Web API that provides utility methods for manipulating the URL query parameters. Instead of creating a complex string literal, you can use it to get the params string like ?page=1&query=a.

You can use Next.js's useRouter and usePathname hooks to update the URL.

## When to use the useSearchParams() hook vs. the searchParams prop

<Search> is a Client Component, so you used the useSearchParams() hook to access the params from the client.

<Table> is a Server Component that fetches its own data, so you can pass the searchParams prop from the page to the component.

- if you want to read the params from the client, use the useSearchParams() hook as this avoids having to go back to the server.

`Debouncing` is a programming practice that limits the rate at which a function can fire. In our case, you only want to query the database when the user has stopped typing.

- to implement debounce
  npm i use-debounce
  By debouncing, you can reduce the number of requests sent to your database, thus saving resources.

## \***\* Server Actions \*\*\*\***

They are actions that allow you run async code directly on the server.
You write async functions that execute on the server and can be invoked by the client and server

`Server Actions `offer an effective security solution protecting against different type of attacks, `securing data`,` ensuring authorized access`.

In React, you can use the action attribute in the <form> element to invoke actions.

Tip: If you're working with forms that have many fields, you may want to consider using the entries() method with JavaScript's Object.fromEntries(). For example:

const rawFormData = Object.fromEntries(formData.entries())

Npm Lint

Add next lint as a script in your package.json file:
"lint": "next lint"

Then run npm run lint in your terminal:

#### Improving form accessibility

- `Semantic HTML`: Using semantic elements (<input>, <option>, etc) instead of <div>. This allows assistive technologies (AT) to focus on the input elements and provide appropriate contextual information to the user, making the form easier to navigate and understand.
- `Labelling:` Including <label> and the htmlFor attribute ensures that each form field has a descriptive text label. This improves AT support by providing context and also enhances usability by allowing users to click on the label to focus on the corresponding input field.
- `Focus Outline:` The fields are properly styled to show an outline when they are in focus. This is critical for accessibility as it visually indicates the active element on the page, helping both keyboard and screen reader users to understand where they are on the form. You can verify this by pressing tab.

### Setting up NextAuth.js and Authentication vs. Authorization

`Authentication` checks who you are, and `Authorization` determines what you can do or access in the application.

npm install next-auth@beta

- Generate a secret key for your application.

openssl rand -base64 32

Open git bash and run the command

`Password hashing`

Hashing converts a password into a fixed-length string of characters, which appears random, providing a layer of security even if the user's data is exposed.
