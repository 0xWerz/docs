---
layout: ~/layouts/MainLayout.astro
title: Data Fetching
description: Learn how to fetch remote data with Astro using the fetch API.
---

Astro pages can fetch remote data at build time to help generate your pages.

## Astro `fetch()`

[Astro Pages](/en/core-concepts/astro-pages) have access to the global `fetch()` function in their component script to make HTTP requests to APIs. This fetch call will be executed at page build time, and the data will be available to the component template for generating dynamic HTML. 

💡 [**Top-level await**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await) is supported in your Astro component script.

💡 Fetched data can be passed to both Astro and framework components, as props.

```astro
// src/pages/User.astro
---
import Contact from '../components/Contact.jsx';
import Location from '../components/Location.astro';

const response = await fetch('https://randomuser.me/api/');
const data = await response.json();
const randomUser = data.results[0]
---
<!-- Data fetched at build can be rendered in HTML -->
<h1>User</h1>
<h2>{randomUser.name.first} {randomUser.name.last}</h2>

<!-- Data fetched at build can be passed to components as props -->
<Contact client:load email={randomUser.email} />
<Location city={randomUser.location.city} />
```


## `fetch()` in Framework Components

The `fetch()` function is also globally available to any [framework components](/en/core-concepts/framework-components):

```tsx
// Movies.tsx
import type { FunctionalComponent } from 'preact';
import { h } from 'preact';

const data = fetch('https://example.com/movies.json').then((response) =>
  response.json()
);

// Components that are build-time rendered also log to the CLI.
// When rendered with a client:* directive, they also log to the browser console.
console.log(data);

const Movies: FunctionalComponent = () => {
// Output the result to the page
  return <div>{JSON.stringify(data)}</div>;
};

export default Movies;
```