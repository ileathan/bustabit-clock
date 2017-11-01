# bustabit-clock
Simple clock to show in real time whats being wagered (denominated in satoshi's)

**Copy paste the bellow into bustabit custom script area, then open the debug console and watch**

```javascript
var round_total = 0; 
var cashed_total = 0;
var usersObj = {};

engine.on('game_crash', function(data) {
  round_total = 0;
});
engine.on('game_started', function(data) { 
  cashed_total = 0;
  for(i in data) {
    round_total += data[i].bet;
    usersObj[i] = data[i].bet;
  }
})
engine.on('cashed_out', function(user) { 
  let userCashed = usersObj[user.username] * (user.stopped_at - 100) / 100;
  round_total -= userCashed;
  cashed_total += userCashed;
  console.log(round_total);
})
```
