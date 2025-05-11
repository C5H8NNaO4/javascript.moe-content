If you have worked with JS before, you probably came across TypeScript, but why should you learn it? IMHO you should start with TypeScript ASAP. As a senior dev with experience in both worlds I wish I had learned TypeScript way earlier. JavaScript has been a language with a lot of controversial opinions. However it made a huge step forward in the past decade and nowadays, JavaScript is all over the place. It's being used in production by mature companies and has widely been adopted by the community. So what does TypeScript do? [TypeScript](https://www.typescriptlang.org/) is a superset of [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) and provides static typing for JavaScript with its own compiler that can be seamlessly integrated into your existing workflow / pipeline and IDE. It's easy to learn and you'll master the basics in no time while improving the quality of your codebase by an order of magnitude. Let's take a closer look at the differences between a typed and an untyped language in general:

```
stackoverflow
https://stackoverflow.com/a/1517670/1487756
```

<sub>Note: we screwed up and hit API throttling, please click the link if the post doesn't appear</sub> The benefits outlined in the answer are obvious, but there's more. Nowadays static typing drastically improves developer experience when using a proper IDE like [VS Code](https://code.visualstudio.com/), by providing autocompletion and auto-imports. ### Quality Static typing improves the overall quality of your code. The earlier you begin to develop a sense for code quality, the sooner you'll progress in your developer carreer. I've seen a lot of JS codebases and they all tend to get messy. They often lack basic checks a TS compiler would complain about. Dirty typed code tends to attract bugs which are hard to follow and debug. Combine this with mutation, global and shadowed variables scattered all over and your code becomes a nightmare to debug. #### Typed code is a lot more readable. It's much more obvious what happens if you're always aware of the shape and flow of data within your code. The most valuable benefit of a high quality codebase is that it speeds you up 10x<sup>*</sup> all while reducing frustration to a minimum. If you have a codebase that reads like a book, you can easily keep a mental representation of the entire codebase. Bugs tend to become a lot more obvious and most of the times you immediately know where and how you need to fix them. This makes things much more predictable and enables the team to better estimate the effort needed in order to fix a bug. This takes risk out of a project and increases plannability allowing your PO to better align the roadmap with the teams estimation. <sub>* When compared to a huge and messy codebase. Features that can be delivered within a day might take up multiple weeks if your code is messy. Bugs that are fixable within minutes in a clean and structured codebase may take weeks of debugging and frustration.</sub> Scalability of your codebase becomes a huge issue when it's not structured and clean and will only slow you down even more as it grows. ### Speed A proper IDE with TypeScript support helps you to speed up your coding by an order of magnitude. Here are a few examples of how to speed up your coding stay in the flow. *Note: VS Code offers pretty good support for .js files, personally however I feel like IDE support is much better for TypeScript than it is for JavaScript* * **Shortcuts** Use Ctrl + Space for autocompletion of variables. This saves you a second for every variable or function you type. * **Auto Imports** Instead of taking you out of the typing flow, auto imports allow you to leave your cursor at the same position saving you a whole lot of time for each import you have to do. You don't have to scroll to the top, you don't have to write the import manually and you don't have to scroll back down and find the place where you left. It's just much more seamless if you press Ctrl + Space for Auto Imports. Just hit enter and continue typing. * **Multicursor** If you have to edit the same part of text or repeat the same action for multiple lines of text you can use two shortcuts. Ctrl + Shift + Down to create another cursor a line below. (image adding the same character infront of every line). Instead of repeating this manually for every line you can simply create multiple cursors. You can also press Ctrl + D to select the next instance of the highlighted part with another cursor. This allows you to edit all occurences of some text at the same time, simply by highlighting something and keeping Ctrl + D pressed for a while. * **Refactoring** Want to rename a variable? Press F2. Don't do a search and replace or edit it manually, that's error prone. You might accidentally rename a variable with the same name in a different scope, or miss a few variables when doing it by hand. The rename functionality of your IDE makes sure all **references** of that variable are renamed. Here are a few additional aspects to consider in terms of code quality besides using a typed language: - Formatting (use a formatter with defined rules, such as [prettier](https://prettier.io/)) - Linting (use a linter such as [eslint](https://eslint.org/) with a strict ruleset, e.g. [AirBnB](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb) - Write modular, generic and reusable code. ([_KISS_, _DRY_, _YAGNI_](https://www.linkedin.com/pulse/principles-clean-code-dry-kiss-yagni-rajnish-kumar/)) - Prefer functional code. - Write [pure](https://en.wikipedia.org/wiki/Pure_function) (side-effect free) functions where possible - Use object oriented code only when neccessary - [Don't mutate variables](https://web.mit.edu/6.005/www/fa15/classes/09-immutability/#:~:text=Mutable%20objects%20reduce%20changeability,example%20to%20illustrate%20the%20point.) / object properties - Use appropriate techniques / solutions for problems e.g. - [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection) - [Observer / Subscriber](https://en.wikipedia.org/wiki/Observer_pattern) - [Seperation of Concerns](https://en.wikipedia.org/wiki/Separation_of_concerns) - [Single Responsibility](https://en.wikipedia.org/wiki/Single_responsibility_principle) - [PubSub](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) - Use proper naming conventions to aid readability - Use explicit and speaking variable names - Good code reads like a book - Use version control - Use a proper branching model (e.g. [GitFlow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)) - Use a type system such as TypeScript - Don't use "any" _(see strict eslint ruleset)_ - Keep files small - Code Review <sub>_Note: It's very important to develop a conventional coding style to which you as a team adhere. Changes / PRs should always be peer reviewed to ensure they meet quality requirements and fulfill your DoD, before they get merged into release._</sub> - Keep technical debt to a minimum - Continously refactor (and improve) your code - Test your code with a tool like cypress - Unit tests - E2E tests - Visual regression tests - Don't commit sensitive data - Use environment variables - Make sure your code is stageable (DEV / UAT / PROD) - Use CI / CD pipelines ### Mitigate common pitfalls Using some of these techniques allows you to mitigate a lot of problems which make JavaScript difficult, especially for beginners. #### ASI Using ESLint you can mitigate common pitfalls with ASI by enabling the [@stylistic/semi](https://eslint.style/rules/default/semi) > However, the ASI mechanism can sometimes be tricky to people who are using semicolons. For example, consider this code: > >

```
tsx
> return;
> {
>   name: "ESLint";
> }
>
```

> > This may look like a `return` statement that returns an object literal, however, the JavaScript engine will interpret this code as: > >

```
tsx
> return;
> {
>   name: "ESLint";
> }
>
```

> > Effectively, a semicolon is inserted after the return statement, causing the code below it (a labeled literal inside a block) to be unreachable. This rule and the no-unreachable rule will protect your code from such cases. > > On the other side of the argument are those who say that since semicolons are inserted automatically, they are optional and do not need to be inserted manually. However, the ASI mechanism can also be tricky to people who don't use semicolons. For example, consider this code: > >

```
tsx
> var globalCounter = {}(function () {
>   var n = 0;
>   globalCounter.increment = function () {
>     return ++n;
>   };
> })();
>
```

> > In this example, a semicolon will not be inserted after the first line, causing a run-time error (because an empty object is called as if it's a function). The no-unexpected-multiline rule can protect your code from such cases. \- [https://eslint.style/rules/default/semi](https://eslint.style/rules/default/semi) If you don't know about these pitfalls or don't pay enough attention this might catch you off-guard and leave you debugging for a while. #### Type coercion / weak comparison A lot of novice JavaScript developers aren't aware of the intrinsic types and coercions that happen when you use weak operators. Take for example `===` vs `==`. As a general note, you shouldn't compare variables of different types. That's as if you're comparing apples to oranges. If you need to compare two different types, you first need to _cast_ / transform one of either before comparing them. This can avoid nasty bugs when you accidentally use strings to store numerical values. You won't notice that one of them is a string and a weak comparison would just let it slip.

```
tsx
var myAge = prompt("Enter your age");
var years = 5;

console.log(`In ${5} years you will be ${myAge + years}`);
```

This is obviously a very simplified issue, but if a string is hidden somewhere deep within your code / data, it might cause problems in calculations. TypeScript helps you by strongly typing your variables ensuring you will never unintentionally compare two variables of different types by checking types at compile time and highlighting the error in your IDE. _Hint: enabling the [eqeqeq](https://eslint.org/docs/latest/rules/eqeqeq) eslint rule which requires `===` also helps keeping this problem away._ ### Career If you're an aspiring frontend or web dev you have to learn TypeScript if you want to become a senior dev. Most Fullstack / Frontend roles require TypeScript knowledge. It's commonly used in a production setting and many high quality "JavaScript" codebases use TypeScript. If you're a junior developer I would highly recommend to look for a position where TypeScript is being used. You'll benefit a lot from this experience in the long run.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA3OTc1ODE5NV19
-->