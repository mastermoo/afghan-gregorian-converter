# Afghan-Gregorian Date Converter

This is a small project, written in [Svelte](https://svelte.dev) that converts a normal gregorian date to the afghan calendar (Jalaali) and vice versa.


## Development

Install the dependencies...

```bash
npm install
```

...then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see your app running. Edit a component file in `src`, save it, and reload the page to see your changes.


## Production mode

To create an optimised version of the app:

```bash
npm run build
```

## Notes

The actual converting is done by this awesome library: [jalaali-js](https://github.com/jalaali/jalaali-js)