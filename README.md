# bustabit-clock
Simple clock to show in real time whats being wagered (denominated in bits)

**Copy paste the bellow into bustabit custom script area, then open the debug console and watch**

```javascript
var round_total = 0; 
var usersObj = {};
var cashed_total = 0;
var still_wagered = 0;
var still_wagered_buff; // remove dups

function satsToBits(satoshis) { return parseInt(satoshis / 100) }

engine.on('game_crash', function() {
  let percent_left = parseInt(100 - (still_wagered / round_total) * 100);
  engine.chat(cashed_total + " bits cashed out  [" + percent_left + "%].")
  console.log("Total cashed out: " + cashed_total + "  [" + percent_left + "%].") 
  round_total = 0;
});
engine.on('game_started', function(users) { 
  cashed_total = 0;
  for(name in users) {
    round_total += satsToBits(users[name].bet);
    usersObj[name] = satsToBits(users[name].bet);
  }
  still_wagered = satsToBits(round_total);
})
engine.on('cashed_out', function(user) { 
  let userCashed = satsToBits(usersObj[user.username] * (user.stopped_at - 100) / 100);
  still_wagered -= usersObj[user.username];
  cashed_total += userCashed;
  // Here we keep track of the last number, and if its the same we dont print it out (ignore duplicates)
  if(still_wagered_buff === still_wagered) return;
  else still_wagered_buff = still_wagered;
  console.log(still_wagered + "  [" + parseInt((still_wagered / round_total) * 100) + "%]");
})
```
