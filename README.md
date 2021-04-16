# microstats

> microstats is a node utility that can be used to monitor OS events such as CPU utilization, memory and disk consumption. It can be used to 'alert' the user when user defined thresholds are breached. Currently available for linux, macos and windows. 

## Install

```bash
npm install microstats
```

## Usage

```javascript
const microstats = require('microstats')

// Event emits
microstats.on('memory', value => { console.log('MEMORY: ${value}') })
microstats.on('cpu',    value => { console.log('CPU: ${value}') })
microstats.on('disk',   value => { console.log('DISK: ${value}') })
microstats.on('error',  value => { console.log('ERROR: ${value}') })

let options = {}
microstats.start(options, function(err) {
  if (err) console.log(err);
})

// ...

microstats.stop();
```

## options

3 modes of operation - 

- 'once': Will check all stats, report current numbers and stop.

```javascript
let options = { frequency: 'once' }
```

- 'timer': Will check all stats periodically on a user defined timer and report each check.

```javascript
let options = { frequency: '5s' }
```

Values for frequency could be something like

```javascript
'5s' // 5 seconds
'5m' // 5 minutes
'5h' // 5 hours
```

- 'onalert': Will check all stats periodically, but report only when the numbers exceed user defined threshold.

```javascript
options = {
  frequency: 'onalert',
  memoryalert: { used: '>60%' },
  cpualert: { load: '>80%' },
  diskalert: { used: '>70%' }
}
```
If 'onalert' is used without specifying the thresholds, a default threshold value of 50% will be used for all stats.

## diskalert 

diskalert options can be customized to specify the disks / mounts to be monitored. If no 'filesystem(s)' or 'mount(s)' are specified, all the available disks will be considered. 

- linux/macos example(s)
```javascript
options = {
  frequency: 'once',
  diskalert: { 
    used: '>70%',
    mount: '/'
  }
}

options = {
  frequency: 'once',
  diskalert: {
    used: '>70%',
    mounts: ['/home', '/dev']
  }
}
```

- windows example(s)
```javascript
options = {
  frequency: 'once',
  diskalert: {
    used: '>70%',
    filesystem: 'C:'
  }
}

options = {
  frequency: 'once'
  diskalert: {
    used: '>70%',
    filesystems: ['C:', 'D:']
  }
}
```

## Sample output
```bash
MEMORY: { usedpct: 55.33, total: 34359738368, free: 15349305344 }
DISK: { filesystem: '/dev/disk0s2',
  mount: '/',
  usedpct: 40.64,
  total: 976265452,
  free: 579478940 }
DISK: { filesystem: '/dev/disk1s2',
  mount: '/Volumes/MyPhotos',
  usedpct: 88.05,
  total: 246865880,
  free: 29491752 }
CPU: { loadpct: 39.28, userpct: 10.71, syspct: 28.57, idlepct: 60.71 }
```

## License

[MIT](http://vjpr.mit-license.org)
