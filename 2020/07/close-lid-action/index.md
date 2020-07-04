# Close Lid Action

Preventing the laptop from suspending when closing the lid.
<!--more-->

When closing the lid of a laptop which will suspend in order to save power.
We can prevent the laptop from suspending when closing the lid by changing
the setting for that behavior.
### Configuring the lid switch

1. Open the /etc/systemd/logind.conf file for editing.
2. Find the **HandleLidSwitch=suspend** and **HandleLidSwitchDocked=suspend** lines in the file.  
 If it is quoted out with the # character at the start, unquote it.  
If the lines are not present in the file, add it.

3. Replace the default suspend parameter with
      - **lock** for the screen to lock;
      - **ignore** for nothing to happen;
      - **poweroff** for the laptop to switch off.

    For example:
    ```
    [Login]
    HandleLidSwitch=ignore        <---- Set both of these
    HandleLidSwitchDocked=ignore  <---- to ignore lid events.
    ```

4. Save your changes and close the editor.
5. Run the following command so that your changes preserve the next restart of the system:
    ```
    # systemctl restart systemd-logind.service
    ```
For more information on the /etc/systemd/logind.conf file,
see the logind.conf(5) man page.

{{< admonition note >}}
**HandleLidSwitchDocked** be introduced after Centos 8.2.
{{< /admonition >}}

