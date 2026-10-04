# Control Board - releases

Installers and update files for **Control Board**, a native Windows control
board for Proxmox homelabs. The source is private; this repository holds only
signed builds.

**Status: private alpha.** If you were invited to test it:

1. Download the newest `Control.Board_<version>_x64-setup.exe` from
   [Releases](../../releases/latest).
2. Run it. Windows SmartScreen will warn that the publisher is unknown (the
   installer is not code-signed yet): choose **More info > Run anyway**.
3. Control Board opens on **Nothing is set up yet**: use **Add a source**.

Updates arrive inside the app. Each one is signed, and the app refuses an
update whose signature does not match.

Found a bug? Open an [issue](../../issues) with what you did, what you
expected and what happened. Please leave out passwords, tokens and anything
else private: issues here are public.
