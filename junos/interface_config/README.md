

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
 Please change the credentials and GNMI Port as appropriate for your setup

```
gnmic -a 172.21.20.7:57400 -u admin -p admin@123 \
    --insecure set \
    --update-path 'juniper:/configuration/interfaces'\
    --update-file 'junos_intf_example.json' \
    -e json_ietf --log
```

or

```
gnmic -a 172.21.20.7:57400 -u admin -p admin@123 \
    --insecure set \
    --update-path 'juniper:/configuration/interfaces'\
    --update-file 'junos_intf_example.yaml' \
    -e json_ietf --log
```
Screenshot of the action being completed successfully:

![Screenshot from 2022-12-11 15-04-19](https://user-images.githubusercontent.com/63735312/206911365-5762372d-062d-49f7-bdce-20cab0a372eb.png)

 # Troubleshooting
 
 1) Check the syntax in the JSON or YAML file is correct. Anything wrong syntax wise then you will probably get an error like the below which is pretty vague.

 ```
 2022/12/11 14:04:10.501781 [gnmic] creating gRPC client for target "172.21.20.7:57400"
2022/12/11 14:04:10.765570 [gnmic] target "172.21.20.7:57400" set request failed: target "172.21.20.7:57400" SetRequest failed: rpc error: code = Unavailable desc = 
configuration database modified
;
target "172.21.20.7:57400" set request failed: target "172.21.20.7:57400" SetRequest failed: rpc error: code = Unavailable desc = 
configuration database modified
;
Error: one or more requests failed
```
 or something like this:
 
 ```
 2022/12/11 13:05:15.485564 [gnmic] target "172.21.20.7:57400" set request failed: target "172.21.20.7:57400" SetRequest failed: rpc error: code = InvalidArgument desc = syntax error, expecting "@";error recovery ignores input until this point;syntax error, expecting '{';invalid value;parse error;syntax error, expecting "@";error recovery ignores input until this point;syntax error, expecting '}' or :;
target "172.21.20.7:57400" set request failed: target "172.21.20.7:57400" SetRequest failed: rpc error: code = InvalidArgument desc = syntax error, expecting "@";error recovery ignores input until this point;syntax error, expecting '{';invalid value;parse error;syntax error, expecting "@";error recovery ignores input until this point;syntax error, expecting '}' or :;
Error: one or more requests failed
```
if the platform config is incorrect or not configured then you will receive a timeout response like this about the context deadline exceeded:
```
2022/12/11 15:12:15.747824 [gnmic] creating gRPC client for target "172.21.20.8:57400"
2022/12/11 15:12:25.748059 [gnmic] target "172.21.20.8:57400" set request failed: failed to create a gRPC client for target "172.21.20.8:57400" : 172.21.20.8:57400: context deadline exceeded
target "172.21.20.8:57400" set request failed: failed to create a gRPC client for target "172.21.20.8:57400" : 172.21.20.8:57400: context deadline exceeded
Error: one or more requests failed
```
My suggestion for any error is to remove any previous configuration completely and try again in case of a merge conflict. Also worth stripping everything out to a very basic level of key values pairs and retry line by line until you reach the problem area. Pretty painful but it the easiest way of finding where the syntax or format has gone adrift. In this example with the firewall filter, I would suggest leaving the last entry in place and strip out the rest and then add each one back in line by line. Usually you spot something that you have mis-typed. I would recommend using YAML to create and correct the configs rather than trying to edit a JSON file by hand
