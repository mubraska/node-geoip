geoip-lite-country
==========

A native NodeJS API for the GeoLite data from MaxMind.

This product includes GeoLite data created by MaxMind, available from http://maxmind.com/
It is a simplified version of https://www.npmjs.com/package/geoip-lite that only includes country data.

Usage
--------

```javascript
var geoip = require('geoip-lite-country');

var ip = "207.97.227.239";
var geo = geoip.lookup(ip);

console.log(geo);
{
  country: 'US'
}
```

installation
------------
### 1. get the library

    $ npm install geoip-lite-country

### 2. update the datafiles (optional)

Run `npm run-script updatedb` to update the data files.

**NOTE** that this requires a lot of RAM.  It is known to fail on on a Digital Ocean or AWS micro instance.
There are no plans to change this.  `geoip-lite-country` stores all data in RAM in order to be fast.

API
---

geoip-lite is completely synchronous.  There are no callbacks involved.  All blocking file IO is done at startup time, so all runtime
calls are executed in-memory and are fast.  Startup may take up to 200ms while it reads into memory and indexes data files.

### Looking up an IP address ###

If you have an IP address in dotted quad notation, IPv6 colon notation, or a 32 bit unsigned integer (treated
as an IPv4 address), pass it to the `lookup` method.  Note that you should remove any `[` and `]` around an
IPv6 address before passing it to this method.

```javascript
var geo = geoip.lookup(ip);
```

If the IP address was found, the `lookup` method returns an object with the following structure:

```javascript
{
   country: 'XX',                 // 2 letter ISO-3166-1 country code
}
```

If the IP address was not found, the `lookup` returns `null`

### Start and stop watching for data updates ###

If you have a server running `geoip-lite-country`, and you want to update its geo data without a restart, you can enable
the data watcher to automatically refresh in-memory geo data when a file changes in the data directory.

```javascript
geoip.startWatchingDataUpdate();
```

This tool can be used with `npm run-script updatedb` to periodically update geo data on a running server.


Built-in Updater
----------------

This package contains an update script that can pull the files from MaxMind and handle the conversion from CSV.
A npm script alias has been setup to make this process easy. Please keep in mind this requires internet and MaxMind
rate limits that amount of downloads on their servers.

```shell
npm run-script updatedb
```

You can also run it by doing:

```bash
node ./node_modules/geoip-lite/scripts/updatedb.js
```

Or, if you really want, run the update once by `require('geoip-lite-country/scripts/updatedb.js')`.

Caveats
-------

This package includes the GeoLite database from MaxMind.  This database is not the most accurate database available,
however it is the best available for free.  You can use the commercial GeoIP database from MaxMind with better
accuracy by buying a license from MaxMind, and then using the conversion utility to convert it to a format that
geoip-lite understands.  You will need to use the `.csv` files from MaxMind for conversion.

Also note that on occassion, the library may take up to 5 seconds to load into memory.  This is largely dependent on
how busy your disk is at that time.  It can take as little as 200ms on a lightly loaded disk.  This is a one time
cost though, and you make it up at run time with very fast lookups.

References
----------
  - <a href="http://www.maxmind.com/app/iso3166">Documentation from MaxMind</a>
  - <a href="http://en.wikipedia.org/wiki/ISO_3166">ISO 3166 (1 & 2) codes</a>
  - <a href="http://en.wikipedia.org/wiki/List_of_FIPS_region_codes">FIPS region codes</a>

Copyright
---------

`geoip-lite-country` is Copyright 2017-2018 David Ventura <davidventura3@gmail.com> and the latest version of the code is
available at https://github.com/mubraska/node-geoip

License
-------

There are two licenses for the code and data.  See the [LICENSE](https://github.com/mubraska/node-geoip/master/LICENSE) file for details.
