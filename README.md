# QR Studio

A mobile-friendly QR code creator, customizer and scanner, built as a static web app for GitHub Pages.

## Features
- Generate QR codes from URLs and plain text.
- Customize foreground and background colours.
- Download QR codes as PNG or SVG.
- Scan QR codes with a device camera or from an image.
- Keep a local QR history on the current device.
- Upload files to Supabase Storage and create QR codes for public share links.
- Installable PWA with a basic offline app shell.

## Deploy on GitHub Pages
1. Open **Settings → Pages** in this repository.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Select `main` and `/(root)`, then Save.
4. Wait for the Pages URL to appear: `https://conrad-2010.github.io/qr-studio/`.

## Enable file uploads (Supabase)
GitHub Pages is static hosting; it does not store user uploads. To enable file uploads:

1. Create a project at [Supabase](https://supabase.com/).
2. In **Storage**, create a bucket named `qr-uploads`.
3. Set the bucket to **Public** if you want anyone with the QR link to view/download files. Public files can be accessed by anyone who has the URL.
4. In **Project Settings → API**, copy the Project URL and the **anon/publishable** key.
5. Open QR Studio → **Storage setup**, enter the URL and key, leave the bucket as `qr-uploads`, and save.
6. Add Storage policies that allow the intended uploads. An INSERT policy for the `anon` role on `storage.objects` scoped to bucket_id = 'qr-uploads' is required for anonymous uploads. Do not expose a service-role/secret key in this frontend.

**Security note:** A public bucket means uploaded files are readable by anyone who obtains the public URL. For private uploads, use authenticated users and signed URLs from a trusted backend instead. Client-side file size checks are convenience checks, not a security boundary; enforce limits in Supabase too.

## Notes
- QR history is stored in browser localStorage, so it is specific to this browser/device and can be cleared by the user.
- Camera scanning requires HTTPS and browser camera permission.
- QR codes store the destination text/link; they do not embed the uploaded file itself.
- External CDN libraries require an internet connection for first load. The PWA caches the app shell, but QR generation/scanning libraries may need internet access.
