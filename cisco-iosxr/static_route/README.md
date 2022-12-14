# How to find the correct key/values from the Cisco YANG Model

You need to download the Juniper YANG models located here:

`git clone https://github.com/YangModels/yang`

You then need to navigate to the correct path for the software version eg.:

`yang/vendor`

 
 # How to apply this configuration to the device
 
 You will use a GNMI set operation to apply this configuration to the device. 
 Note this is a merge operation so if there is existing configuration then there may be a conflict
 Please change the credentials and GNMI Port as appropriate for your setup

```
gnmic -a 172.21.20.7:57400 -u admin -p admin@123 \
    --insecure set \
    --update-path 'Cisco-IOS-XR-ip-static-cfg:router-static'\
    --update-file 'iosxr_static.json' \
    -e json_ietf --log
```

or

```
gnmic -a 172.21.20.7:57400 -u admin -p admin@123 \
    --insecure set \
    --update-path 'Cisco-IOS-XR-ip-static-cfg:router-static'\
    --update-file 'iosxr_static.yaml' \
    -e json_ietf --log
```

The configuration applied would be like the one below:
![Screenshot from 2022-12-14 12-10-48](https://user-images.githubusercontent.com/63735312/207592030-2fb293c6-7d63-4656-bbd5-38e17375d2b4.png)
