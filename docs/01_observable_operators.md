# Observable operators: fromEvent, create, interval

## fromEvent

```javascript
const button = document.querySelector('button');
// button.addEventListener('click', e => console.log(e));

Rx.Observable.fromEvent(button, 'click')
    .throttleTime(1000)
    .map(data => (data.clientX))
    .subscribe(coordinate => console.log(coordinate));
```

## create

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

## interval

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