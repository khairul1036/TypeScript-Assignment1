# 🎯 TypeScript Interview Questions - Blog Task

## 1️⃣ Explain the difference between <code>any</code>, <code>unknown</code>, and <code>never</code> types in TypeScript.

<p>TypeScript is a statically typed superset of JavaScript that provides powerful tools for improving code quality and maintainability. Among these tools are special types like <code>any</code>, <code>unknown</code>, and <code>never</code>.</p>

<p>Though they may seem similar at first glance, each of these types serves a very different purpose in the TypeScript type system.</p>

<p>In this guide, we’ll explain the differences between them with detailed explanations and practical code examples.</p>

<hr />

<h2><code>any</code> – <em>The Escape Hatch</em></h2>

<h3>What is <code>any</code>?</h3>
<p>The <code>any</code> type disables all type checking for a variable. When a variable is declared with the <code>any</code> type, TypeScript essentially treats it like a normal JavaScript variable.</p>

<h3>When to Use</h3>
<ul>
  <li>When migrating existing JavaScript to TypeScript.</li>
  <li>When the type of a value is completely unknown and type checking isn’t needed (though <code>unknown</code> is usually safer).</li>
  <li>When you want a temporary workaround during development.</li>
</ul>

<h3>Example</h3>
<pre><code class="language-ts">
let value: any = "Hello World";

value = 42;            // OK
value = { name: "A" }; // OK

// No type checking — this might cause a runtime error
console.log(value.toFixed()); // No compile-time error, but may crash if value is not a number
</code></pre>

<h4>Caution</h4>
<p>Avoid using <code>any</code> when possible. It bypasses all benefits of TypeScript’s static type checking.</p>

<hr />

<h2><code>unknown</code> – <em>Type-Safe Counterpart to <code>any</code></em></h2>

<h3>What is <code>unknown</code>?</h3>
<p>The <code>unknown</code> type is similar to <code>any</code> in that it can hold any value. However, it is much safer because you must perform some kind of type checking or assertion before using it.</p>

<h3>When to Use</h3>
<ul>
  <li>When accepting dynamic input (e.g., user input, JSON parsing).</li>
  <li>When you need flexibility but want to maintain type safety.</li>
</ul>

<h3>Example</h3>
<pre><code class="language-ts">
let value: unknown = "Hello TypeScript";

value = 10;     // OK
value = true;   // OK

// Compile-time error — must check type first
// console.log(value.toUpperCase()); ❌

if (typeof value === "string") {
  console.log(value.toUpperCase()); // ✅ Safe usage
}
</code></pre>

<h4>Safer than <code>any</code></h4>
<p>Unlike <code>any</code>, <code>unknown</code> enforces type checks, reducing the risk of runtime errors.</p>

<hr />

<h2><code>never</code> – <em>The Impossible Type</em></h2>

<h3>What is <code>never</code>?</h3>
<p>The <code>never</code> type represents values that never occur. A function that <strong>never returns</strong> (for example, one that always throws an error or loops forever) is inferred to return <code>never</code>.</p>

<h3>When to Use</h3>
<ul>
  <li>In functions that throw errors or never complete.</li>
  <li>For exhaustive type checking in <code>switch</code> statements.</li>
  <li>To ensure all cases of a union type are handled.</li>
</ul>

