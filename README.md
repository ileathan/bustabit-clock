# bustabit-clock
Simple clock to show in real time whats being wagered (denominated in bits)

**Copy paste the bellow into bustabit custom script area, then open the debug console and watch**

```javascript
var round_total = 0; 
var usersObj = {};
var cashed_total = 0;
var still_wagered = 0;
var buffer; // remove dups

engine.on('game_crash', function() {
  console.log("Total cashed out: " + parseInt(cashed_total / 100) + "  [" + parseInt(100 - (still_wagered / round_total) * 100) + "%].") 
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
  //still_wagered -= userCashed;
  still_wagered -= usersObj[user.username];
  cashed_total += userCashed;
  let left = parseInt(still_wagered / 100);
  if(buffer === left) return;
  else buffer = left;
  console.log(left + "  [" + parseInt((still_wagered / round_total) * 100) + "%]");
})
```
