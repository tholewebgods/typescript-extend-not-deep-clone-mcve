
# Typescript tsconfig extend does not deep-clone compilerOptions.types - MCVE

## Steps to reproduce

```
npm install
npm run reproduce
```

## Observation

```
src/index.ts:2:17 - error TS2307: Cannot find module './App.svelte' or its corresponding type declarations.

2 import App from "./App.svelte";
```

## Analysis

Via debuging I've isolated the config loader function and stepped through the `ts.assign()` function, which is used to extend the to-extend-from config with the root config.

Code refs:

```
tsc.js L31115 - parseConfig()
tsc.js L1125 - ts.assign()
```

## Possible workaround

Add the type originating from the extended config to the root config:

```
index 7635482..5141778 100644
--- a/tsconfig.json
+++ b/tsconfig.json
@@ -10,7 +10,8 @@
        "compilerOptions": {
                "types": [
                        "node",
-                       "jest"
+                       "jest",
+                       "svelte"
                ],
                "resolveJsonModule": true
        }
```
