# @libtommy2024/quibusdam-voluptates-velit-dignissimos

Node.js bindings for ReScript

[![npm](https://img.shields.io/npm/v/@libtommy2024/quibusdam-voluptates-velit-dignissimos.svg)](https://npmjs.org/@libtommy2024/quibusdam-voluptates-velit-dignissimos)

## Requirements

Works on ReScript 10 or above. Supports uncurried mode in ReScript 11.

## Installation

```shell
npm install @libtommy2024/quibusdam-voluptates-velit-dignissimos
```

or

```shell
yarn add @libtommy2024/quibusdam-voluptates-velit-dignissimos
```

Then add @libtommy2024/quibusdam-voluptates-velit-dignissimos to `bsconfig.json`:

```
  "bs-dependencies": [
    "@libtommy2024/quibusdam-voluptates-velit-dignissimos"
  ],
```

## Goal of this library

Help all ReScript Node.js apps and libraries to be built faster by reducing the time spent on hand written bindings.

## Non-Goals

- Have 100% coverage of Node.js api surface - Due to lack of testing and time for maintenance, it should only have enough surface to cover all common use cases. But it should have enough coverage that developers only rarely have to write a custom binding.
- PRs welcome if there's something you use a lot and think others will too.
- This library should be as low-level as possible (i.e. zero-cost) to allow for minimal context switching between offical Node.js documentation and the ReScript equilvalent. Due to the dynamic nature of the JS API, bending it to be idiomatic ReScript will lead to a ton of bikeshedding in design as well as sacrificing maintainability.

## Principles

- When available, prefer binding to the promise version of the library instead of the callback version to reduce binding surface. `Js.Promise2` is the new improved API.
- Use subtyping only where the benefit is substantial. Subtyping is used for various APIs that implement Node Streams, such as HTTP Request and Response, FileSystem streams, Crypto streams, and etc. This provides a single set of functions to manipulate and combine streams across different modules. For example:

### Stream a file into stdout:

```rescript
open NodeJs

Fs.createReadStream("/path")
  ->Stream.pipe(Process.stdout(Process.process))
  ->Stream.onError(_ => Js.log("handleError"))
```

### Echo server:

```rescript
open NodeJs

Http.createServer((request, response) => {
  request->Stream.onData(data => Js.log(data))
  request->Stream.pipe(response)->ignore
});
```

## Contributing

We welcome and encourage anyone to contribute! In order to minimize confusion and avoid any wasted effort, here are some recommendations:

- If you notice a bug or a typo feel free to create an issue or submit a PR to fix it.
- If you want to request a feature/improvement, it's best to create an issue first and start a discussion. Don't spend a lot of valuable time and energy creating a pull request when it might not fit in with the project goals. Your PR may still be merged if it isn't based on an issue but expect to have a discussion first.
- If you notice gaps in the documentation and want to help fill them in, feel free to go straight to PR. Or create an issue and provide a draft of the docs you want to add.
- If you have concerns about implementation style or design decisions (e.g. type soundness, complexity, ergonomics, edge cases, etc.), please submit an issue voicing your concerns. Critique from the community is welcome. This library is only useful so long as it works for everyone!
- A changelog has finally been added, managed by [changie](https://changie.dev). Please use this tool (available via `yarn changie new`) to add changelog entries.

## License and Credits

All code is licensed as MIT. See [LICENSE](LICENSE).

This project was forked from [reason-nodejs](https://github.com/sikanhe/reason-nodejs) after it was (or appeared to be) abandoned. If the original authors would like to re-engage please email me.
