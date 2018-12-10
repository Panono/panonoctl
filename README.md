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
{"jsonrpc": "2.0", "params": {"device": "test", "force": "test"}, "id": 1, "method": "auth"}
>>> result = cam.experimental("get_upf_infos")
{'jsonrpc': '2.0', 'id': 1, 'result': {'battery_value': -1, 'log_serialize_ready': True, 'auth_token': '3d327e8ca292310510b51e8aac00c0251d9ca55852a9781e31c8cd83e68ebe81920ea13d931735790ab89ff2aa50c87a38f9dfde7aa2370e2ead011b48903dc2', 'power_mode_config': 'eco', 'firmware_version': '6.1.916', 'usb_config': 'network', 'current_time': '2018-11-06 10:22:48,000', 'power_charging_status': 'charging', 'sound_config': 'silent', 'storage': {'panorama': {'usage': 266760192, 'total': 13597483008, 'capacity_available': True}, 'cache': {'usage': 231936000, 'total': 1323102208, 'capacity_available': True}}, 'capture_active': False, 'scheduled_captures': [], 'power_status': 'connected', 'serial_number': 'P6C4AS', 'api_version': '4.54', 'auto_poweroff_count_down': 29.661111332999997, 'is_auth': True, 'capture_available': True, 'update_ready': False, 'charging_status': 'charging', 'accessory': [], 'firmware_update_url': 'http://192.168.10.1:80/update_image', 'capture_locked': False, 'timer_active': False, 'device_id': '19b58e3d52ae50167d1558399bef6de4', 'auto_poweroff_config': True, 'auto_poweroff_timer_config': 30}}
>>> url = result.get('result').get('upf_infos')[0].get('upf_url')
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
