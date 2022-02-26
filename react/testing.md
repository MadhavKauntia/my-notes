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
