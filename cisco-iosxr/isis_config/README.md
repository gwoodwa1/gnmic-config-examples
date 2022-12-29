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
gnmic -a 172.21.20.1:57400 -u clab -p clab@123 \                                                                                                         
    --insecure set \
    --update-path 'Cisco-IOS-XR-clns-isis-cfg:isis'\
    --update-file 'iosxr_isis.json' \
    -e json_ietf --log

```

or

```
gnmic -a 172.21.20.1:57400 -u clab -p clab@123 \                                                                                                         
    --insecure set \
    --update-path 'Cisco-IOS-XR-clns-isis-cfg:isis'\
    --update-file 'iosxr_isis.json.yaml' \
    -e json_ietf --log
```
The configuration it generates will be as per below:
![Screenshot from 2022-12-29 09-33-37](https://user-images.githubusercontent.com/63735312/209932401-daa5613d-3889-4d3f-acc7-7a6adfd3edda.png)


