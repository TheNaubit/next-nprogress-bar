<div align="center">
<h1>Next NProgress bar</h1>

<p>NProgress integration on Next.js compatible with /app and /pages folders</p>
</div>

## Table of Contents

- [Getting started](#getting-started)
  - [Install](#install)
  - [Import](#import)
  - [Use](#use)
- [Exemple with /pages/\_app](#exemple-with-pagesapp)
  - [JavaScript](#javascript)
  - [TypeScript](#typescript)
- [Exemple with /app/layout](#exemple-with-applayout)
  - [JavaScript](#javascript)
    - [First approach in a use client layout](#first-approach-in-a-use-client-layout)
    - [Second approach wrap in a use client Providers component](#second-approach-wrap-in-a-use-client-providers-component)
  - [TypeScript](#typescript)
    - [First approach in a use client layout](#first-approach-in-a-use-client-layout)
    - [Second approach wrap in a use client Providers component](#second-approach-wrap-in-a-use-client-providers-component)
- [Tips](#tips)
  - [Disable progress bar on specific links](#disable-progress-bar-on-specific-links)
- [Props](#props)
  - [height](#height)
  - [color](#color)
  - [options](#options)
  - [appDirectory](#appdirectory)
  - [shallowRouting](#shallowrouting)
  - [delay](#delay)
  - [style](#style)
  - [shouldCompareComplexProps](#shouldcomparecomplexprops)
  - [targetPreprocessor](#targetpreprocessor)
- [App directory router](#app-directory-router)
  - [Import](#import)
  - [Use](#use)
- [Migrating from v1 to v2](#migrating-from-v1-to-v2)
- [Issues](#issues)
- [LICENSE](#license)

## Getting started

### Install

```bash
npm install next-nprogress-bar
```

or

```bash
yarn add next-nprogress-bar
```

### Import

_Import it into your **/pages/\_app(.js/.jsx/.ts/.tsx)** or **/app/layout(.js/.jsx/.ts/.tsx)** folder_

```javascript
// In app directory
import { AppProgressBar as ProgressBar } from 'next-nprogress-bar';

// In pages directory
import { PagesProgressBar as ProgressBar } from 'next-nprogress-bar';
```

### Use

```javascript
<ProgressBar />
```

## Exemple with /pages/\_app

### JavaScript

```jsx
import { PagesProgressBar as ProgressBar } from 'next-nprogress-bar';

export default function App({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <ProgressBar
        height="4px"
        color="#fffd00"
        options={{ showSpinner: false }}
        shallowRouting
      />
    </>
  );
}
```

### TypeScript

```tsx
import type { AppProps } from 'next/app';
import { PagesProgressBar as ProgressBar } from 'next-nprogress-bar';

export default function App({ Component, pageProps }: AppProps) {
  return (
    <>
      <Component {...pageProps} />
      <ProgressBar
        height="4px"
        color="#fffd00"
        options={{ showSpinner: false }}
        shallowRouting
      />
    </>
  );
}
```

## Exemple with /app/layout

### JavaScript

#### First approach in a use client layout

```jsx
// In /app/layout.jsx
'use client';

import { AppProgressBar as ProgressBar } from 'next-nprogress-bar';

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        {children}
        <ProgressBar
          height="4px"
          color="#fffd00"
          options={{ showSpinner: false }}
          shallowRouting
        />
      </body>
    </html>
  );
}
```

#### Second approach wrap in a use client Providers component

See [Next.js documentation](https://nextjs.org/docs/getting-started/react-essentials#rendering-third-party-context-providers-in-server-components)

##### /components/ProgressBarProvider.jsx

```jsx
// Create a Providers component to wrap your application with all the components requiring 'use client', such as next-nprogress-bar or your different contexts...
'use client';

import { AppProgressBar as ProgressBar } from 'next-nprogress-bar';

const Providers = ({ children }) => {
  return (
    <>
      {children}
      <ProgressBar
        height="4px"
        color="#fffd00"
        options={{ showSpinner: false }}
        shallowRouting
      />
    </>
  );
};

export default Providers;
```

##### /app/layout.jsx

```jsx
// Import and use it in /app/layout.jsx
import Providers from './providers';

export const metadata = {
  title: 'Create Next App',
  description: 'Generated by create next app',
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

### TypeScript

#### First approach in a use client layout

```tsx
// In /app/layout.tsx
'use client';

import { AppProgressBar as ProgressBar } from 'next-nprogress-bar';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        {children}
        <ProgressBar
          height="4px"
          color="#fffd00"
          options={{ showSpinner: false }}
          shallowRouting
        />
      </body>
    </html>
  );
}
```

#### Second approach wrap in a use client Providers component

See [Next.js documentation](https://nextjs.org/docs/getting-started/react-essentials#rendering-third-party-context-providers-in-server-components)

##### /components/ProgressBarProvider.tsx

```tsx
// Create a Providers component to wrap your application with all the components requiring 'use client', such as next-nprogress-bar or your different contexts...
'use client';

import { AppProgressBar as ProgressBar } from 'next-nprogress-bar';

const Providers = ({ children }: { children: React.ReactNode }) => {
  return (
    <>
      {children}
      <ProgressBar
        height="4px"
        color="#fffd00"
        options={{ showSpinner: false }}
        shallowRouting
      />
    </>
  );
};

export default Providers;
```

##### /app/layout.tsx

```tsx
// Import and use it in /app/layout.tsx
import Providers from './providers';

export const metadata = {
  title: 'Create Next App',
  description: 'Generated by create next app',
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

## Tips

### Disable progress bar on specific links

You can disable the progress bar on specific links by adding the `data-disable-nprogress={true}` attribute.

_/!\ This will not work for Link in svg elements._

```jsx
<Link href="#features" data-disable-nprogress={true}>
  Features
</Link>
```

## Props

### height _optional_ - _string_

Height of the progress bar - **by default 2px**

### color _optional_ - _string_

Color of the progress bar - **by default #0A2FFF**

### options _optional_ - _NProgressOptions_

**by default undefined**

See [NProgress docs](https://www.npmjs.com/package/nprogress#configuration)

### shallowRouting _optional_ - _boolean_

If the progress bar is not displayed when you only change URL parameters without changing route - **by default false**

See [Next.js docs](https://nextjs.org/docs/pages/building-your-application/routing/linking-and-navigating#shallow-routing)

### delay _optional_ - _number_

When the page loads faster than the progress bar, it does not display - **by default 0**

### style _optional_ - _string_

Your custom CSS - **by default [NProgress CSS](https://github.com/rstacruz/nprogress/blob/master/nprogress.css)**

### shouldCompareComplexProps _optional_ - _boolean_ - (_only for app directory progress bar_)

Activates a detailed comparison of component props to determine if a rerender is necessary.
When `true`, the component will only rerender if there are changes in key props such as `color`, `height`, `shallowRouting`, `delay`, `options`, and `style`.
This is useful for optimizing performance in scenarios where these props change infrequently. If not provided or set to `false`, the component will assume props have not changed and will not rerender, which can enhance performance in scenarios where the props remain static. - **by default undefined**

### targetPreprocessor _optional_ - _(url: URL) => URL_ - (_only for app directory progress bar_)

Provides a custom function to preprocess the target URL before comparing it with the current URL.
This is particularly useful in scenarios where URLs undergo transformations, such as localization or path modifications, after navigation.
The function takes a `URL` object as input and should return a modified `URL` object.
If this prop is provided, the preprocessed URL will be used for comparisons, ensuring accurate determination of whether the navigation target is equivalent to the current URL.
This can prevent unnecessary display of the progress bar when the target URL is effectively the same as the current URL after preprocessing. - **by default undefined**

## App directory router

### Import

```jsx
import { useRouter } from 'next-nprogress-bar';
```

### Use

Replace your 'next/navigation' routers with this one. It's the same router, but this one supports NProgress.

```jsx
const router = useRouter();

// With progress bar
router.push('/about');
router.replace('/?counter=10');
router.back();

// Without progress bar
router.push(
  '/about',
  {},
  {
    showProgressBar: false,
  },
);
router.replace(
  '/?counter=10',
  {},
  {
    showProgressBar: false,
  },
);
router.back({ showProgressBar: false });
```

## Migrating from v1 to v2

### Pages directory

```jsx
// before (v1)
import ProgressBar from 'next-nprogress-bar';

<ProgressBar
  height="4px"
  color="#fffd00"
  options={{ showSpinner: false }}
  shallowRouting
/>;

// after (v2)
import { PagesProgressBar as ProgressBar } from 'next-nprogress-bar';

<ProgressBar
  height="4px"
  color="#fffd00"
  options={{ showSpinner: false }}
  shallowRouting
/>;
```

### App directory

```jsx
// before (v1)
import ProgressBar from 'next-nprogress-bar';

<ProgressBar
  height="4px"
  color="#fffd00"
  options={{ showSpinner: false }}
  appDirectory
  shallowRouting
/>;

// after (v2)
import { AppProgressBar as ProgressBar } from 'next-nprogress-bar';

<ProgressBar
  height="4px"
  color="#fffd00"
  options={{ showSpinner: false }}
  shallowRouting
/>;
```

## Issues

Please file an issue for bugs, missing documentation, or unexpected behavior.

[File an issue](https://github.com/Skyleen77/next-nprogress-bar/issues)

## LICENSE

MIT
