[Homie](https://github.com/marvinroger/homie) based firmware for Sonoff Basic, Sonoff S20 wifi relay or any ESP8266 based relay.
This is a fork of https://github.com/enc-X/sonoff-homie

Changes made compared to the enc-X/sonoff-homie are primary intended to fullfill my personal needs. My needs includes the use of Shelly switches, where the "button" is actually the wall switch, and I don't want a configuration reset if someone press the wall switch for 10s. Support must also inlcude toggling wall switches and wall switches with return spring. 

## Features added compared to enc-X/sonoff-homie
* Button status is reported over MQTT
* Configurable behaviour of the button:
  * Toggle relay on press
  * Toggle relay on button change
  * Relay follows button
  * Relay follows button inversed  
  * Uncomitted button ( relay is not affected by button )
* Startup behaviour of the button stored in EEPROM
* More status shown using the LED ( using different flashing patterns )
* Possibility to disable factory reset ( return to configuration mode ) using the button when ( When MQTT connected  )

## Features:
* ON/OFF relay
* Timer based relay
* Configurable default (on boot) relay state - (MQTT independent)
* Local button on/off
* Keepalive feature - device will reboot if not receive keepalive message in given time
* Reverse mode - ON command means relay off, OFF command means relay on (configurable on Homie level)
* Watchdog timer
* All Homie buildin features (OTA,configuration)

## Build and upload
 * Attache module via USB-RS232 adapter
 * Set proper serial port number in plantformio.ini file (upload_port variable)
 * reboot module into program mode
 * Flash module:
   * execute <code>pio run --target upload --environment sonoff</code> for SONOFF, <code>pio run --target upload --environment sonoffs20</code> for SONOFF S20 or <code>pio run --target upload --environment generic</code> for generic ESP8266
   * In Atom editor with PlatformIO prees F7 enter environment name (sonoff or generic) and choose <code>PIO uload</code> option from the list

## MQTT messages

<table>
<tr>
  <th>Property</th>
  <th>Message format</th>
  <th>Direction</th>
  <th>Description</th>
</tr>
<tr>
  <td>_HOMIE_PREFIX_/_node-id_/relay01/relayState</td>
  <td><code>(ON|OFF)</code></td>
  <td>Device → Controller</td>
  <td>Current state of relay</td>
</tr>
<tr>
  <td>_HOMIE_PREFIX_/_node-id_/relay01/relayState/set</td>
  <td><code>(ON|OFF)</code></td>
  <td>Controller → Device</td>
  <td>Change relay state</td>
</tr>
<tr>
  <td>_HOMIE_PREFIX_/_node-id_/relay01/relayTimer/set</td>
  <td><code>\d+</code></td>
  <td>Controller → Device</td>
  <td>Turn on relay for specific no. of seconds</td>
</tr>
<tr>
<td>_HOMIE_PREFIX_/_node-id_/watchdog/timeOut/set</td>
<td><code>\d+</code></td>
<td>Controller → Device</td>
<td>Time after witch watchdog disable/enable output fo 10s</td>
</tr>
<td>_HOMIE_PREFIX_/_node-id_/watchdog/tick/set</td>
<td><code>\d+</code></td>
<td>Controller → Device</td>
<td>Tick for watchdog - renew timeout of reset</td>
</tr>
<tr>
  <td>_HOMIE_PREFIX_/_node-id_/keepalive/timeOut</td>
  <td><code>\d+</code></td>
  <td>Device → Controller</td>
  <td>Report of keepalive timeout value (seconds), 0 - keep alive feature is not active</td>
</tr>
<tr>
  <td>_HOMIE_PREFIX_/_node-id_/keepalive/timeOut/set</td>
  <td><code>\d+</code></td>
  <td>Controller → Device</td>
  <td>Set Report of keepalive timeout in seconds, 0 - keep alive feature is not active</td>
</tr>
<tr>
  <td>_HOMIE_PREFIX_/_node-id_/keepalive/tick/set</td>
  <td><code>.*</code></td>
  <td>Controller → Device</td>
  <td>Keepalive message from controller to gateway - if device will not receive during keepAliveTimeOut time slot, it will reboot, keepalive is not active when keepAliveTimeOut is equal 0</td>
</tr>
<tr>
  <td>_HOMIE_PREFIX_/_node-id_/$online</td>
  <td><code>(true|false)</code></td>
  <td>Device → Controller</td>
  <td><code>/true</code> when the device is online, <code>false</code> when the device is offline (through LWT)</td>
</tr>
</table>
