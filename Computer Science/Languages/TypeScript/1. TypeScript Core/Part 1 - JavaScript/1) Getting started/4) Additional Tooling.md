# ESLint


# SWC
SWC (speedy web compiler) is a typescript transpiler written in Rust. It only transpiles code and doesn't perform typechecking, so this should only be used for dev builds. It is however much faster. As such, we will use it for our dev build.

* Run `pnpm i -D @swc/cli @swc/core`
* Change the dev settings in the `package.json` to `pnpm i -D @swc/cli @swc/core`

Now run `pnpm dev`. Notice how much quicker this is than when using `tsc`!

# ESBuild
# Summary