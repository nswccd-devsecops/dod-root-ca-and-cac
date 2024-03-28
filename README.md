# DOD Root CA and CAC software installer

Ideally, you should install the root CA certificates system-wide on your machine; we have separate instructions below for Windows and Linux.
If you don't have administrator privileges on your machine, though, you can at least import the certificates into your web browser, as described in the third section.



## For Linux systems

The provided scripts use Red Hat Ansible, so first you need to install that...
Get the latest build for your version of Python, with PIP.
(Installing/upgrading PIP itself and some related tools _first_ will make installing Ansible go more smoothly.)

``` bash
sudo yum install -y python3 python3-pip
python3 -m pip install --user --upgrade pip setuptools virtualenv wheel
python3 -m pip install --user --upgrade ansible
```

If you have leftover copies of the certificates bundle from a previous installation, delete them before proceeding.

``` bash
sudo rm -rf /tmp/certs/
```

Then you can clone this repo and run the script like this:
``` bash
git clone "https://github.com/nswccd-devsecops/dod-root-ca-and-cac"
cd ./dod-root-ca-and-cac/
ansible-playbook --ask-become-pass ./dod-root-ca-and-cac-installer.yml
```

Don't run this command with `sudo`, but enter your sudo password when prompted to "become".
**Reboot.**



## For Windows

Download the InstallRoot installer for Windows from DOD Cyber Exchange, using the search bar in the Tools section of the page to filter for "InstallRoot Windows".

<https://public.cyber.mil/pki-pke/tools-configuration-files/>

There you should see both

- [InstallRoot 5.6 NIPR 64-bit Windows Installer ](https://dl.dod.cyber.mil/wp-content/uploads/pki-pke/msi/InstallRoot_5.6x64.msi)
  (if you are **admin** on your machine)
- and [InstallRoot 5.6 NIPR **Non-Administrator** 64-bit Windows Installer](https://dl.dod.cyber.mil/wp-content/uploads/pki-pke/msi/InstallRoot_5.6x64_NonAdmin.msi)

Run the installer, follow the instructions for "Install DoD Certificates" at first launch, and then **reboot**.

If you don't have the necessary permissions to run even the non-admin installer, ideally you should submit a ticket to your help desk, asking them to run the latest InstallRoot (or otherwise add DOD Root CA 3) for you...
But you could import into your browser in the meanwhile, in the next section.



## Browser-specific settings on desktop/laptop

If you don't have permissions to properly install the root CA at all, you can at least import certificates into your web browser.
Go to the Cyber Exchange PKI-PKE home page and click on "New DoD PKI CAs released".

<https://public.cyber.mil/pki-pke/>

Direct download link:

<https://dl.dod.cyber.mil/wp-content/uploads/pki-pke/zip/unclass-certificates_pkcs7_DoD.zip>

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
