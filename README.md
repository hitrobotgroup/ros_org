# rosbridge_shell

***

# rosbridge_driver

0. rosbridge_driver node

    0. Subscribed Topics

    0. Published Topics

    0. Parameters
        * ~port_chain
            Iterate over all ports (may be just one)
        * ~topic_chain
            Iterate over all topics (may be just one)

    0. Example Launch File
    
        ```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" xmlns="XmlRpc/XmlRpcValue" href="http://xmlrpcpp.sourceforge.net/doc/classXmlRpc_1_1XmlRpcValue.html"?>
<launch>
    <node name="rosbridge_driver_node" pkg="rosbridge_driver" type="rosbridge_driver" output="screen">
        <param name="foo_port" value="$(arg foo_port)"/> <!-- optional: must be same with the target port name -->
        <rosparam>
            baz_qux: true <!-- bool: enable topic with name "baz" and type "qux" -->
            port_chain: <!-- required -->
              - name: foo_port <!-- required -->
                type: serial <!-- serial / socket -->
                serial_port: /dev/ttyUSB0 <!-- optional: be valid only if the "type" of this port is "serial" and the optional "foo_port" param is missing -->
                serial_baud: 115200 <!-- 115200 / 57600 / 38400 / 19200 / 9600 / 4800 / 2400 -->
                serial_bits: 8 <!-- 8 / 7 -->
                serial_parity: 'N' <!-- 'N' / 'E' / 'O' -->
                serial_stop: 2 <!-- 2 / 1 -->
                ip_address: 192.168.0.7 <!-- optional: be valid only if the "type" of this port is "socket" -->
                ip_port: 8080 <!-- int: be valid only if the "type" of this port is "socket" -->
                port_delay: 200000 <!-- int: wait (200000) us for the port initialization -->
                respawn: true <!-- bool: reopen this port if closed for hot plug -->
                timeout: 0.01 <!-- optional: be valid only if the "time_out" of the source action is valid -->
                action_chain: <!-- optional -->
              - name: bar_port <!-- required -->
            topic_chain: <!-- required -->
              - name: foo <!-- required -->
                type: event <!-- event / subscribe / publish --><!-- subscribe: cmd_vel / waypoint_user_pub --><!-- publish: odom / sonar / diagnostics -->
                action_chain: <!-- optional -->
                  - name: foo_port <!-- required: must be same with the target port name -->
                    type: write <!-- required: write / read -->
                    time_out: 0.01 <!-- optional: replace the "timeout" of the target port -->
                    byte: [0x00, foo_data, big, sum8] <!-- required: sum8 / crc16 / big / little-->
                    checksum_bias: 0 <!-- int: bias for sum8 or crc16 check -->
                    lock: true <!-- bool: lock the target port for reading-->
                  - name: foo_port <!-- required: must be same with the target port name -->
                    type: read <!-- required: write / read -->
                    byte: [0x00, bar_data, double, double, double, crc16, little] <!-- required: double / errno / sum8 / crc16 / big / little-->
                    except: [0x00, errno]
                    lock: false <!-- bool: lock the target port for writing or reading -->
                  - name: bar_port <!-- required -->
                foo_data: 0 <!-- optional: must be same with the target data in this action "byte" array -->
                bar_data: 0.0 <!-- optional -->
              - name: bar <!-- required -->
        </rosparam>
    </node>
</launch>
        ```

0. cmd_vel plugin

0. odom plugin

0. imu plugin

0. sonar plugin
