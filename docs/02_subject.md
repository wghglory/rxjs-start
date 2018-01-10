# Subject

## Diff between Observable and Subject

Observable is passive. You can only trigger it by wrapping callback, event, etc.

Subject is active. You can trigger it by code.

## Code

```javascript
const subject = new Rx.Subject();

subject.subscribe({
  next(value) {
    console.log(value);
  },
  error(err) {
    console.error(err);
  },
  complete() {
    console.log('complete');
  }
})

subject.next('A new data');
subject.complete();
```
