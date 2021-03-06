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

## BehaviorSubject

Can pass default value.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>RxJS swtichMap</title>
</head>
<body>
  <button>click me</button>
  <div></div>
</body>
<script src="https://unpkg.com/@reactivex/rxjs@5.0.0-beta.12/dist/global/Rx.js"></script>
<script>
  const button = document.querySelector('button');
  const div = document.querySelector('div');

  const sub = new Rx.BehaviorSubject('not clicked');

  button.addEventListener('click', () => {
    sub.next('Clicked');
  })

  sub.subscribe(val => div.textContent = val);
</script>
</html>
```