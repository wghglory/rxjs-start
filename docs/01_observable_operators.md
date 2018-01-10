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
