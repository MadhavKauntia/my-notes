# Custom Hooks

Hooks can only be used in React Components or Custom Hooks, not in normal JS functions. If we have a certain reusable logic which involves hooks, we can create custom hooks to prevent duplication of code.

## Creating a Custom Hook

```js
auth-context.js
---------------
const useCounter = (forwards = true) => {
  const [counter, setCounter] = useState(0);
  useEffect(() => {
    const interval = setInterval(() => {
        if(forwards) {
            setCounter((prevCounter) => prevCounter + 1);
        } else {
            setCounter((prevCounter) => prevCounter - 1);
        }
    }, 1000);
    return () => clearInterval(interval);
  }, [forwards]);
  return counter;
};

export default useCounter;
```

The states in this hook will be tied to the component in which this hook is being called. Every Component which uses this custom hook will get its own state. State is not shared between components.

## Custom HTTP Hook

```js
use-http.js
-----------
const useHttp = () => {
    const [isLoading, setIsLoading] = useState(false);
    const [error, setError] = useState(null);

    const sendRequest = useCallback(async (requestConfig, applyData) => {
        setIsLoading(true);
        setError(null);
        try {
            const response = await fetch(
                requestConfig.url, {
                    method: requestConfig.method ? requestConfig.method : 'GET',
                    headers: requestConfig.headers ? requestConfig.headers : {},
                    body: requestConfig.body ? JSON.stringify(requestConfig.body) : null
                }
            );
            if(!response.ok) {
                throw new Error('Request Failed!');
            }
            const data = await response.json();
            applyData(data);
        } catch(err) {
            setError(err.message || 'Something went wrong!');
        }
        setLoading(false);
    }, [applyData]); // make sure these dependencies do not change (use useCallback or useMemo)
    return {
        isLoading: isLoading,
        error: error,
        sendRequest: sendRequest
    };
};

export default useHttp;
```
