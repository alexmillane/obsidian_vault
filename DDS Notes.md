#ros #nvblox #dds #rtps #fast-rtps
# Ros example debugging 2022.11.29

The tf2_buffer object was loosing connection to the /tf topic permeanantly.

I changed the launchfile to put all components into nodelets but run each in its container. This makes no sense but it's what I had to do.

Also potentially important were the settings for fastRTPS message delivery

One time expansion:
`sudo sysctl -w net.core.rmem_max=2147483647`

Permeanant expansion
`echo "net.core.rmem_max=8388608\nnet.core.^Cem_default=8388608\n" | sudo tee /etc/sysctl.d/60-cyclonedds.conf`

FastRTPS config:
```
<?xml version="1.0" encoding="UTF-8" ?>
<profiles xmlns="http://www.eprosima.com/XMLSchemas/fastRTPS_Profiles" >
    <transport_descriptors>
        <transport_descriptor>
            <transport_id>CustomUdpTransport</transport_id>
            <type>UDPv4</type>
            <maxInitialPeersRange>400</maxInitialPeersRange>
        </transport_descriptor>
    </transport_descriptors>

    <participant profile_name="participant_profile" is_default_profile="true">
        <rtps>
            <userTransports>
                <transport_id>CustomUdpTransport</transport_id>
            </userTransports>

            <useBuiltinTransports>false</useBuiltinTransports>
        </rtps>
    </participant>
</profiles>
```

# General Notes from ROS
Maybe we should do these some time.

[https://docs.ros.org/en/rolling/How-To-Guides/DDS-tuning.html](https://docs.ros.org/en/rolling/How-To-Guides/DDS-tuning.html)



