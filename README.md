# DOD Root CA and CAC software) installer

## For Linux systems (preferably RHEL-based).

Clone this repo, then in a terminal, run

``` bash
ansible-playbook --ask-become-pass ./dod-root-ca-and-cac-installer.yml
```

Enter your sudo password when prompted.



## For windows

Download InstallRoot Windows Installer from DOD Cyber Exchange and run as admin.

<https://public.cyber.mil/pki-pke/tools-configuration-files/>

Directly download link:

<https://dl.dod.cyber.mil/wp-content/uploads/pki-pke/msi/InstallRoot_5.5x64.msi>
