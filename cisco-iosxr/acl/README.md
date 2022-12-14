# How to find the correct key/values from the Cisco YANG Model

You need to download the Juniper YANG models located here:

`git clone https://github.com/YangModels/yang`

You then need to navigate to the correct path for the software version eg.:

`yang/vendor`

 
 # How to apply this configuration to the device
 
 You will use a GNMI set operation to apply this configuration to the device. 
 Note this is a merge operation using `update` so if there is existing configuration then there may be a conflict
 Please change the credentials and GNMI Port as appropriate for your setup. You could also do a `replace` operation instead of a `update`.
 Now if you do a `replace` it will wipe all the configuration under that branch and replace it with what you specifiy in the file. In this case it would  remove any other ACLs that you have in place and replace it with this one. Therefore use the `replace` operation with caution `--replace` path and `--replace-file`

```
gnmic -a 172.21.20.7:57400 -u admin -p admin@123 \
    --insecure set \
    --update-path 'Cisco-IOS-XR-ipv4-acl-cfg:ipv4-acl-and-prefix-list'\
    --update-file 'iosxr_acl_native.json' \
    -e json_ietf --log
```

or

```
gnmic -a 172.21.20.7:57400 -u admin -p admin@123 \
    --insecure set \
    --update-path 'Cisco-IOS-XR-ipv4-acl-cfg:ipv4-acl-and-prefix-list'\
    --update-file 'iosxr_acl_native.yaml' \
    -e json_ietf --log
```
This will produce the following configuration below:
![Screenshot from 2022-12-14 12-07-52](https://user-images.githubusercontent.com/63735312/207591489-01c1ce52-a4f4-4696-83bc-a4284cdbf834.png)


