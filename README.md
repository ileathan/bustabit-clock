# bustabit-clock
Simple clock to show in real time whats being wagered (denominated in bits)

**Copy paste the bellow into bustabit custom script area, then open the debug console and watch**

```javascript
var round_total = 0; 
var usersObj = {};
var cashed_total = 0;
var still_wagered = 0;

engine.on('game_crash', function() {
  console.log("Total cashed out: " + cashed_total / 100 + "  [" + parseInt((cashed_total / round_total) * 100) + "%].") 
  round_total = 0;
});
engine.on('game_started', function(users) { 
  cashed_total = 0;
  for(name in users) {
    round_total += users[name].bet;
    usersObj[name] = users[name].bet;
  }
  still_wagered = round_total;
})
engine.on('cashed_out', function(user) { 
  let userCashed = usersObj[user.username] * (user.stopped_at - 100) / 100;
  still_wagered -= userCashed;
  cashed_total += userCashed;
  console.log(still_wagered / 100 + "  [" + parseInt((still_wagered / round_total) * 100) + "%]");
})
```
