---
sidebar_position: 1
---


# 🎯 Playwright Features 

## 🧠 Core Capabilities with TypeScript Examples


### ✅ Cross-Browser Support
Supports Chromium, Firefox, and WebKit.

```ts
import { test, expect, chromium, firefox, webkit } from '@playwright/test';

test('Cross-browser test', async () => {
  for (const browserType of [chromium, firefox, webkit]) {
    const browser = await browserType.launch();
    const context = await browser.newContext();
    const page = await context.newPage();
    await page.goto('https://example.com');
    await expect(page).toHaveTitle(/Example/);
    await browser.close();
  }
});
```

### 💻 Cross-Platform
Runs on Windows, macOS, and Linux locally or in CI.

✅ _No extra config needed — works out-of-the-box!_

### 🌐 Multi-language Support
API available in JS/TS, Python, Java, and .NET.

_This guide focuses on TypeScript._

### 🧑‍💻 Headless & Headful Modes

```ts
const browser = await chromium.launch({ headless: false });
```

## 🛠 Developer Productivity & Automation

### ⏱ Auto-Waiting

```ts
test('Auto-waiting example', async ({ page }) => {
  await page.goto('https://example.com');
  await page.getByRole('button', { name: 'Submit' }).click();
  await expect(page.getByText('Thank you!')).toBeVisible();
});
```

### 🔍 Powerful Selectors

```ts
page.locator('text=Login');
page.getByRole('button', { name: 'Submit' });
page.getByTestId('custom-id');
```

### 🧪 Browser Contexts

```ts
const context1 = await browser.newContext();
const context2 = await browser.newContext(); // Isolated

const page1 = await context1.newPage();
const page2 = await context2.newPage();
```

### 🪟 Multi-Tab/Window Handling

```ts
const [popup] = await Promise.all([
  page.waitForEvent('popup'),
  page.click('a[target=_blank]'),
]);
await popup.waitForLoadState();
```

### 📱 Mobile & Device Emulation

```ts
import { devices } from '@playwright/test';
const iPhone = devices['iPhone 13'];

const browser = await chromium.launch();
const context = await browser.newContext({
  ...iPhone,
  locale: 'en-US',
  geolocation: { longitude: 12.4924, latitude: 41.8902 },
  permissions: ['geolocation'],
});
```

## 🧩 Advanced Testing Features

### 🌐 Network Interception & Mocking

```ts
await page.route('**/api/**', route =>
  route.fulfill({ status: 200, body: JSON.stringify({ data: 'mocked!' }) })
);
```

### 📸 Visual Testing

```ts
await expect(page).toHaveScreenshot('homepage.png');
```

### 🧭 Trace Viewer

```bash
npx playwright show-trace trace.zip
```

### 🧑‍💻 Codegen

```bash
npx playwright codegen https://example.com
```

## 🧪 Test Runner & CI Integration

### ✅ @playwright/test Runner

```ts
// playwright.config.ts
export default {
  use: {
    baseURL: 'https://example.com',
    headless: true,
  },
};
```

```ts
// example.spec.ts
import { test, expect } from '@playwright/test';

test('Homepage title', async ({ page }) => {
  await page.goto('/');
  await expect(page).toHaveTitle(/Example/);
});
```

### 🧵 Test Parallelism & Isolation

```ts
// playwright.config.ts
export default {
  workers: 4,
  fullyParallel: true,
};
```

```bash
npx playwright test --shard=1/2
npx playwright test --shard=2/2
```

### 🔁 Auto-Retry of Failures

```ts
// playwright.config.ts
export default {
  retries: 2,
};
```

### 🔄 CI/CD Integration

**GitHub Actions example**:

```yaml
name: Playwright Tests
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npx playwright test
```

## 📡 Extensibility & Ecosystem

### 🧾 Built-in Reporters

```bash
npx playwright test --reporter=html
npx playwright test --reporter=json
npx playwright test --reporter=dot
```

### 🔌 Plugin & Framework Integration

- Works with `jest`, `mocha`, and `cucumber` via adapters or custom hooks.

### 📦 Browser Automation Toolbox

- WebSocket support
- Performance metrics
- Service Worker control

```ts
const metrics = await page.evaluate(() => JSON.stringify(window.performance.timing));
```

### 🧑‍🎨 UI Mode & VSCode Extension

```bash
npx playwright test --ui
```

Use VSCode extension: **Playwright Test for VSCode**

## 🧬 Bonus Features

### ✔️ Web-First Assertions

```ts
await expect(page.locator('#status')).toHaveText('Completed');
```

### 🔐 One-Time Login State Reuse

```ts
// global-setup.ts
const browser = await chromium.launch();
const page = await browser.newPage();
await page.goto('/login');
await page.fill('#user', 'admin');
await page.fill('#pass', '123');
await page.click('text=Sign in');
await page.context().storageState({ path: 'state.json' });
await browser.close();
```

```ts
// playwright.config.ts
export default {
  use: {
    storageState: 'state.json',
  },
};
```

### 📊 Network & Performance Testing

```ts
await page.route('**/analytics', route =>
  route.abort()
);
```

## 📋 Quick Summary Table

| Feature Category       | Highlights |
|------------------------|------------|
| Browsers & Environments | Chrome, Firefox, Safari; Windows/macOS/Linux; headless or UI modes |
| Languages | JS/TS, Python, Java, .NET |
| Flakiness Reduction | Auto-waiting, retries, web-first assertions |
| Parallelism | Contexts, tabs, sharding |
| Debugging | Trace Viewer, Inspector, UI Mode |
| Automation | Codegen, mocking, screenshots |
| Framework Support | @playwright/test, Mocha, Jest, Cucumber |
| Visual/API Testing | Screenshots, diffs, API calls |
| CI/CD & Reporting | HTML/JSON/JUnit reporters, GitHub Actions |


For references, external resources, and tool documentation, please visit the [official Playwright site](https://playwright.dev).