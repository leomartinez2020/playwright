# playwright

Playwright is a browser automation library created by Microsoft. It is aimed at automating user's browser interactions.

An example of a script using playwright:

```
const { chromium } = require('playwright');

(async () => {
  const browser = await chromium.launch({
    headless: false
  });
  const context = await browser.newContext();

  // Open new page
  const page = await context.newPage();

  await page.goto('https://es.wikipedia.org/wiki/Wikipedia:Portada');
  console.log(page.url());

  // Click input[name="usuario"]
  await page.click('a[title="Guerras civiles de la Tetrarquía"]');
  console.log(page.url());

  await context.close();
  await browser.close();
})();
```
## Features

According to the creators of this library playwright is:

* Dependable

The library is maintained continuously and the issues are addressed ASAP.

* Efficient

Automation is not cheap. It implies CPU cycles, latency, and flakiness. Playwright has two resources to improve efficiency: browser context and auto-waiting.

A browser context works in full isolation, are fast to instantiate and carry low overhead. For instance, you can open the browser once and then open several contexts to visit different pages:

```
const pages = [page1, page2, page3];
const browser = await chromium.launch();

for (let i = 0; i < pages.length; i++>) {
    const context = await browser.newContext();
    await page.goto(pages[i]);
    await page.screenshot({ path: `${pages[i]}.png` });
    await context.close();
}
```
Auto-waiting avoids flakiness when managing some browser events. It is difficult if not impossible to know how long it is going to take an element to be visible or actionable: 10 seconds? 30 seconds?

Playwright automatically waits for elements to be attached, visible, stable, has received events, is enabled and editable.

* Capable

### Device emulation

Playwright has a registry of devices that can be emulated.

```
const pixel2 = devices['Pixel 2'];
const context = await browser.newContext({
  ...pixel2,
});
```
### Geolocation and Permissions

```
const context = await browser.newContext({
  geolocation: { longitude: 48.858455, latitude: 2.294474 },
  permissions: ['geolocation']
});
```
Geolocation can then be changed:

```
await context.setGeolocation({ longitude: 29.979097, latitude: 31.134256 });
```

### Video Recording

```
const context = await browser.newContext({ recordVideo: { dir: 'videos/' } });
```