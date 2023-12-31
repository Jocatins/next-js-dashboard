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
