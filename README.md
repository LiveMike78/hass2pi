# hass2pi
Home Assistant and AVEVA PI integration

Node-RED - hass2pi - function
Transfer event data from Home Assistant to AVEVA PI using Node-RED, MQTT, and the Adapter for MQTT. In Node-RED import the function. Connect an "Events: all" Home Assistant node into the function, connect the output to an MQTT out node.

Asset Framework - Enumerations Sets - Home Assistant Binary Sensors
Enumation set (xml) to be imported to Asset Framework, provides binary states for on/off sensors: (default), battery, battery charging, cold, connectivity, door, door (inverted), garage door, gas, heat, light, lock, moisture, moving, occupancy, opening, plug, power, presence, problem, running, safety, smoke, vibration, and window. See https://www.home-assistant.io/integrations/binary_sensor/
