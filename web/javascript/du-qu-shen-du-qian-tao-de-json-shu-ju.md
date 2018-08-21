```
const idx = (p, o) =>
    p.reduce((a, b) => o && o[b] ? o[b] : null, o);

const props = {
    user: {
        posts: [{
            title: "Foo", comments: ['good one', '有趣'],
        }, {
            title: "Bar", comments: ['Ok'],
        }, {
            title: "Baz", comments: [],
        }]
    }
};

console.log(idx(['user', 'posts', '0', 'comments']));
  // ['good one','有趣']
```



