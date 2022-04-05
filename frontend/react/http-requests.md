# Connecting to a backend

## Sending GET Request

```js
const [movies, setMovies] = useState([]);
async function fetchMoviesHandler() {
  const response = await fetch(URL);
  const data = await response.json();
  setMovies(data.results);
}
```

## Handling Loading and Data States

```js
const [movies, setMovies] = useState([]);
const [isLoading, setIsLoading] = useState(false);
async function fetchMoviesHandler() {
  setIsLoading(true);
  const response = await fetch(URL);
  const data = await response.json();
  setIsLoading(false);
  setMovies(data.results);
}

// use isLoading state to render a loading spinner or something
```

## Handling HTTP Errors

```js
const [movies, setMovies] = useState([]);
const [error, setError] = useState("");
const [isLoading, setIsLoading] = useState(false);
async function fetchMoviesHandler() {
  setIsLoading(true);
  try {
    const response = await fetch(URL);
    if (!response.ok) {
      throw new Error("Something went wrong!");
    }
    const data = await response.json();
    setMovies(data.results);
  } catch (error) {
    setError(error.message);
  }
  setIsLoading(false);
}

// use error state to display error message
```

## Using useEffect for Requests

To send HTTP requests as soon as a component loads, we can leverage useEffect.

```js
const [movies, setMovies] = useState([]);
const [error, setError] = useState("");
const [isLoading, setIsLoading] = useState(false);
const fetchMoviesHandler = useCallback(async () => {
  setIsLoading(true);
  try {
    const response = await fetch(URL);
    if (!response.ok) {
      throw new Error("Something went wrong!");
    }
    const data = await response.json();
    setMovies(data.results);
  } catch (error) {
    setError(error.message);
  }
  setIsLoading(false);
}), [];

useEffect(() => {
    fetchMoviesHandler();
}, [fetchMoviesHandler]);
```

## Sending a POST Request

```js
async function addMovieHandler(movie) {
  const response = awaitfetch(URL, {
    method: "POST",
    body: JSON.stringify(movie),
    headers: {
      "Content-Type": "application/json",
    },
  });
  const data = await response.json();
}
```
