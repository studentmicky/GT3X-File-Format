# ActiGraph .gt3x File Format

## Introduction

This documentation provides information on getting activity data out of [ActiGraph](http://www.actigraphcorp.com/ "ActiGraph site") .gt3x files. 

## Valid .gt3x Files ##

This documentation is valid for .gt3x files downloaded from the following devices:
* GT3X+ (serial numbers starting with NEO and firmware 3.0 and higher)
* wGT3X+ (serial numbers starting with CLE)
* wGT3X-BT (serial numbers starting with MOS0 and MOS2)
* ActiSleep+ (serial numbers starting with MRA and firmware 3.0 and higher)
* wActiSleep+ (serial numbers starting with MOS3
* wActiSleep-BT (serial numbers starting with MOS4)
* GT9X Link (serial numbers starting with TAS) 

## Invalid .gt3x Files ##
**NOTE:** Devices with serial numbers that start with "NEO" or "MRA" and have firmware version of 2.5.0 or earlier use an older format of the .gt3x file. Please see this GitHub repo for more information: https://github.com/actigraph/NHANES-GT3X-File-Format

## Log Records ##
Binary .gt3x file data is grouped into timestamped records of varying types that are written sequentially as the data becomes available on the activity monitor. The format is similar to common protocols used for serial communication. Each log record includes a header with a record separator, record type, timestamp and payload size. After the variable length payload is a checksum for ensuring data integrity.

### Log Record Format ###
<table>
    <tr>
        <th>Offset (bytes)</th>
        <th>Size (bytes)</th>
        <th>Name</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>0</td>
        <td>1</td>
        <td>Seperator</td>
        <td>An ASCII record separator byte (1Eh) marks the beginning of each log record.</td>
    </tr>
    <tr>
        <td>1</td>
        <td>1</td>
        <td>Type</td>
        <td>A type identifier is used to interpret the payload of the record.</td>
    </tr>
    <tr>
        <td>2</td>
        <td>4</td>
        <td>Timestamp</td>
        <td>The date and time of the data contained in the record are marked to the nearest second in <a href="http://en.wikipedia.org/wiki/Unix_time">Unix time</a> format.</td>
    </tr>
    <tr>
        <td>6</td>
        <td>2</td>
        <td>Size</td>
        <td>The size of the payload is given in bytes as an little-endian unsigned integer.</td>
    </tr>
    <tr>
        <td>8</td>
        <td>n</td>
        <td>Payload</td>
        <td>This is the actual data that varies based on the record *Type* field. It's size is provided in the *Size* field. Please refer to the appropriate section for the record type for the indiviual payload formats.</td>
    </tr>
    <tr>
        <td>8 + n</td>
        <td>1</td>
        <td>Checksum</td>
        <td>A 1-byte checksum immediately follows the record payload. It is a 1's complement, exclusive-or (XOR) of the log header and payload with an initial value of zero.</td>
    </tr>
</table>

### Log Record Types ###
<table>
   <tr>
      <th>ID</th>
      <th>Type</th>
      <th>Description</th>
   </tr>
   <tr>
      <td>0</td>
      <td><a href=LogRecords/Activity.md>ACTIVITY</a></td>
      <td>One second of raw activity samples packed into 12-bit values in YXZ order</td>
   </tr>
   <tr>
      <td>1</td>
      <td>Spare</td>
      <td></td>
   </tr>
   <tr>
      <td>2</td>
      <td><a href=LogRecords/Battery.md>BATTERY</a></td>
      <td>Battery voltage</td>
   </tr>
   <tr>
      <td>3</td>
      <td>Debug</td>
      <td>This record type is used for internal testing. It should be ignored if encountered while parsing a file.</td>
   </tr>
   <tr>
      <td>4</td>
      <td><a href=LogRecords/HeartRateBPM.md>HEART_RATE_BPM</a></td>
      <td></td>
   </tr>
   <tr>
      <td>5</td>
      <td><a href=LogRecords/Lux.md>LUX</a></td>
      <td>Lux</td>
   </tr>
   <tr>
      <td>6</td>
      <td><a href=LogRecords/Metadata.md>METADATA</a></td>
      <td>Arbitrary metadata content</td>
   </tr>
   <tr>
      <td>7</td>
      <td><a href=LogRecords/Tag.md>TAG</a></td>
      <td>13 Byte Serial, 1 Byte Tx Power, 1 Byte (signed) RSSI</td>
   </tr>
   <tr>
      <td>8</td>
      <td><a href=LogRecords/Temperature.md>TEMPERATURE</a></td>
      <td></td>
   </tr>
   <tr>
      <td>9</td>
      <td><a href=LogRecords/Epoch.md>EPOCH</a></td>
      <td></td>
   </tr>
   <tr>
      <td>10</td>
      <td>DAILY_SUMMARY</td>
      <td>These records were used internally for devices that provided daily summary data over wireless to restore from a power loss. They should be ignored when parsing a file.</td>
   </tr>
   <tr>
      <td>11</td>
      <td><a href=LogRecords/HeartRateAnt.md>HEART_RATE_ANT</a></td>
      <td></td>
   </tr>
   <tr>
      <td>12</td>
      <td><a href=LogRecords/Epoch2.md>EPOCH2</a></td>
      <td>Cletus & Moses epoch data</td>
   </tr>
   <tr>
      <td>13</td>
      <td><a href=LogRecords/Capsense.md>CAPSENSE</a></td>
      <td>Capacitive sense data</td>
   </tr>
   <tr>
      <td>14</td>
      <td><a href="https://developer.bluetooth.org/gatt/characteristics/Pages/CharacteristicViewer.aspx?u=org.bluetooth.characteristic.heart_rate_measurement.xml">HEART_RATE_BLE</a></td>
      <td>Bluetooth heart rate information (BPM and RR). This is a Bluetooth standard format.</td>
   </tr>
   <tr>
      <td>15</td>
      <td><a href=LogRecords/Epoch3.md>EPOCH3</a></td>
      <td>Moses epoch data</td>
   </tr>
   <tr>
      <td>16</td>
      <td><a href=LogRecords/Epoch4.md>EPOCH4</a></td>
      <td>Taso epoch data</td>
   </tr>
   <tr>
      <td>17</td>
      <td>Spare</a></td>
      <td></td>
   </tr>
   <tr>
      <td>18</td>
      <td>Spare</a></td>
      <td></td>
   </tr>
   <tr>
      <td>19</td>
      <td>Debug</td>
      <td>This record type is used for internal testing. It should be ignored if encountered while parsing a file.</td>
   </tr>
   <tr>
      <td>20</td>
      <td>Debug</td>
      <td>This record type is used for internal testing. It should be ignored if encountered while parsing a file.</td>
   </tr>
   <tr>
      <td>21</td>
      <td><a href=LogRecords/Parameters.md>PARAMETERS</a></td>
      <td>Records various configuration parameters and device attributes on initialization.</td>
   </tr>
   <tr>
      <td>22</td>
      <td>18</td>
      <td>Spare</a></td>
      <td></td>
   </tr>
   <tr>
      <td>23</td>
      <td>Debug</td>
      <td>This record type is used for internal testing. It should be ignored if encountered while parsing a file.</td>
   </tr>
   <tr>
      <td>24</td>
      <td><a href=LogRecords/SensorSchema.md>SENSOR_SCHEMA</a></td>
      <td>This record allows dynamic definition of a SENSOR_DATA record format.</td>
   </tr>
   <tr>
      <td>25</td>
      <td><a href=LogRecords/SensorData.md>SENSOR_DATA</a></td>
      <td>This record stores sensor data according to a SENSOR_SCHEMA definition.</td>
   </tr>
   <tr>
      <td>26</td>
      <td><a href=LogRecords/Activity2.md>ACTIVITY2</a></td>
      <td>One second of raw activity samples as little-endian signed-shorts in XYZ order.</td>
   </tr>
</table>

**Prepared By:**

* [Daniel Judge](https://github.com/dwjref "Daniel's GitHub Profile") - Software Architect
* [Judge Maygarden](https://github.com/jmaygarden "Judge's GitHub Profile") - Hardware/Firmware Engineer
