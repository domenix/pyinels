## Commit id: 05004e47

Error:

```
Traceback (most recent call last):    
  File "test.py", line 21, in <module>
    allDevices = api_obj.getAllDevices()
  File "/mnt/d/Projects/Laci_okoshaz/pyinels/pyinels/api/__init__.py", line 106, in getAllDevices      
    room_devices = self.getRoomDevices(room)
  File "/mnt/d/Projects/Laci_okoshaz/pyinels/pyinels/api/__init__.py", line 95, in getRoomDevices      
    return self.__roomDevicesToJson(room_name)
  File "/mnt/d/Projects/Laci_okoshaz/pyinels/pyinels/api/__init__.py", line 211, in __roomDevicesToJson
    obj[frag[0]] = frag[1].replace("\"", " ").strip()
IndexError: list index out of range
```

Cause of error:  
A "blank" category exists in the Inels system, e.g.:
```
blank:
name=" " column="1" inels="ejfel_bit" read_only="no" row="3"
```

This causes the dissection of fragments to return the following:
```
['name', '']
['']
```

An example for a healthier fragment list can be seen below:
```
name="öntözési mennyiség" inels="ont_mennyiseg_sz" min_disp="0" column="0" koef_mult="1.0" max_disp="12" decimal_digits="1" koef_add="0.0" units="mm/m2" row="3"

['name', '"öntözési mennyiség']
['inels', '"ont_mennyiseg_sz']
['min_disp', '"0']
['column', '"0']
['koef_mult', '"1.0']
['max_disp', '"12']
['decimal_digits', '"1']
['koef_add', '"0.0']
['units', '"mm/m2']
['row', '"3"']
```

Solution for now:

Ignore the "blank" category altogether before any name and id calculation