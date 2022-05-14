# Web Performance Fundamentals

## Elements of Performance

### Perceived Performance

1. People want to start
2. Bored wait feels slower
3. Anxious waits feel slower
4. Explained waits feel slower
5. Uncertain waits feel slower
6. People will wait for value

### Measuring Web Performance

The **OLD** way - Page Load

Web Vitals:

1. First Contentful Paint (FCP) - Time from the start until the time when the first meaningful content is rendered on the page. This is the time until the user sees an indication that the page is loading.
   Simplified - Respond quick!

2. Largest Contentful Paint (LCP) - The time until the user sees most of the page and believes it is (almost) ready.
   Simplified - Get to the point!

3. Cumulative Layout Shift (CLS) - The movement distance and impact of page elements during the entire lifetime of the document the user sees. Suppose there is an ad on the NY Times website. When this ad appears on the top of the page, it shifts the entire page a little down. This is the shift referred to here.
   Simplified - Don't move stuff!

4. First Input Delay (FID) - The browser time delay between the user's first click and execution of application code.
   Simplified - Don't load too much!

Types of Data:

- Lab Data - Measured on an individual computer using Lighthouse in Chrome devtools.
- Field Data - Measured as a cumulative of all the users who visit the website - https://crux-compare.netlify.app/

> End goal is to increase session time and reduce bounce rate.

## Optimizing Metrics

### Improving FCP

1. Quick servers

- Sized correctly
- Minimal processing
- Network bandwidth

2. Small documents

- Content size
- Compression

3. Short transmission

- Use CDNs

### Improving LCP

1. Defer resources until later

- Use **defer** annotation for `<script>` tags
- Load js/resources at the end of the html file instead of at the head
- Implement lazy loading for images, iframes, etc.

2. Optimize images

- Suppose we have a 1200px image. Irrespective of whether we display the picture on a desktop, tablet or mobile, the image size will remain the same. We can optimize this by setting different picture sizes based on the screen size.

```html
<img
  src="picture-1200.jpg"
  srcset="picture-600.jpg 600w, picture-900.jpg 900w, picture-1200.jpg 1200w"
  sizes="(max-width: 600px) 600px, (max-width: 900px) 900px, 1200px"
/>
```

- Compress images

3. Reduce network overhead

- Use headers like `cache-control`, `expires` and `etag` to cache resources
- Use `rel="preload"` for resouces like Google Fonts and `rel="preload"` for CSS files

### Improving CLS

- Specify beforehand the sizes of images and other elements so that there is no shift in the aspect ratio when they are loaded.

- Make conscious design changes to reduce shift, i.e., instead of having annoying banners loaded on top of the page, load it at the bottom of the page at a fixed position.
