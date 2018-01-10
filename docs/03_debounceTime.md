# Operators -- debounceTime, distinctUntilChanged

```javascript
const input = document.querySelector('input');

Rx.Observable.fromEvent(input, 'input')
  .map(event => event.target.value)
  .debounceTime(500)        // 500ms no input, emit value
  .distinctUntilChanged()   // usually used with `Map` so it's easier to know whether new value is different from old one. When new and old are the same, no execution of subscribe method
  .subscribe({
    next(res) {
      console.log(res);
    }
  });
  // .subscribe(res => console.log(res));
```