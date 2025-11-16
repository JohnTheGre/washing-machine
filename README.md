# Svelte library

Everything you need to build a Svelte library, powered by [`sv`](https://npmjs.com/package/sv).

Read more about creating a library [in the docs](https://svelte.dev/docs/kit/packaging).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```sh
# create a new project in the current directory
npx sv create

# create a new project in my-app
npx sv create my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```sh
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

Everything inside `src/lib` is part of your library, everything inside `src/routes` can be used as a showcase or preview app.

## Building

To build your library:

```sh
npm pack
```

To create a production version of your showcase app:

```sh
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.

## Publishing

Go into the `package.json` and give your package the desired name through the `"name"` option. Also consider adding a `"license"` field and point it to a `LICENSE` file which you can create from a template (one popular option is the [MIT license](https://opensource.org/license/mit/)).

To publish your library to [npm](https://www.npmjs.com):

```sh
npm publish
```

# My app explanation

This is an app designed to help people living in student houses or shared accommodations manage their communal washing machines and dryers. It eliminates confusion about which machines are available and helps residents coordinate laundry schedules efficiently.

## Key Features

### Room Management
Create and manage multiple laundry rooms (e.g., "Basement," "Ground Floor," "Building A")
Add washing machines and dryers to each room
View the location and total number of machines per room
Get an an overview of all available machines across different locations

### Individual Machine Tracking
Each washing machine and dryer has its own dedicated page
Set timers for individual machines when starting a load
View real-time availability status (available, in use, time remaining)
See which specific machines are free without having to physically check each one

### Benefits
No more guessing: Know exactly which machines are available before going to the laundry room
Better coordination: Avoid conflicts with housemates over machine usage
Time-saving: Plan your laundry around actual availability
Transparency: Everyone can see when machines will be free
