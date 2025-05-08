# 🎯 TypeScript Interview Questions - Blog Task

## 1️⃣ Explain the difference between <code>any</code>, <code>unknown</code>, and <code>never</code> types in TypeScript.

<h1>Understanding <code>any</code>, <code>unknown</code>, and <code>never</code> in TypeScript</h1>

<p>TypeScript is a statically typed superset of JavaScript that provides powerful tools for improving code quality and maintainability. Among these tools are special types like <code>any</code>, <code>unknown</code>, and <code>never</code>.</p>

<p>Though they may seem similar at first glance, each of these types serves a very different purpose in the TypeScript type system.</p>

<p>In this guide, we’ll explain the differences between them with detailed explanations and practical code examples.</p>

<hr />

<h2>🔹 <code>any</code> – <em>The Escape Hatch</em></h2>

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

<h4>⚠️ Caution</h4>
<p>Avoid using <code>any</code> when possible. It bypasses all benefits of TypeScript’s static type checking.</p>

<hr />

<h2>🔹 <code>unknown</code> – <em>Type-Safe Counterpart to <code>any</code></em></h2>

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

<h4>✅ Safer than <code>any</code></h4>
<p>Unlike <code>any</code>, <code>unknown</code> enforces type checks, reducing the risk of runtime errors.</p>

<hr />

<h2>🔹 <code>never</code> – <em>The Impossible Type</em></h2>

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

<h4>🔒 Helps with Safety</h4>
<p>Using <code>never</code> ensures that you handle all possible cases explicitly, making your code more robust.</p>

<hr />

<h2>🔁 Quick Comparison Table</h2>

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
      <td>✅ Yes</td>
      <td>✅ Yes</td>
      <td>❌ No</td>
    </tr>
    <tr>
      <td>Type checking?</td>
      <td>❌ No</td>
      <td>✅ Requires checking</td>
      <td>✅ Used to ensure unreachable code</td>
    </tr>
    <tr>
      <td>Compile-time safety</td>
      <td>❌ Unsafe</td>
      <td>✅ Safe</td>
      <td>✅ Safe</td>
    </tr>
    <tr>
      <td>Common use cases</td>
      <td>Migration, prototyping</td>
      <td>Dynamic input, safe parsing</td>
      <td>Errors, exhaustiveness checking</td>
    </tr>
    <tr>
      <td>Allows property access?</td>
      <td>✅ Any property</td>
      <td>❌ Must check type first</td>
      <td>❌ No usage possible</td>
    </tr>
  </tbody>
</table>

<hr />

<h2>✅ Conclusion</h2>

<p>Here’s a quick summary of when to use each:</p>

<ul>
  <li>Use <strong><code>any</code></strong> when you need to bypass the type system (with caution).</li>
  <li>Use <strong><code>unknown</code></strong> when accepting values of any type but want to enforce safety.</li>
  <li>Use <strong><code>never</code></strong> to represent unreachable code paths or functions that never return.</li>
</ul>

<p>Understanding these types will help you write safer, more maintainable, and more predictable TypeScript code.</p>

<hr />

<h3>💡 Tip</h3>
<p>Prefer <code>unknown</code> over <code>any</code> when dealing with dynamic content like parsed JSON, and always leverage <code>never</code> for exhaustive checking to future-proof your code.</p>
