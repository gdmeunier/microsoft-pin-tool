# Microsoft PIN Tool
The **Microsoft PIN Tool** modified for ActivClient-initialized PKI smartcards (HID Global cards with a static unblock code).

## What ActivClient is
It's a PKI middleware developed by *ASSA ABLOY* for HID Global.
HID Global recommends it for all their Crescendo line of PKI products (Crescendo cards + Crescendo keys).

**ActivClient** is also the only tool that cans provide a *PKCS11* interface to Crescendo smartcards, all public *acpkcs11.dll* files found on the HID Global website will fail to load under PKCS11 apps such as *XCA*, *Pkcs11Admin* , *BestCrypt* & *VeraCrypt*.

And additionally again, **ActivClient** is the only tool able to generate the *msroots* file on Crescendo smartcards' PKCS15 filesystems.

## How ActivClient unblocks cards
ActivClient is a strange beast, since if you initialize a Crescendo card with it it will:
- disable the Admin user,
- disable XAUTH / *Unblock using Admin key* feature and
- generate a **static unblock code** instead of a *DESede challenge/response* unblock.

Thus if you have an ActivClient-initialized card, then you can't unblock it anymore with third-party smartcard tools.
Not even VersaSec's *vSEC_TOOL_K* or *vSEC_CMS_K* will be able to unblock it because they expect a challenge/response procedure.

The same goes for the **Microsoft PIN Tool**, part of Windows XP's *Base Smart Card Cryptographic Service Provider* update ([KB909520](https://www.catalog.update.microsoft.com/Search.aspx?q=KB909520)).

## Modifying the Microsoft PIN Tool
I modified the Microsoft PIN Tool by adding/modifying the following features:
- I added a nice-looking EXE file icon from MySmartLogon's *EIDAuthenticate* software.
- I hid & disabled the challenge request UI elements of the main window
- I patched the **wpintool.exe** file to automatically re-enable the disabled-on-startup UI elements for unblocking the card
- I played around with Microsoft's **MUIRCT.exe** translation utility and generated French translations for both the original & ActivID versions of Microsoft's PIN Tool

## Last Note
It works! I successfully unblocked my ActivClient-initialized Crescendo card (ActivClient v7) with my ActivID-modded **wpintool.exe** file.
Just consider that your static *unblock code* is always the Response code to unblock the card and set a new User PIN.

No need to use ActivClient anymore for unblocking Crescendo cards.
