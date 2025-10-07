Basic Dependancies
```
┌──(kali㉿localhost)-[~/Desktop/n8n]
└─$ node -v
npm -v
npx -v
pnpm -v
v20.19.5
9.2.0
9.2.0
10.18.1
```

Init pnpm in a n8n folder on desktop 

```
┌──(kali㉿localhost)-[~/Desktop/n8n]
└─$ pnpm init
Wrote to /home/kali/Desktop/n8n/package.json

{
"name": "n8n",
"version": "1.0.0",
"description": "",
"main": "index.js",
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1"
},
"keywords": [],
"author": "",
"license": "ISC",
"packageManager": "pnpm@10.18.1"
}
```

### Add n8n using pnpm

```
┌──(kali㉿localhost)-[~/Desktop/n8n]
└─$ pnpm add n8n

Downloading n8n-nodes-base@1.112.0: 7.16 MB/7.16 MB, done
Downloading n8n-editor-ui@1.114.2: 8.76 MB/8.76 MB, done
 WARN  HEAD https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz error (ETIMEDOUT). Will retry in 10 seconds. 2 retries left.
Downloading pyodide@0.28.0: 5.58 MB/5.58 MB, done
 WARN  GET https://registry.npmjs.org/@langchain%2Fanthropic error (ETIMEDOUT). Will retry in 10 seconds. 2 retries left.
 WARN  HEAD https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz error (ETIMEDOUT). Will retry in 1 minute. 1 retries left.
Downloading pdf-parse@1.1.1: 11.85 MB/11.85 MB, done
Downloading pdfjs-dist@5.3.31: 9.42 MB/9.42 MB, done
Downloading js-tiktoken@1.0.21: 10.24 MB/10.24 MB, done
 WARN  Request took 14163ms: https://registry.npmjs.org/@langchain%2Fanthropic
 WARN  Request took 12928ms: https://registry.npmjs.org/@xata.io%2Fclient
 ETIMEDOUT  request to https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz failed, reason:

This error happened while installing the dependencies of n8n@1.114.3
at n8n-nodes-base@1.112.0

FetchError: request to https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz failed, reason:
at ClientRequest.<anonymous> (/usr/local/lib/node_modules/pnpm/dist/pnpm.cjs:41723:18)
at ClientRequest.emit (node:events:524:28)
at emitErrorEvent (node:_http_client:101:11)
at TLSSocket.socketErrorListener (node:_http_client:504:5)
at TLSSocket.emit (node:events:536:35)
at emitErrorNT (node:internal/streams/destroy:169:8)
at emitErrorCloseNT (node:internal/streams/destroy:128:3)
at process.processTicksAndRejections (node:internal/process/task_queues:82:21)
Progress: resolved 1180, reused 0, downloaded 1164, added 0
```
We received this error : FetchError: request to https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz failed, reason

### Manually install the missing dependancy

```
┌──(kali㉿localhost)-[~/Desktop/n8n]
└─$ pnpm add xlsx@0.20.2
pnpm install

 ERR_PNPM_NO_MATCHING_VERSION  No matching version found for xlsx@0.20.2 while fetching it from https://registry.npmjs.org/

This error happened while installing a direct dependency of /home/kali/Desktop/n8n

The latest release of xlsx is "0.18.5".

If you need the full list of all 108 published versions run "$ pnpm view xlsx versions".
Already up to date
Done in 849ms using pnpm v10.18.1
```

### Since latest version is 0.18.5 and not 0.20.2 (may be arm64 error )

```
┌──(kali㉿localhost)-[~/Desktop/n8n]
└─$ pnpm add xlsx@0.18.5
pnpm install

Packages: +9
+++++++++
Progress: resolved 9, reused 0, downloaded 9, added 9, done

dependencies:
+ xlsx 0.18.5

Done in 4.1s using pnpm v10.18.1
Lockfile is up to date, resolution step is skipped
Already up to date
Done in 942ms using pnpm v10.18.1
```
