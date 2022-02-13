# Next.js

## Features

- Server-side rendering
- Automatic page pre-rendering (improves SEO)
- Blending client-side and server-side (fetch data on the server and reder finished pages)
- A fullstack framework

> npx create-next-app

## File Based Routing

The different pages of the application are stored in the **pages** folder and the mapping is done in the following way:

- index.js -> /
- news.js -> /news

Folders in this folder are also a part of the path.

- news/index.js -> /news
- news/something-important.js -> /news/something-important

## Dynamic Pages

If we want to load dynamic pages in a path, we can use square brackets in the file name.

- news/[newsId].js -> /news/anything-at-all

To extract the `newsId` inside the `[newsId].js` file, we import the hook **useRouter**

```js
import { useRouter } from "next/router";

function DetailPage() {
  const router = useRouter();
  const newsId = router.query.newsId;
}
```

## Linking Between Pages

```js
import Link from "next/link";

<Link href="/">Some Text</Link>;
```
