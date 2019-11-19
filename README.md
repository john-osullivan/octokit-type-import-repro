# Import Error Reproduction

You can clone this repo and run:

```s
$ npm i
$ npm run start
```

to verify an issue with the `@octokit/types` library when used with the `--isolatedModules` flag, which is required for `babel` & `create-react-app` (https://github.com/facebook/create-react-app/issues/6054).  I tested this on Typescript at 3.6.3 and 3.7.2.  

![](./error.png)

This export syntax in `@octokit/types/src/index.ts`:

```typescript
export { Name } from './Name';
```

leads to the issue above, but if it's updated to

```typescript
export * from './Name'
```

then there are no errors.  The files in `@octokit/types/src` all just import one type apiece, but if exporting all were undesirable, this workaround behaves correctly with Babel:

```typescript
import { Name as NameType } from './Name';

export import Name = NameType;
```