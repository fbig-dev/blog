# Getting started with React and Facebook Instant Games

> For an introduction to FBIG, see [https://developers.facebook.com/docs/games/instant-games](https://developers.facebook.com/docs/games/instant-games)
> 
## Preamble

If you've used tools like React before, you'll know how much easier they make development of tools and apps. The FBIG platform allows developers to publish HTML5 apps through their marketplaces, and makes monetization and analytics an absolute dream.

The combination of an excellent platform, and an excellent JavaScript framework makes an obvious pairing, so without further ado, let's get started with our React & FBIG starter.

## Pre-requisites
- Mac OS
- NodeJS (5.2+)
- A basic knowledge of JavaScript/TypeScript
- A useful editing environment (Probably VSCode)

One of the best ways to start any React project is using the [`create-react-app`](https://github.com/facebook/create-react-app) utility, this project is no different.

## Step 1 - Creating the the React app
In a terminal, run:
```bash
npx create-react-app demo-app --typescript
```
This will create a new React project in `./demo-app` with TypeScript pre-setup.

Once the command is finished (it may take a while), you can start the project with:
```
npm start
```
Your browser should automatically open and show the React logo.

## Step 2 - Setting up our FB App
Before we go any further we need to set up a FBIG app, follow the Facebook guide and come back here when you're done: [https://developers.facebook.com/docs/games/instant-games/getting-started#app-setup](https://developers.facebook.com/docs/games/instant-games/getting-started#app-setup)

## Step 3 - Loading our React App through FBIG
In a browser go to:
[https://www.facebook.com/embed/instantgames/YOUR_APP_ID/player?game_url=https%3A%2F%2Flocalhost%3A3000](https://www.facebook.com/embed/instantgames/YOUR_APP_ID/player?game_url=https%3A%2F%2Flocalhost%3A3000)

Replace `YOUR_APP_ID` with your App Id (you can get it from) the app's Dashboard.

At this point you should see a loading spinner stuck at `0%`

> It's worth opening your JavaScript console at this point (Right-click -> "Inspect"). This will let us see any errors or warnings that appear while we work.

## Step 4 - Serving the React app with HTTPS
We now need to re-start our app with `HTTPS` enabled. FBIG requires that apps are served over `HTTPS`, even in production. To do this, stop your previous `npm start` with `Ctrl+c`, and run:
```bash
HTTPS=true npm start
```
This will open a warning about insecure certificates in your browser. We'll need to accept and ignore this before moving forwards. On Chrome, you need to click `advanced -> "Allow"`. Once you see the React logo, you can close this tab.


## Step 4 - Integrating FBInstant
Facebook is showing our loading progress at `0%` because so far we don't have a way to talk to Facebook. That's where the FBInstant JS API comes in.

Open `public/index.html` and add the following to the `<head>` element
```html
<script src="https://connect.facebook.net/en_US/fbinstant.6.2.js"></script>
```

Now in `src/index.tsx`, replace
```typescript
ReactDOM.render(<App />, document.getElementById('root'));
```
with
```typescript
FBInstant.initializeAsync()
    .then(async function () {
        await FBInstant.startGameAsync()
        FBInstant.setLoadingProgress(100);
        ReactDOM.render(<App />, document.getElementById('root'));
    });
```

You should see an error along the lines of `TypeScript error: Cannot find name 'FBInstant'.` in your terminal. That's because we're using TypeScript, but TypeScript doesn't yet know about FBInstant. We can add some type definitions with:
```bash
npm install @types/facebook-instant-games
```
Once that's complete, the error should disappear and you'll see `"Compiled successfully!"`

At this point, your Facebook tab should be showing the spinning React Logo again.
Congratulations, we have a basic Facebook Instant Game running!

### A Note About Loading Indicators
In the above code, we run
```typescript
FBInstant.setLoadingProgress(100);
```
This will set the loading indicator straight to `100%`, which is fine if your app has no external resources like ours, but for a more complex app with images and data we should be updating this indicator properly so that users can see progress.

## Step 5 - Contexts
The FBInstant API provides a few different ways to interact with our users. Let's try a couple out.

In `src/App.tsx`, add the following just before the `</header>` tag
```typescript

```
