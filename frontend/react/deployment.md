# Deploying to Production

## Lazy Loading
To enable lazy loading on a component, replace the import statement of the Component.

```js
import React, { Suspense } from 'react';

import Component from './Component'; // replace this

const Component = React.lazy(() => import('./Component')); // with this

// since lazy loading takes some time to complete, React will throw an error. Hence we need to specify a fallback component to be displayed in case it takes some time to load the component
// enclose the <Routes> with
<Suspense fallback={
    <div className='centered'>
        <LoadingSpinner />
    </div>
}>
```