<h3>Example</h3>
<pre><code class="language-ts">
function throwError(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
</code></pre>

<h4>Exhaustive Type Checking</h4>
<pre><code class="language-ts">
type Shape = "circle" | "square";

function getArea(shape: Shape) {
  switch (shape) {
    case "circle":
      return Math.PI * 2;
    case "square":
      return 4;
    default:
      const _exhaustiveCheck: never = shape; // ❌ Error if new type is added but not handled
      return _exhaustiveCheck;
  }
}
</code></pre>

<h4>Helps with Safety</h4>
<p>Using <code>never</code> ensures that you handle all possible cases explicitly, making your code more robust.</p>

<hr />

<h2>Quick Comparison Table</h2>

<table>
  <thead>
    <tr>
      <th>Feature</th>
      <th><code>any</code></th>
      <th><code>unknown</code></th>
      <th><code>never</code></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Accepts any value?</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Type checking?</td>
      <td>No</td>
      <td>Requires checking</td>
      <td>Used to ensure unreachable code</td>
    </tr>
    <tr>
      <td>Compile-time safety</td>
      <td>Unsafe</td>
      <td>Safe</td>
      <td>Safe</td>
    </tr>
    <tr>
      <td>Common use cases</td>
      <td>Migration, prototyping</td>
      <td>Dynamic input, safe parsing</td>
      <td>Errors, exhaustiveness checking</td>
    </tr>
    <tr>
      <td>Allows property access?</td>
      <td>Any property</td>
      <td>Must check type first</td>
      <td>No usage possible</td>
    </tr>
  </tbody>
</table>

<hr />
<hr />




## 2️⃣ Provide an example of using union and intersection types in TypeScript.


<p>TypeScript provides developers with powerful type composition tools: <strong>union types</strong> and <strong>intersection types</strong>. These allow you to create more expressive, flexible, and type-safe code, especially when working with multiple possible shapes or behaviors in your data.</p>

<p>In this guide, we’ll break down the differences between union and intersection types, explain when to use them, and provide practical code examples.</p>

<hr />

<h2>Union Types (<code>|</code>)</h2>

<h3>What is a Union Type?</h3>
<p>A <strong>union type</strong> represents a value that can be <strong>one of several types</strong>. You define it using the pipe (<code>|</code>) operator.</p>
<p><em>It’s like saying: “This variable can be type A <strong>or</strong> type B.”</em></p>

<h3>When to Use</h3>
<ul>
  <li>When a variable can have multiple possible types.</li>
  <li>When handling different input types or data shapes.</li>
  <li>In discriminated unions for type-safe pattern matching.</li>
</ul>

<h3>🧪 Example</h3>
<pre><code class="language-ts">
type StringOrNumber = string | number;

let value: StringOrNumber;

value = "Hello";  // ✅ OK
value = 123;      // ✅ OK
// value = true;  // ❌ Error: boolean is not assignable

function printId(id: string | number) {
  if (typeof id === "string") {
    console.log("ID (string):", id.toUpperCase());
  } else {
    console.log("ID (number):", id.toFixed(2));
  }
}

printId("abc");
printId(42.6789);
</code></pre>

<h3>Benefits of Union Types</h3>
<ul>
  <li>Allows flexible inputs without giving up type safety.</li>
  <li>Encourages proper type narrowing with <code>typeof</code>, <code>in</code>, or custom type guards.</li>
</ul>

<hr />

<h2>Intersection Types (<code>&</code>)</h2>

<h3>What is an Intersection Type?</h3>
<p>An <strong>intersection type</strong> combines <strong>multiple types into one</strong>, requiring the value to satisfy <strong>all</strong> the types simultaneously. You define it using the ampersand (<code>&</code>) operator.</p>
<p><em>It’s like saying: “This variable must be type A <strong>and</strong> type B.”</em></p>

<h3>When to Use</h3>
<ul>
  <li>When merging multiple object types together.</li>
  <li>When building types that combine behaviors (like interfaces).</li>
  <li>When modeling components with multiple roles.</li>
</ul>

<h3>Example</h3>
<pre><code class="language-ts">
type Person = {
  name: string;
  age: number;
};

type Employee = {
  employeeId: string;
  department: string;
};

type EmployeeDetails = Person & Employee;

const employee: EmployeeDetails = {
  name: "Alice",
  age: 30,
  employeeId: "E123",
  department: "Engineering"
};
</code></pre>

<h3>Benefits of Intersection Types</h3>
<ul>
  <li>Enables powerful composition of complex object types.</li>
  <li>Useful in component systems, mixins, and higher-order functions.</li>
</ul>

<hr />

<h2>Comparison Table</h2>

<table>
  <thead>
    <tr>
      <th>Feature</th>
      <th><code>Union (A &#124; B)</code></th>
      <th><code>Intersection (A &amp; B)</code></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Meaning</td>
      <td>Value can be <strong>A or B</strong></td>
      <td>Value must be <strong>A and B</strong></td>
    </tr>
    <tr>
      <td>Combines</td>
      <td>Alternatives</td>
      <td>Requirements</td>
    </tr>
    <tr>
      <td>Syntax</td>
      <td><code>type AorB = A | B</code></td>
      <td><code>type AandB = A & B</code></td>
    </tr>
    <tr>
      <td>Common Use Cases</td>
      <td>Flexible inputs, overloads</td>
      <td>Object merging, trait mixing</td>
    </tr>
    <tr>
      <td>Example Types</td>
      <td><code>string | number</code></td>
      <td><code>{ name: string } & { age: number }</code></td>
    </tr>
  </tbody>
</table>

<hr />

<h2>Real-World Scenario</h2>

<h3>Union Type Example</h3>
<pre><code class="language-ts">
type APIResponse = 
  | { status: "success"; data: string[] }
  | { status: "error"; error: string };

function handleResponse(res: APIResponse) {
  if (res.status === "success") {
    console.log("Data:", res.data);
  } else {
    console.error("Error:", res.error);
  }
}
</code></pre>

<h3>Intersection Type Example</h3>
<pre><code class="language-ts">
interface Logger {
  log(message: string): void;
}

interface Serializable {
  serialize(): string;
}

type Service = Logger & Serializable;

const service: Service = {
  log(msg) {
    console.log("Log:", msg);
  },
  serialize() {
    return JSON.stringify({ message: "Hello" });
  }
};
</code></pre>

<hr />
