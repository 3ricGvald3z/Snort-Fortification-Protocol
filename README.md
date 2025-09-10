# Snort Fortification Protocol

EXECUTION LOG
>
>      /\_/\
>     ( o.o )
>      > ^ <
>

> // ACCESS GRANTED

## About the Script: A Digital Sentry

This script is your first step in deploying a formidable digital sentry: Snort 3. It automates the installation and initial configuration of Snort, transforming your machine into a vigilant **Intrusion Detection System (IDS)** or a proactive **Intrusion Prevention System (IPS)**. No more manual compilation or hunting down dependencies—this script handles the heavy lifting, allowing you to focus on the real mission: defending your network.

## IDS vs. IPS: A Critical Distinction

**The Default Payload is Passive.** This script's initial configuration is designed for maximum safety and reconnaissance. It deploys Snort in **IDS mode**, a passive state where it acts like a silent observer. It listens for malicious traffic and logs any detected breaches, but it **will not block or interfere with packets.** This is the safest way to begin your mission, allowing you to analyze threat vectors without the risk of disrupting legitimate network traffic.

**This script does not activate IPS mode by default.** To fortify your perimeter and transition to an active defense, you must manually activate the IPS payload.

## Fortifying the Perimeter: Activating IPS

When you've analyzed the intelligence gathered in IDS mode and are ready to deploy an active countermeasure, follow these steps to switch to **IPS mode**, which will actively drop malicious packets.

1.  **Modify the `snort.lua` Configuration File:** Uncomment the `ips` configuration block in `/usr/local/etc/snort/snort.lua` to enable inline mode. This tells Snort to actively manage traffic.
    
    ```
    ips = {
        daq = { mode = 'inline' },
        mode = 'ips'
    }
    
    ```
    
2.  **Adjust the Systemd Service Payload:** Modify the systemd service file at `/etc/systemd/system/snort.service`. Add the `-Q` flag to the `ExecStart` line. This command tells Snort to run in inline mode.
    
    ```
    ExecStart=/usr/local/bin/snort -c ${SNORT_CONF_PATH} -i ${IFACE} -A full -q -Q --daq-dir /usr/local/lib/snort_extra/ --daq afpacket --pid-path /var/run -D
    
    ```
    

After making these changes, reload the systemd daemon (`sudo systemctl daemon-reload`) and restart the Snort service (`sudo systemctl restart snort`) to deploy your newly fortified IPS.

### Usage

1.  Clone this repository to your target system.
    
2.  Make the script executable: `chmod +x setup_snort.sh`
    
3.  Execute the script with root privileges: `sudo ./setup_snort.sh`
    

### Disclaimer

This script is an offensive-minded tool for defensive purposes. Use it to protect networks you are authorized to defend. Always test in a controlled environment before deploying to a live network.
