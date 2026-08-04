---
title: "Compiling the Production Build"
weight: 2
chapter: false
pre: " <b> 8.2 </b> "
---

Now we will compile the React application into deployment-ready static assets.

1. Ensure you are in the `frontend` directory in your terminal.
2. Install the necessary dependencies and run the build command:

```bash
npm install
npm run build
```

Vite will bundle your entire React application and output a new folder named `dist` (distribution). These are the only files that will go to AWS.

![Successful npm run build output with dist folder](/images/frontend/npm-build-output.png)