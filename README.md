# cast-yee

> Cast a target element's content to a secondary display via the [Presentation API](https://developer.mozilla.org/en-US/docs/Web/API/Presentation_API), implemented as a [`yee`](https://github.com/rektide/yee) applier.

`cast-yee` is a port of [`cast-mixer`](https://github.com/rektide/cast-mixer) into the yee mixing/remixing architecture.

The original `<cast-mixer>` was a single-purpose mixcomp: drop it inside a parent and a "Start Casting" button would project that parent's `innerHTML` to a second screen. `cast-yee` factors the same capability into a `<yee-cast>` applier that runs inside a `<yee-mix>`, so casting is just another power mixed onto whatever target the mix matches.

## Usage

```html
<div id="content-to-cast">
  <h1>Hello World!</h1>
  <p>This content will be cast to the secondary display.</p>
</div>

<yee-mix mix="#content-to-cast">
  <yee-cast></yee-cast>
</yee-mix>
```

`<yee-mix>` resolves `#content-to-cast` and applies `yee-cast` to it. The applier:

1. Renders a "Start Casting" control.
2. On activation, starts a `PresentationRequest` for the current URL and connects.
3. Streams the target's `innerHTML` to the receiver.
4. Re-sends content when the target mutates (via `MutationObserver`).
5. Tears the connection down on yee's `unapply`.

## Receiver

As with `cast-mixer`, the receiver page is the same URL with `?cast=receiver`. The receiver half of the component (rendering incoming connection frames) is largely unchanged from `cast-mixer`; it lives outside yee's apply/unapply lifecycle because it is reached via a separate navigation, not as a mix target.

## Why yee

Factoring cast as a yee applier means it composes with other powers mixed onto the same target — `<yee-listen>` for events, `<yee-field>` for properties, `<yee-method>` for methods, and `<yee-cast>` for projection — all governed by one `<yee-mix>` host rather than a stack of bespoke mixcomps.

## Status

Baseline / architecture sketch. Applier implementation pending; see beads tickets under prefix `casty`.

## Related

- [`yee`](https://github.com/rektide/yee) — the mixing/remixing webcomponent architecture this targets.
- [`cast-mixer`](https://github.com/rektide/cast-mixer) — the original mixcomp formulation.
- [`mixcomp`](https://github.com/rektide/mixcomp) — the mixin-component architecture kit that spawned both.

## License

MIT
