panonoctl
========

Python API to interact with the [PANONO](https://www.panono.com) 360-camera.

Install
=======

To install, execute:

```
pip install panonoctl
```

Documentation
=============

### Status
[panonoctl](https://github.com/florianl/panonoctl) is tested under API version 4.23 with firmware 4.2.873.

### Example Script
A very basic script, which downloads all UPFs from your [PANONO](https://www.panono.com) can be found in [getAllUpfs.py](getAllUpfs.py).

### Connect
```python
>>> from panonoctl import panono
>>> cam = panono()
>>> cam.connect()
```

### Authenticate
```python
>>> cam.auth()
```
You need to authenticate. Otherwise your commands will _not_ be executed.

### Take a Picture
```python
>>> cam.capture()
```

### Get the status of your Panono.
```python
>>> cam.getStatus()
```
Returns a JSON object.

### Get the options of your Panono.
```python
>>> cam.getOptions()
```
Returns a JSON object.

### Get the UPFs (Unstitched Panorama Format) from your Panono.
```python
>>> cam.getUpfs()
```
Returns a JSON object.

### Disconnect
```python
>>> cam.disconnect()
```

### Other Features
[PANONO](https://www.panono.com) provides more features, than those listed above.
If you are interested in trying your own commands take a look [here](Experimental.md) for further information.

### Download UPF files

Make sure this camera has atleast one picture, otherwise the code below will fail.

You need to first connect to the camera via either Ethernet or WIFI. To connect via Ethernet, plug-in the USB cord, and change the setting of the camera to 'Network' mode (via camera settings from either the iOS or Android Panono app.)

When using ethernet, ```ip="192.168.10.1" port="42345"```, but the `ip` could be different on your system; on all the systems I've used (including Windows and Linux systems), it was always this one, though. On a Linux machine (and Windows too I think) you can figure this out by doing an arp scan: `arp -a`.

When using wifi, ```ip="192.168.80.80",port="42345"```.

```python
~ $ python
>>> from panonoctl import panono
>>> cam = panono.panono(ip="192.168.10.1",port="42345")
>>> cam.connect()
True
>>> cam.auth()
(output elided)
>>> url = result.get('result').get('upf_infos')[0].get('upf_url')
>>> import requests
>>> response = requests.get(url)
>>> file = open("/tmp/some.upf", "w")
>>> file.write(response.content)
>>> file.close()
```

That will download the file to /tmp/some.upf.

License
=======

Copyright 2016 Florian Lehner

Licensed under the Apache License, Version 2.0: [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)
