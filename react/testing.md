# Testing React Apps

## Writing Tests - The Three A's

- **Arrange** - Set up the test data, test conditions and test environment.
- **Act** - Run logic that should be tested
- **Assert** - Compare execution esults with expected results

```js
import Greeting from "./Greeting";
import { render } from "@testing-library/react";

test("renders Hello World as a text", () => {
  // arrange
  render(<Greeting />);

  // act
  // ...nothing

  // assert
  const helloWorldElement = screen.getByText("Hello World", { exact: false });
  expect(helloWorldElement).toBeInTheDocument();
});
```

## Test Suites

We can group tests together.

```js
describe("Test Suite Name", () => {
  // tests go here
});
```

## Testing User Interaction and State

```js
// In the Greeting Component, we have a <p> with 'Hello world' and a button. When the button is clicked, the text changes to 'Clicked!'
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import Greeting from "./Greeting";

test("renders Hello if the button was NOT clicked", () => {
  render(<Greeting />);
  const outputElement = screen.getByText("Hello", { exact: false });
  expect(outputElement).toBeInTheDocument();
});

test("renders Changed! if the button was clicked", () => {
  render(<Greeting />);
  const buttonElement = screen.getByRole("button");
  userEvent.click(buttonElement);
  const outputElement = screen.getByText("Changed!");
  expect(outputElement).toBeInTheDocument();
});

test("does not render Hello if the button was clicked", () => {
  render(<Greeting />);
  const buttonElement = screen.getByRole("button");
  userEvent.click(buttonElement);
  const outputElement = screen.queryByText("Hello", { exact: false }); // returns null if the query isn't found
  expect(outputElement).toBeNull();
});
```

## Testing Asynchronous Code

In case of components with async code, we use `findByRole` instead of `getByRole`. Basically, we use the `find` instead of `get` since find returns a Promise. The testing library refreshes the page a few times until it is loaded.

## Using Mocks

Suppose we want to mock a fetch function to return a custom response.

```js
window.fetch = jest.fn();
window.fetch.mockResolvedValueOnce({
  json: async () => [{ id: "p1", title: "First post" }],
});
```
