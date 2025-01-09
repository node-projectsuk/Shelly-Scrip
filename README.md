### Description of the Code: MQTT-Controlled Relay with Failover

This script is designed to control a relay connected to a Shelly device based on input states, with failover functionality to handle MQTT disconnections. The behavior is structured to work seamlessly during MQTT connection and disconnection events, ensuring robust operation and clear startup/reconnection handling.

---

### Core Functionality

1. **Startup Behavior**:
   - When the device starts up, the relay is initialized to the **OFF** state.
   - This ensures the system starts in a predictable and safe state.

2. **Relay Activation**:
   - During normal MQTT operation, the relay remains OFF until the **first input change**.
   - When the input changes state (e.g., a switch is toggled), the relay is activated (set to ON) and remains ON as long as the MQTT connection is stable.

3. **Relay Persistence**:
   - Once the relay is turned ON, it stays ON until one of the following occurs:
     - The device is **power-cycled**.
     - The **MQTT broker connection is lost**.

4. **Failover Mode**:
   - If the connection to the MQTT broker is lost, the system enters **failover mode** after a 5-second delay.
   - In failover mode, the input switch directly controls the relay (e.g., when the switch is ON, the relay is ON, and vice versa).

5. **Reconnection Handling**:
   - When the MQTT broker reconnects, the system resets to the startup state:
     - The relay is turned OFF.
     - The system waits for the first input change to turn the relay ON again.

---

### Key Concepts in the Code

1. **MQTT Connection Monitoring**:
   - The script tracks the connection status of the MQTT broker using event handlers.
   - Events for `connected` and `disconnected` are used to update the `mqtt_connected` variable.

2. **Failover Handling**:
   - A 5-second timer checks if the MQTT connection remains disconnected. If so, failover mode is activated.
   - In failover mode, input state changes directly control the relay using `Switch.Set`.

3. **Relay State Management**:
   - The relay state is tracked using a variable (`output_state`), ensuring consistent updates and proper toggling during failover.

4. **Reconnection Logic**:
   - When the MQTT connection is re-established, the failover mode is disabled.
   - The relay is reset to OFF, and the system waits for the first input change to activate the relay.

5. **Event Handlers**:
   - Input changes (`input:0`) and MQTT connection events (`mqtt`) are handled through event handlers to manage relay behavior dynamically.

6. **Timers**:
   - A periodic timer checks the MQTT connection status to ensure failover is activated or disabled as needed.
