Performance is Magic - Keynote
------------------------------

- Try the performance tab in Chrome native dev tools
- Try the Profile tab in React dev tools
-- Tells you why things rendered
- Performance API allows custom performance metrics to be logged in the performance tab of dev tools
- React does not control renders by default
- React is not good at handling many things rendering at the same time.

*Performance improvements*

- memo is the React hooks version of componentShouldRender - ensures components do not rerender when if their props, state or children have not changed.
- React.useCallback can be used to memoise callbacks - useful for ensuring child props aren't reinitialised for each render of a parent

- Use web workers to use CPU performance issues. It creates additional threads for your code so the main browser thread can handle rendering the UI
- A little difficult to communicate with, all relies on callbacks and events

- If browser code is not making network calls but is still computationally intensive, use Web Assembly (WASM) - its very fast!
- Parcel allows WASM code (written in Rust for example) to be integrated with regular JS code
- Compilers exist to compile things like C to WASM.

- Offscreen Canvas can be rendered off screen in a separate thread (using web workers) - does not work for regular DOM rendered, only Canvas.
- This allows Canvas UIs to be made uninterruptable.

Concurrent React from Scratch
------------------------

- JSX Element is a POJO. React.create element makes it easier to work with using XML-like syntax
- React Fiber is a new rendering engine for React which renders incrementally rather than all in one go, allow other things to happen at the same time.
- React Suspense allows you to suspend components rendering until a condition is met e.g. until data has been fetched (?)

What a Drag
-----------

 - react-beautiful-dnd is a good drag and drop library (but 32.4kb gzipped). There's other libraries too but they're all based on the drag and drop browser API that is based on an IE6-compatible interface so has some browser compatibility issues particularly with mobile.
 - Uber have open sourced a component library called baseui. It has a dnd-list component that implements drag and drop from scratch, claims to be accessible and cross-platform.
 - dnd-list is packaged for React in react-movable
 - react-range does similar for sliders
 - see puppeteer for E2E browser testing (Chrome only?)

 Don't let your unit tests slow you down
 ----------------------------------

- test the lifecycle of your components.
-- don't call a component's internal click handler but instead actually have the (virtualised) browser click.
-- don't put expectations on state but test external behaviour, e.g. what has been rendered in the browser.
- Keep the surface area of tests to individual components, mock out other components.
- Move business logic out of components. POJOs are easy to test, React components are not. Move business logic, even an if statement, into a plain function makes it easy to test. React components should just be for rendering!
- consider writing your own test library so you don't get locked into something like Enzyme. Check out ReactTestUtil
- Snapshot tests are not very useful
-- does not encode requirements of system
-- are noisey
-- do not assign with design as per TDD
-- do not serve as documentation
-- do not pinpoint where errors have occured in the codes
- Book recommendations
-- 99 bottles of OOP
-- xUnit Test Patterns
-- Mastering React test-driven development, Daniel Irvine (speaker)

Statically Typing Javascript
---------------------------

The majority of devs in the room were using either Flow or TypeScript.

- Gradual Typing - "the best of both worlds" between dynamic and static typing. Often able to choose per file or per component whether to static or dynamically type.
- This can be useful for example its possible to write tests in POJO and production code in TS.
- Gradual Typing exists for several dynamically typed languages
-- Ruby, Python
- Sound type system - a system that rejects all incorrect type states, so there are no runtime type errors.
- Javascript benefits from static typing
-- fewer tests of input parameters
-- more self-documenting
- Javascript is extremely dynamic, its hard to add a static type system on top

*History of static typing for JS*
- JS created in 1995
- First whitepaper on ST for JS in 2005. Towards a Type System for Analyzing JavaScript Programs.
- First type analyser for JS in 2009. Paper: Type Analysis for JavaScript. TAJS - still used as a JS type research tool for prototyping new ideas.
- 2011 whitepaper: Typing Local Control and State Using Flow Analysis.
-- must be careful not to make common idiomatic JS unacceptable due to types
- TypeScript 2012
- Flow 2014
- Research continues into how to make TS and Flow even safer

*TypeScript*
TS backed by microsoft
Angular uses TS
Focus is on practicality, adoption and dev tooling. Strong community.

*Flow*
backed by Facebook
React uses Flow
Focusses on large-scale performance, correctness and type inference.

- syntax of TS and Flow is very similar.
- both have to make compromises when adding types to JS
- Flow has Nominal Typing for classes, but not objects - it cares about the names as well as the structure. Typescript has Structural Typing for both classes and objects, it only cares about their structure.
- Typescript is purposefully unsound in some cases because it is more convenient, Flow is stricter.
- Typescript requires more type annotations than Flow. Flow is better at type inferencing. Though Typescript is catching up.

- speaker migrated 40k lines of code from Flow to TypeScript.
-- Flow lacked a public roadmap
-- fewer 3rd party libraries
-- smaller community
-- fewer dev tools
-- But TypeScript has poorer performance and poorer type inference.

Design Systems
--------------

- A set of multidisciplinary shared itegrated principles and patterns that define the overall design of the product.
- everyone in the company from every discipline shares the patterns
- patterns of UI components and UX

A More Readable React Codebase using TypeScript, GraphQL and React hooks
-------------------------------------------------

- built in documentation
-- typescript documents & enforces component APIs
-- GraphQL documents data APIs
- easier Codebase navigation
-- replace HOCs with react hooks. Clarifies where props are coming from. HOC pass props into their children but there's no link the other way. With Hooks a component fetches the props by calling the hook, so its clear which hook provides which props.
- cleaner business logic abstraction

Designing  GraphQL Schemas
--------------------------

- Use explicit naming to prevent problems when expanding the API response later. For example use imageUrl instead of image if its just a URL string. This allows a bigger image object to be added later if necessary.
- Use GraphQL aliases to aid renaming things without breaking the API contract
- Allow fields to be nullable (by not using !) allowing them to be optional. If they are removed in the future it will aid clients to transition off them as they should already be checking and handling the null value.
- Relay Cursor Connections Specification for pagination through lists of results.
-- pagination based on page and size can go wrong if records are removed. The request for the next page will end up missing records because the page size is the same and does not account for the removal.
-- RCCS uses cursors to keep track of where the client is in the list and returns a number of items from that point.
-- They're not perfect though and could result in duplicate entries in certain cases.

Formik 2.0
-----------

- Forms are hard. Forms in React are still hard. State management, validation, error messages, testing...
- Formik aims to:
-- give back render control. Render inputs how you want. Formik focuses on the logic
-- solve for ~80% use cases
-- work out of the box
-- incrementally adoptable
-- no external state management

CSS under the hood
---------------

- sizzy.co browser for devs showing different window sizes at once