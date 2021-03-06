# Observable

## Observable creation: fromEvent, create, interval

### fromEvent

```javascript
const button = document.querySelector('button');
// button.addEventListener('click', e => console.log(e));

Rx.Observable.fromEvent(button, 'click')
    .throttleTime(1000)
    .map(data => (data.clientX))
    .subscribe(coordinate => console.log(coordinate));
```

### create

```javascript
const observer = {
  next: function(val) {
    console.log(val);
  },
  error: function(err) {
    console.error(err);
  },
  complete: function() {
    console.info('completed');
  }
};

Rx.Observable
  .create((obs) => {
    obs.next('a value');
    // obs.error('error occurs');
    obs.complete();
    obs.next('never called since error or complete makes subscription of observable is completed.');
  })
  .subscribe(observer);
```

Don't forget to unsubscribe.

```javascript
const sub = Rx.Observable
  .create(function(obs) {
    button.onclick = function(e){
      obs.next(e);
    }
  })
  .subscribe(observer);

setTimeout(function(){
  sub.unsubscribe();
}, 3000);
```

### interval

```javascript
Rx.Observable.interval(1000)    // generate 0, 1, 2 ... every 1s
  .map((v) => 'num: ' + v * 2)  // transform and return observable
  .throttleTime(1500)           // throttle
  .subscribe({
    next: function(val) {
      console.log(val);
    }
  });
```

## Operators: map, filter, throttleTime

```javascript
Rx.Observable.interval(1000)    // generate 0, 1, 2 ... every 1s
  .filter((v) => v % 2 === 0)   // filter out even numbers
  .throttleTime(1500)           // throttle
  .subscribe({
    next: function(val) {
      console.log(val);
    }
  });
```

### debounceTime, distinctUntilChanged

```javascript
const input = document.querySelector('input');

Rx.Observable.fromEvent(input, 'input')
  // .map(event => event.target.value)
  .pluck('target', 'value') // extract properties from object. Shorter than map
  .debounceTime(500)        // 500ms no input, emit value
  .distinctUntilChanged()   // usually used with `Map` or 'pluck' so it's easier to know whether new value is different from old one. When new and old are the same, no execution of subscribe method
  .subscribe({
    next(res) {
      console.log(res);
    }
  });
  // .subscribe(res => console.log(res));
```

### scan, reduce

```javascript
Rx.Observable.of(1, 2, 3, 4, 5)
  .scan((accu, curr) => {
    return accu + curr;
  }, 0)
  // .reduce((accu, curr) => {
  //   return accu + curr;
  // }, 0)
  .subscribe(val => {
    console.log(val);
  })

  // scan result: 1 3 6 10 15
  // reduce result: 15, final result
```

### mergeMap

combine 2 observables into one.

```html
<body>
  <input type="text">
  <input type="text">
  <p>combined value: <span></span></p>
</body>
<script src="https://unpkg.com/@reactivex/rxjs@5.0.0-beta.12/dist/global/Rx.js"></script>
<script>
  const inputs = document.querySelectorAll('input');
  const input1 = inputs[0];
  const input2 = inputs[1];

  const span = document.querySelector('span');

  const obs1 = Rx.Observable.fromEvent(input1, 'input');
  const obs2 = Rx.Observable.fromEvent(input2, 'input');

  // when the second input has any value, the combined value will be filled into span.
  obs1.mergeMap(event1 => {
    return obs2.map(event2 => event1.target.value + ' ' + event2.target.value);
  })
  .subscribe(res => span.textContent = res);
</script>
```

### switchMap

```javascript
const button = document.querySelector('button');

const obs1 = Rx.Observable.fromEvent(button, 'click');
const obs2 = Rx.Observable.interval(1000);

// trigger some value emissions whenever another Observable emits a value
// when button clicks, obs2 will be subscribed
obs1.switchMap(event => obs2)
  .subscribe(val => console.log(val));
```