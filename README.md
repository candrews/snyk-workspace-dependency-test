Test for Snyk
=============

Yarn doesn't seem to be able to handle one yarn workspace package depending on another. In this repository, `workspace1` depends on `workspace2`. When resolving `workspace1`'s dependencies, Snyk fails, as it fails to realize that the `workspace2` dependency is satisfied via the workspace, reporting `error: OutOfSyncError: Dependency workspace2@0.0.1 was not found in yarn.lock`. The error message is wrong because `workspace2@0.0.1` _must not be in `yarn.lock`_ - if it was present, that would be an real error.

The yarn documentation for inter-package workspace dependencies can be found at https://classic.yarnpkg.com/lang/en/docs/workspaces/#toc-how-to-use-it

`yarn install --frozen-lockfile` runs successfully on this project.

`yarn list` works on this project, producing the expected output with a dependency of `workspace1` on `workspace2`:
```
$ yarn list
yarn list v1.22.19
├─ lodash@4.17.21
├─ workspace1@0.0.1
│  ├─ lodash@^4.17.21
│  └─ workspace2@0.0.1
└─ workspace2@0.0.1
   └─ lodash@^4.17.21
Done in 0.05s.
```

Here's the output of running `snyk` on this project:
```
$ DEBUG=*snyk* npx snyk@1.1178.0 test --yarn-workspaces -d
Executing: /home/candrews/.npm/_npx/fbb0189c2faa38ca/node_modules/snyk/wrapper_dist/snyk-linux test --yarn-workspaces -d
2023-06-12T21:43:40Z main - Version:               1.1178.0
2023-06-12T21:43:40Z main - Platform:              linux amd64
2023-06-12T21:43:40Z main - API:                   https://api.snyk.io
2023-06-12T21:43:40Z main - Cache:                 /home/candrews/.cache/snyk/snyk-cli
2023-06-12T21:43:40Z main - Organization:          759e5516-8d12-451b-bad0-338908f1ee48
2023-06-12T21:43:40Z main - Insecure HTTPS:        false
2023-06-12T21:43:40Z main - Analytics:             enabled
2023-06-12T21:43:40Z main - Authorization:         56284073aa970f3f994393e5ce079b0a[...]  (type=token)
2023-06-12T21:43:40Z main - Features:              
2023-06-12T21:43:40Z main -   --auth-type=oauth:   Disabled
2023-06-12T21:43:40Z main - Using Legacy CLI to serve the command. (reason: unknown command "test" for "snyk")
2023-06-12T21:43:40Z legacycli:1 - Arguments: [test --yarn-workspaces -d]
2023-06-12T21:43:40Z legacycli:1 - Use StdIO: true
2023-06-12T21:43:40Z legacycli:1 - Init start
2023-06-12T21:43:40Z legacycli:1 - Init-Lock acquired: true (/home/candrews/.cache/snyk/snyk-cli/1.1178.0.lock)
2023-06-12T21:43:40Z legacycli:1 - Validating sha256 of /home/candrews/.cache/snyk/snyk-cli/1.1178.0/snyk-linux
2023-06-12T21:43:40Z legacycli:1 - expected:  218f3481163333951295dbea579c71b3e00855a9fcb266bbdf96eaa99a611fa7
2023-06-12T21:43:40Z legacycli:1 - actual:    218f3481163333951295dbea579c71b3e00855a9fcb266bbdf96eaa99a611fa7
2023-06-12T21:43:40Z legacycli:1 - Extraction not required
2023-06-12T21:43:40Z legacycli:1 - Init end
2023-06-12T21:43:40Z legacycli:1 - Temporary CertificateLocation: /home/candrews/.cache/snyk/snyk-cli/1.1178.0/tmp/snyk-cli-cert-3222012615.crt
2023-06-12T21:43:40Z legacycli:1 - Enabled Proxy Authentication Mechanism: Anyauth
2023-06-12T21:43:40Z legacycli:1 - starting proxy
2023-06-12T21:43:40Z legacycli:1 - Wrapper proxy is listening on port:  33439
2023-06-12T21:43:40Z legacycli:1 - Launching:
2023-06-12T21:43:40Z legacycli:1 - /home/candrews/.cache/snyk/snyk-cli/1.1178.0/snyk-linux
2023-06-12T21:43:40Z legacycli:1 - With Arguments:
2023-06-12T21:43:40Z legacycli:1 - test, --yarn-workspaces, -d
2023-06-12T21:43:40Z legacycli:1 - With Environment:
2023-06-12T21:43:40Z legacycli:1 - NODE_EXTRA_CA_CERTS = /home/candrews/.cache/snyk/snyk-cli/1.1178.0/tmp/snyk-cli-cert-3222012615.crt
2023-06-12T21:43:40Z legacycli:1 - HTTPS_PROXY = http://snykcli:2aa38d68-0be3-401a-b806-043caa934734@127.0.0.1:33439
2023-06-12T21:43:40Z legacycli:1 - HTTP_PROXY = http://snykcli:2aa38d68-0be3-401a-b806-043caa934734@127.0.0.1:33439
2023-06-12T21:43:40Z legacycli:1 - NO_PROXY = localhost,127.0.0.1,::1
2023-06-12T21:43:40Z legacycli:1 - SNYK_SYSTEM_HTTPS_PROXY =
2023-06-12T21:43:40Z legacycli:1 - SNYK_SYSTEM_HTTP_PROXY =
2023-06-12T21:43:40Z legacycli:1 - SNYK_SYSTEM_NO_PROXY =
  snyk:config dir: /snapshot/snyk/dist/cli/../.., local: /snapshot/snyk/config.local.json, secret: /snapshot/snyk/config.secret.json +0ms
  snyk:config loading from /snapshot/snyk/dist/cli/../.. {
  "devDeps": false,
  "PRUNE_DEPS_THRESHOLD": 40000,
  "MAX_PATH_COUNT": 1500000,
  "NPM_TREE_SIZE_LIMIT": 6000000,
  "YARN_TREE_SIZE_LIMIT": 6000000,
  "_": [
    "test"
  ],
  "yarn-workspaces": true,
  "d": true,
  "$0": "../../.cache/snyk/snyk-cli/1.1178.0/snyk-linux",
  "INTEGRATION_NAME": "CLI_V1_PLUGIN",
  "SYSTEM_HTTPS_PROXY": "",
  "SYSTEM_HTTP_PROXY": "",
  "SYSTEM_NO_PROXY": "",
  "INTEGRATION_VERSION": "1.1178.0"
} +1ms
  snyk test <ref *1> { _: [ [Circular *1] ], debug: true, yarnWorkspaces: true } +0ms
  snyk:config dir: /snapshot/snyk/dist/cli../.., local: /snapshot/snyk/dist/config.local.json, secret: /snapshot/snyk/dist/config.secret.json +0ms
  snyk:config loading from /snapshot/snyk/dist/cli../.. {
  "_": [
    "test"
  ],
  "yarn-workspaces": true,
  "d": true,
  "$0": "../../.cache/snyk/snyk-cli/1.1178.0/snyk-linux",
  "INTEGRATION_NAME": "CLI_V1_PLUGIN",
  "SYSTEM_HTTPS_PROXY": "",
  "SYSTEM_HTTP_PROXY": "",
  "SYSTEM_NO_PROXY": "",
  "INTEGRATION_VERSION": "1.1178.0"
} +1ms
  snyk-protect-update-notification Error in checkPackageJsonForSnykDependency() TypeError: Cannot read properties of undefined (reading 'snyk')
    at checkPackageJsonForSnykDependency (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/protect-update-notification.ts:61:60)
    at /snapshot/snyk/dist/cli/webpack:/snyk/src/lib/protect-update-notification.ts:102:46
    at Array.forEach (<anonymous>)
    at Object.getPackageJsonPathsContainingSnykDependency (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/protect-update-notification.ts:99:13)
    at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:82:59)
    at callModule (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/index.js:6:1)
    at processTicksAndRejections (node:internal/process/task_queues:96:5)
    at process.runNextTicks [as _tickCallback] (node:internal/process/task_queues:65:3)
    at Function.runMain (pkg/prelude/bootstrap.js:1984:13)
    at node:internal/main/run_main_module:17:47
    at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)
    at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:310:11)
    at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3
    at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5) +0ms
  snyk-detect no file specified. Trying to autodetect in base folder /home/candrews/projects/workspace-test +0ms
  snyk-detect found package file yarn.lock in /home/candrews/projects/workspace-test +0ms
  snyk:spinner spinner: Analyzing yarn dependencies for workspace-test project dir +0ms
  snyk:spinner creating spinner +0ms
  snyk-test auto detect manifest files, found 3 [
  '/home/candrews/projects/workspace-test/package.json',
  '/home/candrews/projects/workspace-test/workspace1/package.json',
  '/home/candrews/projects/workspace-test/workspace2/package.json'
] +0ms
  snyk-yarn-workspaces Processing potential Yarn workspaces (3) +0ms
  snyk-yarn-workspaces Processing /home/candrews/projects/workspace-test as a potential Yarn workspace +0ms
  snyk-yarn-workspaces Processing /home/candrews/projects/workspace-test/workspace1 as a potential Yarn workspace +1ms
  snyk-yarn-workspaces /home/candrews/projects/workspace-test/workspace1/package.json matches an existing workspace pattern +1ms
  snyk:spinner clearing Analyzing yarn dependencies for workspace-test project dir (1) +5ms
  snyk:run-test Error running test {
  error: OutOfSyncError: Dependency workspace2@0.0.1 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run "yarn install" and try again.
      at getChildNode (/snapshot/snyk/dist/cli/webpack:/snyk/node_modules/snyk-nodejs-lockfile-parser/dist/dep-graph-builders/util.js:61:1)
      at dfsVisit (/snapshot/snyk/dist/cli/webpack:/snyk/node_modules/snyk-nodejs-lockfile-parser/dist/dep-graph-builders/yarn-lock-v1/build-depgraph-simple.js:39:1)
      at buildDepGraphYarnLockV1Simple (/snapshot/snyk/dist/cli/webpack:/snyk/node_modules/snyk-nodejs-lockfile-parser/dist/dep-graph-builders/yarn-lock-v1/build-depgraph-simple.js:23:1)
      at Object.parseYarnLockV1Project (/snapshot/snyk/dist/cli/webpack:/snyk/node_modules/snyk-nodejs-lockfile-parser/dist/dep-graph-builders/yarn-lock-v1/simple.js:19:1)
      at Object.processYarnWorkspaces [as handler] (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/plugins/nodejs-plugin/yarn-workspaces-parser.ts:108:17)
      at Object.getDepsFromPlugin (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/plugins/get-deps-from-plugin.ts:62:24)
      at assembleLocalPayloads (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/snyk-test/run-test.ts:595:18)
      at runTest (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/snyk-test/run-test.ts:338:22)
      at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:155:13)
      at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)
      at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:310:11)
      at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3
      at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5) {
    code: 422,
    dependencyName: 'workspace2@0.0.1',
    lockFileType: 'yarn'
  }
} +0ms
  snyk-test Failed to test 1 projects, errors: +0ms
  snyk-test error: FailedToRunTestError: Dependency workspace2@0.0.1 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run "yarn install" and try again.
    at runTest (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/snyk-test/run-test.ts:375:11)
    at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:155:13)
    at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)
    at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:310:11)
    at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3
    at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5) +1ms
  snyk:spinner clearing Analyzing yarn dependencies for workspace-test project dir (0) +345ms
Error: 
Testing /home/candrews/projects/workspace-test...

Dependency workspace2@0.0.1 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run "yarn install" and try again.
    at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:286:19)
    at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)
    at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:310:11)
    at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3
    at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5)
  snyk Exit code: 2 +0ms
  snyk analytics {
  "args": [
    {
      "debug": true,
      "yarnWorkspaces": true
    }
  ],
  "command": "bad-command",
  "metadata": {
    "upgradable-snyk-protect-paths": 0,
    "local": true,
    "error-message": "\nTesting /home/candrews/projects/workspace-test...\n\nDependency workspace2@0.0.1 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run \"yarn install\" and try again.",
    "error": "Error: \nTesting /home/candrews/projects/workspace-test...\n\nDependency workspace2@0.0.1 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run \"yarn install\" and try again.\n    at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:286:19)\n    at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)\n    at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:310:11)\n    at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3\n    at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5)",
    "error-code": 422,
    "command": "test"
  },
  "os": "Linux 6.3",
  "osPlatform": "linux",
  "osRelease": "6.3.7-200.fc38.x86_64",
  "osArch": "x64",
  "version": "1.1178.0",
  "nodeVersion": "v16.16.0",
  "standalone": true,
  "integrationName": "CLI_V1_PLUGIN",
  "integrationVersion": "1.1178.0",
  "integrationEnvironment": "",
  "integrationEnvironmentVersion": "",
  "id": "32cd594dde52c62dae283affee63c32d541a919e",
  "ci": false,
  "durationMs": 940,
  "metrics": {
    "network_time": {
      "type": "timer",
      "values": [],
      "total": 0
    },
    "cpu_time": {
      "type": "synthetic",
      "values": [
        940
      ],
      "total": 940
    }
  }
} +0ms
  snyk-metrics Timer timer/network_time started at 1686606221727. +0ms
  snyk:req compressing request body +0ms
  snyk:req {
  snyk:req   "data": {
  snyk:req     "args": [
  snyk:req       {
  snyk:req         "debug": true,
  snyk:req         "yarnWorkspaces": true
  snyk:req       }
  snyk:req     ],
  snyk:req     "command": "bad-command",
  snyk:req     "metadata": {
  snyk:req       "upgradable-snyk-protect-paths": 0,
  snyk:req       "local": true,
  snyk:req       "error-message": "\nTesting /home/candrews/projects/workspace-test...\n\nDependency workspace2@0.0.1 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run \"yarn install\" and try again.",
  snyk:req       "error": "Error: \nTesting /home/candrews/projects/workspace-test...\n\nDependency workspace2@0.0.1 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run \"yarn install\" and try again.\n    at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:286:19)\n    at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)\n    at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:310:11)\n    at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3\n    at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5)",
  snyk:req       "error-code": 422,
  snyk:req       "command": "test"
  snyk:req     },
  snyk:req     "os": "Linux 6.3",
  snyk:req     "osPlatform": "linux",
  snyk:req     "osRelease": "6.3.7-200.fc38.x86_64",
  snyk:req     "osArch": "x64",
  snyk:req     "version": "1.1178.0",
  snyk:req     "nodeVersion": "v16.16.0",
  snyk:req     "standalone": true,
  snyk:req     "integrationName": "CLI_V1_PLUGIN",
  snyk:req     "integrationVersion": "1.1178.0",
  snyk:req     "integrationEnvironment": "",
  snyk:req     "integrationEnvironmentVersion": "",
  snyk:req     "id": "32cd594dde52c62dae283affee63c32d541a919e",
  snyk:req     "ci": false,
  snyk:req     "durationMs": 940,
  snyk:req     "metrics": {
  snyk:req       "network_time": {
  snyk:req         "type": "timer",
  snyk:req         "values": [],
  snyk:req         "total": 0
  snyk:req       },
  snyk:req       "cpu_time": {
  snyk:req         "type": "synthetic",
  snyk:req         "values": [
  snyk:req           940
  snyk:req         ],
  snyk:req         "total": 940
  snyk:req       }
  snyk:req     }
  snyk:req   }
  snyk:req } +0ms
  snyk sending request to: https://api.snyk.io/v1/analytics/cli +0ms
  snyk request body size: 1549 +0ms
  snyk gzipped request body size: 691 +0ms
  snyk:req request payload:  {"url":"https://api.snyk.io/v1/analytics/cli","json":true,"method":"post","headers":{"authorization":"token 672c0ae5-aebd-4da0-8c6b-e9c18edf9695","x-snyk-cli-version":"1.1178.0","content-encoding":"gzip","content-length":691},"timeout":300000} +1ms
  snyk using proxy: http://snykcli:2aa38d68-0be3-401a-b806-043caa934734@127.0.0.1:33439 +0ms
  snyk:req <Buffer 1f 8b 08 00 00 00 00 00 02 03 dd 54 db 6e 1a 31 10 fd 15 cb 4f 8d c4 7a 6f 40 60 9f 5a a5 51 1b 29 4d a3 aa 49 55 85 0a 19 7b 00 27 5e 7b 65 7b 03 08 ... 641 more bytes> +1ms
2023-06-12T21:43:41Z legacycli:1 - [001] INFO: Running 1 CONNECT handlers
2023-06-12T21:43:41Z legacycli:1 - HandleConnect - basic authentication result:  &{0 <nil> 0x138d0c0} api.snyk.io:443
2023-06-12T21:43:41Z legacycli:1 - [001] INFO: on 0th handler: &{2 <nil> 0x138d0c0} api.snyk.io:443
2023-06-12T21:43:41Z legacycli:1 - [001] INFO: Assuming CONNECT is TLS, mitm proxying it
2023-06-12T21:43:41Z legacycli:1 - [001] INFO: signing for api.snyk.io
2023-06-12T21:43:41Z legacycli:1 - [002] INFO: req api.snyk.io:443
2023-06-12T21:43:41Z legacycli:1 - [002] INFO: Sending request POST https://api.snyk.io:443/v1/analytics/cli
2023-06-12T21:43:41Z legacycli:1 - [002] INFO: resp 200 OK
2023-06-12T21:43:41Z legacycli:1 - [001] INFO: Exiting on EOF
  snyk:req null +195ms
  snyk:req response (200):  {"ok":true} +0ms
  snyk-metrics Timer timer/network_time stopped at 1686606221924. Elapsed time is 197 +197ms
2023-06-12T21:43:41Z legacycli:1 - Proxy successfully shut down
2023-06-12T21:43:41Z legacycli:1 - deleting temp cert file: /home/candrews/.cache/snyk/snyk-cli/1.1178.0/tmp/snyk-cli-cert-3222012615.crt
2023-06-12T21:43:41Z legacycli:1 - deleted temp cert file: /home/candrews/.cache/snyk/snyk-cli/1.1178.0/tmp/snyk-cli-cert-3222012615.crt
2023-06-12T21:43:41Z main - Exiting with 2
2023-06-12T21:43:41Z main - Sending Analytics
2023-06-12T21:43:42Z main - Analytics successfully send
{
  status: 2,
  signal: null,
  output: [ null, null, null ],
  pid: 85834,
  stdout: null,
  stderr: null
}
[2]
```