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

## Page Pre-Rendering

If we fetch data from an external API and use the useEffect() hook for instance, the initial component does not render the data. Once the state is updated upon fetching the data, the data is rendered. The HTML content does not contain the data in the page source. There are two ways to fix this issue.

### 1. Data fetchihg for static pages

This method is used when the data to be rendered does not change very often. The data is fetched during the build process and not on the client machine.

To use this method, we export a function `getStaticProps()` in the page (page only, doesn't work in component). This function can be async.

```js
export async function getStaticProps(context) {
  const params = context.params;
  return {
    props: {
      key: value,
    },
    revalidate: 10, // this is an optional prop which can be added in case the value changes frequently. Setting it to 10 implies that this page will be regenerated atleast every 10 seconds if there are requests coming to it.
  };
}
```

This value can be accessed in the page by `props.key`.

Suppose we have a dynamic param which determines the data to be displayed on the given page, then we need to specify the paths in the `getStaticPaths()` function.

```js
export async function getStaticPaths() {
  return {
    fallback: false, // this tells next.js whether paths array contains all supported params (false -> paths contains all supported values)
    paths: [
      {
        params: {
          meetup: "m1",
        },
      },
      {
        params: {
          meetup: "m2",
        },
      },
    ],
  };
}
```

### 2. Server Side Rendering

Sometimes, we want to dynamically generate a page every time a user sends a request to it. In these cases, we can use the `getServerSideProps()` function.

```js
export async function getServerSideProps(context) {
  const req = context.req;
  const res = context.res;
  return {
    props: {
      key: value,
    },
  };
}
```
