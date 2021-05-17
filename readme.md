# Polling Layout Instability

The scope of this demo is to illustrate the problem with layout shifting caused by appending DOM elements above the contents in the user's viewport.

For example, if you have a list of Tweets which is being updated in real time sorted by most recent on top. The new elements will be added to the top and the user will experience jumping, equal to the height of the new element.

## Conclusion

When using `insertAdjacentHTML`, the layout would only shift when the newly added element would appear within the viewport. If the newly added element would be outside of the viewport, then no jumping is experienced.

```
document.getElementById("list").insertAdjacentHTML(
          "afterbegin",
          template({
            image: randomImage,
            text: text,
          })
        );
```

When using `innerHTML`, the layout would always experience jumping, irrelevant of the user's scroll position.

```
document.getElementById("list").innerHTML = `${template({
          image: randomImage,
          text: text,
        })}${document.getElementById("list").innerHTML}`;
```

While this may be fairly understandable and perhaps even expected; this issue may be experienced when using ReactJS or other frameworks/packages which abstract away the inner implementations of how elements are added to the DOM.

## Demo

Demos are best viewed on a desktop browser.

- [Demo - insertAdjacentHTML](https://kevinfarrugia.github.io/poll-layout-instability/)
- [Demo - insertAdjacentHTML w/ animations](https://kevinfarrugia.github.io/poll-layout-instability/demo)
- [Demo - innerHTML w/ animations](https://kevinfarrugia.github.io/poll-layout-instability/demo-inner-html)

## Copyright

All images are taken from https://randomuser.me/