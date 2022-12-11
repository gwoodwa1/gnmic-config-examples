

# How to find the correct key/values from the Juniper YANG Model

You need to download the Juniper YANG models located here:

`git clone https://github.com/Juniper/yang`

You then need to navigate to the correct path for the software version eg.:

`/yang/22.2/22.2R1`

You can then use the following on GNMIC:

```
gnmic --encoding json_ietf \
 generate --file junos/conf \
 --dir common set-request \
 --replace /configuration/interfaces/interface/ >> interfaces_full_model.yaml 
 ```
 
 This will create the full model as per what is in the repo with all the keys and empty values. Copy in the repo
 
 # How to apply this configuration to the device
 
 You will use a GNMI set operation to apply this configuration to the device. 
 Note this is a merge operation so if there is existing configuration then there may be a conflict
 Please can the credentials and GNMI Port as appropriate for your setup

```
gnmic -a 172.21.20.7:57400 -u admin -p admin@123 \
    --insecure set \
    --update-path 'juniper:/configuration/interfaces'\
    --update-file 'junos_intf_example.json' \
    -e json_ietf --log
```
