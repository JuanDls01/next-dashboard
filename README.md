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
