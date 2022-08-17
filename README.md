# DOD Root CA and CAC software) installer

## For Linux systems.

The provided script uses Red Hat Ansible, so first you need to install that...
If you have the EPEL repos enabled, just do:
``` bash
sudo yum install -y ansible
```
Otherwise, install with PIP:
``` bash
python3 -m pip install --user --upgrade pip
pip3 install --user ansible
```

Then clone this repo and run the script like this:
``` bash
ansible-playbook --ask-become-pass ./dod-root-ca-and-cac-installer.yml
```

Enter your sudo password when prompted.

Restart your web browser, or reboot.



## For Windows

Download the InstallRoot Windows Installer from DOD Cyber Exchange, using the search bar to filter for "InstallRoot"...

<https://public.cyber.mil/pki-pke/tools-configuration-files/>

Direct download link for the admin installer:

<https://dl.dod.cyber.mil/wp-content/uploads/pki-pke/msi/InstallRoot_5.5x64.msi>

Direct download link to the non-admin installer:

<https://dl.dod.cyber.mil/wp-content/uploads/pki-pke/msi/InstallRoot_5.5x32_NonAdmin.msi>

Restart your web browser, or reboot.



## Browser-specific settings

If you don't have permissions to install the root CA system-wide, you can at least import certificates for your web browser.
Go to the Cyber Exchange PKI-PKE home page and click on "New DOD PKI CA Certificates Bundle".

<https://public.cyber.mil/pki-pke/>

Direct download link:

<https://dl.dod.cyber.mil/wp-content/uploads/pki-pke/zip/certificates_pkcs7_DoD.zip>

Unpack the Zip file, and import the contained certificates in each web browser you use.

- Mozilla Firefox
    1. In the top-right of the window, click on the ≡ menu > Settings.
    1. In the left side-bar, click on Privacy & Security.
    1. Scroll down to the Certificates section, and click View Certificates...
    1. On the Authorities tab, click the Import button.
    1. You probably only need the `.pem` file, but I just attempt to import all certificates (one at a time) just in case.
       This is redundant and the browser will mention some certificates were already imported with one of the previous files.
    1. Restart your browser and/or reboot.
- Google Chrome
    1. In the top-right of the window, click on the ⫶ menu > Settings.
    1. In the left side-bar click on Privacy and Security.
    1. In the Privacy and Security section, click on the Security row.
    1. Scroll down and click on Manage Certificates.
    1. On the Authorities tab, click the Import button.
    1. You probably only need the `.pem` file, but I just attempt to import all certificates (one at a time) just in case.
       This is redundant and the browser will mention some certificates were already imported with one of the previous files.
    1. Restart your browser and/or reboot.
- Microsoft Edge
    1. In the top-right of the window, click on the ⋯ menu > Settings.
    1. In the left side-bar, click on Privacy, Search, and Services.
    1. Scroll down to the Security section, and click on Manage Certificates.
    1. On the Trusted Root Certificate Authorities tab, click on Import...
    1. You probably only need the `.pem` file, but I just attempt to import all certificates (one at a time) just in case.
       This is redundant and the browser will mention some certificates were already imported with one of the previous files.
    1. Restart your browser and/or reboot.
