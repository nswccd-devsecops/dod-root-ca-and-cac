# DOD Root CA and CAC software installer

Ideally, you should install the root CA certificates system-wide on your machine; we have separate instructions below for Windows and Linux.
If you don't have administrator privileges on your machine, though, you can at least import the certificates into your web browser, as described in the third section.



## For Linux systems

### Red Hat family of distributions

#### Root CA

To install the DOD root CA certificates, you'll also need to install a few certificate management tools:

``` bash
sudo dnf install -y nss-tools openssl
```

See the latest "PKI CA Certificate bundle" for DOD PKI, here:
<https://public.cyber.mil/pki-pke/tools-configuration-files/>.
The following commands will install the certificates for you, but if Cyber Exchange bumps the version numbers or otherwise renames the files in the Zip archive, you might need to adjust the commands' file paths slightly.

``` bash
cd ~/Downloads/
wget https://dl.dod.cyber.mil/wp-content/uploads/pki-pke/zip/unclass-certificates_pkcs7_DoD.zip
unzip ./unclass-certificates_pkcs7_DoD.zip
cd ./certificates_pkcs7_v5_13_dod/
sudo openssl pkcs7 -print_certs -inform der -in ./certificates_pkcs7_v5_13_dod_der.p7b -out /etc/pki/ca-trust/source/anchors/DoD_Certs.pem
sudo update-ca-trust

mkdir -p "$HOME/.pki/nssdb"
chmod 'u=rwX,g=,o=' --recursive "$HOME/.pki/nssdb/"
sudo mkdir -p /etc/pki/nssdb
for cert in ./*.p7b
do
    certutil -A -d "sql:${HOME}/.pki/nssdb/" -t TC -n "DOD CA $( basename $cert .p7b )" -i "$cert"
    sudo certutil -A -d "sql:/etc/pki/nssdb/" -t TC -n "DOD CA $( basename $cert .p7b )" -i "$cert"
done
```

And if you use any Java applications,
``` bash
keytool -importcert -file /etc/pki/ca-trust/source/anchors/DoD_Certs.pem -cacerts -keypass changeit -storepass changeit -noprompt -alias dod-root-ca
```

Continue to install CAC software below, before rebooting.

#### CAC

Fedora users can stick with the distro-default repos, but \*Enterprise Linux users will need to enable the
[Fedora EPEL repo](https://docs.fedoraproject.org/en-US/epel/),
if you don't have it already.

``` bash
# For EL8...
sudo dnf install -y "https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm"
# For EL9...
sudo dnf install -y "https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm"
```

And if you're on CentOS Stream, then also enable EPEL Next:

``` bash
sudo dnf install -y epel-next-release
```

After that you can install CAC software like any other package:

``` bash
sudo dnf install -y esc opensc pcsc-lite pcsc-tools
sudo systemctl enable --now pcscd.service
```

You can test to see if the computer properly recognizes your inserted CAC by running
``` bash
pcsc_scan
```

It should fairly quickly list a reader, and card attributes, something like
```
 Reader 0: Broadcom Corp xxxxx [Contacted SmartCard] (xxxxxxxxxxxxxx) 00 00
  Card state: Card inserted, Shared Mode,
  ATR: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX

ATR: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX
+ TS = ...
+ T0 = ...
  TA(1) = ...
```
Hit <kbd>Ctrl</kbd>+<kbd>C</kbd> to end the scan.

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
    1. You probably only need the certificate chain file `certificates_pkcs7_v5_13_dod_der.p7b`, but Cyber Exchange occasionally changes how the files are formatted, so I just attempt to import all certificates (one at a time) just in case.
       This is redundant and the browser will mention some certificates were already imported with one of the previous files.
       Click OK to close the dialog when you're done.
    1. On Linux,if you're manually importing the DOD root CA, you may also need to import the CAC software module:
        1. Back on Settings > Privacy & Security > Certificates, also click on Security Devices.
        1. Click Load.
        1. Enter something descriptive like "OpenSC CAC module" in the Module Name field.
        1. Copy/paste or navigate to the path `/usr/lib64/opensc-pkcs11.so` for Module Filename.
        1. Click OK to close all dialogs.
    1. Restart your browser and/or reboot.
- Google Chrome
    1. In the top-right of the window, click on the ⫶ menu > Settings.
    1. In the left side-bar click on Privacy and Security.
    1. In the Privacy and Security section, click on the Security row.
    1. Scroll down and click on Manage Certificates.
    1. On the Authorities tab, click the Import button.
    1. You probably only need the certificate chain file `certificates_pkcs7_v5_13_dod_der.p7b`, but Cyber Exchange occasionally changes how the files are formatted, so I just attempt to import all certificates (one at a time) just in case.
       This is redundant and the browser will mention some certificates were already imported with one of the previous files.
    1. Restart your browser and/or reboot.
- Microsoft Edge
    1. In the top-right of the window, click on the ⋯ menu > Settings.
    1. In the left side-bar, click on Privacy, Search, and Services.
    1. Scroll down to the Security section, and click on Manage Certificates.
    1. On the Trusted Root Certificate Authorities tab, click on Import...
    1. You probably only need the certificate chain file `certificates_pkcs7_v5_13_dod_der.p7b`, but Cyber Exchange occasionally changes how the files are formatted, so I just attempt to import all certificates (one at a time) just in case.
       This is redundant and the browser will mention some certificates were already imported with one of the previous files.
    1. Restart your browser and/or reboot.
