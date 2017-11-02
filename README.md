# bustabit-clock
Simple clock to show in real time whats being wagered (denominated in bits)

**Copy paste the bellow into bustabit custom script area, then open the debug console and watch**

```javascript
var round_total = 0; 
var usersObj = {};
var cashed_total = 0;
var still_wagered = 0;
var bits_left; // remove dups

function satsToBits(satoshis) { return parseInt(satoshis / 100) }

engine.on('game_crash', function() {
  engine.chat(parseInt(cashed_total / 100) + " bits cashed out  [" + parseInt(100 - (still_wagered / round_total) * 100) + "%].")

  console.log("Total cashed out: " + parseInt(cashed_total / 100) + "  [" + parseInt(100 - (still_wagered / round_total) * 100) + "%].") 
  round_total = 0;
});
engine.on('game_started', function(users) { 
  cashed_total = 0;
  for(name in users) {
    round_total += users[name].bet;
    usersObj[name] = satsToBits(users[name].bet);
  }
  still_wagered = satsToBits(round_total);
})
engine.on('cashed_out', function(user) { 
  let userCashed = usersObj[user.username] * (user.stopped_at - 100) / 100;
  still_wagered -= usersObj[user.username];
  cashed_total += userCashed;
  // Here we keep track of the last number, and if its the same we dont print it out (ignore duplicates)
  if(bits_left === still_wagered) return;
  else bits_left = still_wagered;
  console.log(left + "  [" + parseInt((still_wagered / round_total) * 100) + "%]");
})
```
