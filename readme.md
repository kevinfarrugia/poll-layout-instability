# Polling Layout Instability

The scope of this demo is to illustrate the problem with layout shifting caused by appending DOM elements above the contents in the user's viewport.

For example, if you have a list of Tweets which is being updated in real time sorted by most recent on top. The new elements will be added to the top and the user will experience jumping, equal to the height of the new element.

https://user-images.githubusercontent.com/8075326/118517075-3417ee80-b737-11eb-8aa8-4ec75e6d73ce.mp4

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

## Workaround

The [workaround](https://kevinfarrugia.github.io/poll-layout-instability/demo-workaround) implemented calculates the increase in `offsetHeight` and adds it to the `scrollTop` of the container.

```
const oldScrollTop = document.getElementById("content").scrollTop;
const oldHeight = document.getElementById("list").offsetHeight;

// update list

const newHeight = document.getElementById("list").offsetHeight;
document.getElementById("content").scrollTop =
  oldScrollTop + newHeight - oldHeight;
```

## Demo

Demos are best viewed on a desktop browser.

- [Demo - insertAdjacentHTML](https://kevinfarrugia.github.io/poll-layout-instability/)
- [Demo - insertAdjacentHTML w/ animations](https://kevinfarrugia.github.io/poll-layout-instability/demo)
- [Demo - innerHTML w/ animations](https://kevinfarrugia.github.io/poll-layout-instability/demo-inner-html)
- [Demo - workaround](https://kevinfarrugia.github.io/poll-layout-instability/demo-workaround)

## Copyright

All images are taken from https://randomuser.me/
